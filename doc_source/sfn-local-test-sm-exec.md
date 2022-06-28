# Using Mocked Service Integrations<a name="sfn-local-test-sm-exec"></a>

In Step Functions Local, you can test the execution paths of your state machines without actually calling integrated services by using mocked service integrations\. To configure your state machines to use mocked service integrations, you create a mock configuration file\. In this file, you define the desired output of your service integrations as mocked responses and the executions which use your mocked responses to simulate an execution path as test cases\.

By providing the mock configuration file to Step Functions Local, you can test service integration calls by running state machines that use the mocked responses specified in the test cases instead of making actual service integration calls\.

**Note**  
 If you don't specify mocked service integration responses in the mock configuration file, Step Functions Local will invoke the AWS service integration using the endpoint you configured while setting up Step Functions Local\. For information about configuring endpoints for Step Functions Local, see [Setting Configuration Options for Step Functions Local](sfn-local-config-options.md)\. 

**Topics**
+ [Key concepts in this topic](#mock-serv-integ-concepts)
+ [Step 1: Specify Mocked Service Integrations in a Mock Configuration File](#create-mock-config-file)
+ [Step 2: Provide the Mock Configuration File to Step Functions Local](#supply-mock-config-file)
+ [Step 3: Run Mocked Service Integration Tests](#run-mocked-serv-integ-tests)
+ [Configuration File for Mocked Service Integrations](sfn-local-mock-cfg-file.md)

## Key concepts in this topic<a name="mock-serv-integ-concepts"></a>

This topic uses several concepts which are defined in the following list:
+ Mocked Service Integrations \- Refers to Task states configured to use mocked responses instead of performing actual service calls\.
+ Mocked Responses \- Refers to mock data that Task states can be configured to use\.
+ Test Cases \- Refers to state machine executions configured to use mocked service integrations\.
+ Mock Configuration File \- Refers to mock configuration file that contains JSON, which defines mocked service integrations, mocked responses, and test cases\.

## Step 1: Specify Mocked Service Integrations in a Mock Configuration File<a name="create-mock-config-file"></a>

You can test Step Functions AWS SDK and optimized service integrations using Step Functions Local\. The following image shows the state machine defined in the State machine definition tab:

![\[Mocked service integration example.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/msi-graph.png)

To do this, you must create a mock configuration file containing sections as defined in [Introducing structure of mock configuration](sfn-local-mock-cfg-file.md#mock-cfg-struct)\.

1. Create a file named `MockConfigFile.json` to configure tests with mocked service integrations\.

   The following example shows a mock configuration file referencing a state machine with two defined states named `LambdaState` and `SQSState`\.

------
#### [ Mock configuration file example ]

   The following is an example of a mock configuration file which demonstrates how to mock responses from [invoking a Lambda function](connect-lambda.md) and [sending a message to Amazon SQS](connect-sqs.md)\. In this example, the [`LambdaSQSIntegration`](sfn-local-mock-cfg-file.md#mock-cfg-sm-sect) state machine contains three test cases named `HappyPath`, `RetryPath`, and `HybridPath` which mock the `Task` states named `LambdaState` and `SQSState`\. These states use the `MockedLambdaSuccess`, `MockedSQSSuccess`, and `MockedLambdaRetry` mocked service responses\. These mocked service responses are defined in the `MockedResponses` section of the file\.

   ```
   {
     "StateMachines":{
       "LambdaSQSIntegration":{
         "TestCases":{
           "HappyPath":{
             "LambdaState":"MockedLambdaSuccess",
             "SQSState":"MockedSQSSuccess"
           },
           "RetryPath":{
             "LambdaState":"MockedLambdaRetry",
             "SQSState":"MockedSQSSuccess"
           },
           "HybridPath":{
             "LambdaState":"MockedLambdaSuccess"
           }
         }
       }
     },
     "MockedResponses":{
       "MockedLambdaSuccess":{
         "0":{
           "Return":{
             "StatusCode":200,
             "Payload":{
               "StatusCode":200,
               "body":"Hello from Lambda!"
             }
           }
         }
       },
       "LambdaMockedResourceNotReady":{
         "0":{
           "Throw":{
             "Error":"Lambda.ResourceNotReadyException",
             "Cause":"Lambda resource is not ready."
           }
         }
       },
       "MockedSQSSuccess":{
         "0":{
           "Return":{
             "MD5OfMessageBody":"3bcb6e8e-7h85-4375-b0bc-1a59812c6e51",
             "MessageId":"3bcb6e8e-8b51-4375-b0bc-1a59812c6e51"
           }
         }
       },
       "MockedLambdaRetry":{
         "0":{
           "Throw":{
             "Error":"Lambda.ResourceNotReadyException",
             "Cause":"Lambda resource is not ready."
           }
         },
         "1-2":{
           "Throw":{
             "Error":"Lambda.TimeoutException",
             "Cause":"Lambda timed out."
           }
         },
         "3":{
           "Return":{
             "StatusCode":200,
             "Payload":{
               "StatusCode":200,
               "body":"Hello from Lambda!"
             }
           }
         }
       }
     }
   }
   ```

------
#### [ State machine definition ]

   The following is an example of a state machine definition called `LambdaSQSIntegration`, which defines two service integration task states named `LambdaState` and `SQSState`\. `LambdaState` contains a retry policy based on `States.ALL`\.

   ```
   {
     "Comment":"This state machine is called: LambdaSQSIntegration",
     "StartAt":"LambdaState",
     "States":{
       "LambdaState":{
         "Type":"Task",
         "Resource":"arn:aws:states:::lambda:invoke",
         "Parameters":{
           "Payload.$":"$",
           "FunctionName":"HelloWorldFunction"
         },
         "Retry":[
           {
             "ErrorEquals":[
               "States.ALL"
             ],
             "IntervalSeconds":2,
             "MaxAttempts":3,
             "BackoffRate":2
           }
         ],
         "Next":"SQSState"
       },
       "SQSState":{
         "Type":"Task",
         "Resource":"arn:aws:states:::sqs:sendMessage",
         "Parameters":{
           "QueueUrl":"https://sqs.us-east-1.amazonaws.com/123456789012/myQueue",
           "MessageBody.$":"$"
         },
         "End": true
       }
     }
   }
   ```

------

   You can run the `LambdaSQSIntegration` state machine definition referenced in the mock configuration file using one of the following test cases:
   + `HappyPath` \- This test mocks the output of `LambdaState` and `SQSState` using `MockedLambdaSuccess` and `MockedSQSSuccess` respectively\.
     + The `LambdaState` will return the following value:

       ```
       "0":{
         "Return":{
           "StatusCode":200,
           "Payload":{
             "StatusCode":200,
             "body":"Hello from Lambda!"
           }
         }
       }
       ```
     + The `SQSState` will return the following value:

       ```
       "0":{
         "Return":{
           "MD5OfMessageBody":"3bcb6e8e-7h85-4375-b0bc-1a59812c6e51",
           "MessageId":"3bcb6e8e-8b51-4375-b0bc-1a59812c6e51"
         }
       }
       ```
   + `RetryPath` \- This test mocks the output of `LambdaState` and `SQSState` using `MockedLambdaRetry` and `MockedSQSSuccess` respectively\. In addition, `LambdaState` is configured to perform four retry attempts\. The mocked responses for these attempts are defined and indexed in the `MockedLambdaRetry` state\.
     + The initial attempt ends with a task failure containing a cause and error message as shown in the following example:

       ```
       "0":{
         "Throw": {
           "Error": "Lambda.ResourceNotReadyException",
           "Cause": "Lambda resource is not ready."
         }
       }
       ```
     + The first and second retry attempts end with a task failure containing a cause and error message as shown in the following example:

       ```
       "1-2":{
         "Throw": {
           "Error": "Lambda.TimeoutException",
           "Cause": "Lambda timed out."
         }
       }
       ```
     + The third retry attempt ends with a task success containing state result from Payload section in the mocked Lambda response\.

       ```
       "3":{
         "Return": {
           "StatusCode": 200,
           "Payload": {
             "StatusCode": 200,
             "body": "Hello from Lambda!"
           }
         }
       }
       ```
**Note**  
For states with a retry policy, Step Functions Local will exhaust the retry attempts set in the policy until it receives a success response\. This means that you must denote mocks for retries with consecutive attempt numbers and should cover all the retry attempts before returning a success response\. 
If you do not specify a mocked response for a specific retry attempt, for example, retry "3", the state machine execution will fail\.
   + `HybridPath` \- This test mocks the output of `LambdaState`\. After `LambdaState` runs successfully and receives mocked data as a response, `SQSState` performs an actual service call to the resource specified in production\.

   For information about how to start test executions with mocked service integrations, see [Step 3: Run Mocked Service Integration Tests](#run-mocked-serv-integ-tests)\.

1. Make sure that the mocked responses' structure conforms to the structure of actual service responses you receive when you make integrated service calls\. For information about the structural requirements for mocked responses, see [Configuring mocked service integrations](sfn-local-mock-cfg-file.md#mock-resp-struct-req)\.

   In the previous example mock configuration file, the mocked responses defined in `MockedLambdaSuccess` and `MockedLambdaRetry` conform to the structure of actual responses that are returned from calling `HelloFromLambda`\.
**Important**  
AWS service responses can vary in structure between different services\. Step Functions Local doesn't validate if mocked response structures conform to actual service response structures\. You must ensure that your mocked responses conform to actual responses before testing\. To review the structure of service responses, you can either perform the actual service calls using Step Functions or view the documentation for those services\.

## Step 2: Provide the Mock Configuration File to Step Functions Local<a name="supply-mock-config-file"></a>

 You can provide the mock configuration file to Step Functions Local in one of the following ways: 

------
#### [ Docker ]

**Note**  
If you're using the Docker version of Step Functions Local, you can provide the mock configuration file using an environment variable only\. In addition, you must mount the mock configuration file onto the Step Functions Local container at the initial server boot\-up\.

Mount the mock configuration file onto any directory within the Step Functions Local container\. Then, set an environment variable named `SFN_MOCK_CONFIG` that contains the path to the mock configuration file in the container\. This method enables the mock configuration file to be named anything as long as the environment variable contains the file path and name\. 

The following command shows the format to start the Docker image\.

```
docker run -p 8083:8083 
--mount type=bind,readonly,source={absolute path to mock config file},destination=/home/StepFunctionsLocal/MockConfigFile.json 
-e SFN_MOCK_CONFIG="/home/StepFunctionsLocal/MockConfigFile.json" amazon/aws-stepfunctions-local
```

The following example uses the command to start the Docker image\.

```
docker run -p 8083:8083 
--mount type=bind,readonly,source=/Users/admin/Desktop/workplace/MockConfigFile.json,destination=/home/StepFunctionsLocal/MockConfigFile.json 
-e SFN_MOCK_CONFIG="/home/StepFunctionsLocal/MockConfigFile.json" amazon/aws-stepfunctions-local
```

------
#### [ JAR File ]

Use one of the following ways to provide the mock configuration file to Step Functions Local:
+ Place the mock configuration file in the same directory as `Step FunctionsLocal.jar`\. When using this method, you must name the mock configuration file `MockConfigFile.json`\. 
+ In the session running Step Functions Local, set an environment variable named `SFN_MOCK_CONFIG`, to the full path of the mock configuration file\. This method enables the mock configuration file to be named anything as long as the environment variable contains its file path and name\. In the following example, the `SFN_MOCK_CONFIG` variable is set to point at a mock configuration file named `EnvSpecifiedMockConfig.json`, located in the `/home/workspace` directory\. 

  ```
  export SFN_MOCK_CONFIG="/home/workspace/EnvSpecifiedMockConfig.json"
  ```

**Note**  
If you do not provide the environment variable `SFN_MOCK_CONFIG` to Step Functions Local, by default, it will attempt to read a mock configuration file named `MockConfigFile.json` in the directory from which you launched Step Functions Local\.
If you place the mock configuration file in the same directory as `Step FunctionsLocal.jar` and set the environment variable `SFN_MOCK_CONFIG`, Step Functions Local will read the file specified by the environment variable\. 

------

## Step 3: Run Mocked Service Integration Tests<a name="run-mocked-serv-integ-tests"></a>

After you create and provide a mock configuration file to Step Functions Local, run the state machine configured in the mock configuration file using mocked service integrations\. Then check the execution results using an API action\.

1. Create a state machine based on the previously mentioned definition in the [mock configuration file](#create-mock-config-file)\.

   ```
   aws stepfunctions create-state-machine \
       --endpoint http://localhost:8083 \
       --definition "{\"Comment\":\"Thisstatemachineiscalled:LambdaSQSIntegration\",\"StartAt\":\"LambdaState\",\"States\":{\"LambdaState\":{\"Type\":\"Task\",\"Resource\":\"arn:aws:states:::lambda:invoke\",\"Parameters\":{\"Payload.$\":\"$\",\"FunctionName\":\"arn:aws:lambda:us-east-1:123456789012:function:HelloWorldFunction\"},\"Retry\":[{\"ErrorEquals\":[\"States.ALL\"],\"IntervalSeconds\":2,\"MaxAttempts\":3,\"BackoffRate\":2}],\"Next\":\"SQSState\"},\"SQSState\":{\"Type\":\"Task\",\"Resource\":\"arn:aws:states:::sqs:sendMessage\",\"Parameters\":{\"QueueUrl\":\"https://sqs.us-east-1.amazonaws.com/123456789012/myQueue\",\"MessageBody.$\":\"$\"},\"End\":true}}}" \
       --name "LambdaSQSIntegration" --role-arn "arn:aws:iam::123456789012:role/service-role/LambdaSQSIntegration"
   ```

1. Run the state machine using mocked service integrations\.

   To use the mock configuration file, make a `[StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html)` API call on a state machine configured in the mock configuration file\. To do this, append the suffix, `#test_name`, to the state machine ARN used by `StartExecution`\. `test_name` is a test case, which is configured for the state machine in the same mock configuration file\.

   The following command is an example that uses the `LambdaSQSIntegration` state machine and mock configuration\. In this example, the `LambdaSQSIntegration` state machine is executed using the `HappyPath` test defined in [Step 1: Specify Mocked Service Integrations in a Mock Configuration File](#create-mock-config-file)\. The `HappyPath` test contains the configuration for the execution to handle mock service integration calls that `LambdaState` and `SQSState` states make using the `MockedLambdaSuccess` and `MockedSQSSuccess` mocked service responses\.

   ```
   aws stepfunctions start-execution \
       --endpoint http://localhost:8083 \
       --name executionWithHappyPathMockedServices \
       --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:LambdaSQSIntegration#HappyPath
   ```

1. View the state machine execution response\.

   The response to calling `StartExecution` using a mocked service integration test is same as the response to calling `StartExecution` normally, which returns the execution ARN and start date\.

   The following is an example response to calling `StartExecution` using the mocked service integration test:

   ```
   {
      "startDate":"2022-01-28T15:03:16.981000-05:00",
      "executionArn":"arn:aws:states:us-east-1:123456789012:execution:LambdaSQSIntegration:executionWithHappyPathMockedServices"
   }
   ```

1. Check the execution's results by making a `[ListExecutions](https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListExecutions.html)`, `[DescribeExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html)`, or `[GetExecutionHistory](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetExecutionHistory.html)` API call\.

   ```
   aws stepfunctions get-execution-history \
       --endpoint http://localhost:8083 \
       --execution-arn arn:aws:states:us-east-1:123456789012:execution:LambdaSQSIntegration:executionWithHappyPathMockedServices
   ```

   The following example demonstrates parts of a response to calling `GetExecutionHistory` using the execution ARN from the example response shown in step 2\. In this example, the output of `LambdaState` and `SQSState` is the mock data defined in `MockedLambdaSuccess` and `MockedSQSSuccess` in the [mock configuration file](#create-mock-config-file)\. In addition, the mocked data is used the same way that data returned by performing actual service integration calls would be used\. Also, in this example, the output from `LambdaState` is passed onto `SQSState` as input\.

   ```
   {
       "events": [
           ...
           {
               "timestamp": "2021-12-02T19:39:48.988000+00:00",
               "type": "TaskStateEntered",
               "id": 2,
               "previousEventId": 0,
               "stateEnteredEventDetails": {
                   "name": "LambdaState",
                   "input": "{}",
                   "inputDetails": {
                       "truncated": false
                   }
               }
           },
           ...
           {
               "timestamp": "2021-11-25T23:39:10.587000+00:00",
               "type": "LambdaFunctionSucceeded",
               "id": 5,
               "previousEventId": 4,
               "lambdaFunctionSucceededEventDetails": {
                   "output": "{\"statusCode\":200,\"body\":\"\\\"Hello from Lambda!\\\"\"}",
                   "outputDetails": {
                       "truncated": false
                   }
               }
           },
           ...
               "timestamp": "2021-12-02T19:39:49.464000+00:00",
               "type": "TaskStateEntered",
               "id": 7,
               "previousEventId": 6,
               "stateEnteredEventDetails": {
                   "name": "SQSState",
                   "input": "{\"statusCode\":200,\"body\":\"\\\"Hello from Lambda!\\\"\"}",
                   "inputDetails": {
                       "truncated": false
                   }
               }
           },
           ...
           {
               "timestamp": "2021-11-25T23:39:10.652000+00:00",
               "type": "TaskSucceeded",
               "id": 10,
               "previousEventId": 9,
               "taskSucceededEventDetails": {
                   "resourceType": "sqs",
                   "resource": "sendMessage",
                   "output": "{\"MD5OfMessageBody\":\"3bcb6e8e-7h85-4375-b0bc-1a59812c6e51\",\"MessageId\":\"3bcb6e8e-8b51-4375-b0bc-1a59812c6e51\"}",
                   "outputDetails": {
                       "truncated": false
                   }
               }
           },
           ...
       ]
   }
   ```