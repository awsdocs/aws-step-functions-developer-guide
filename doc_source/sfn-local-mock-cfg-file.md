# Configuration File for Mocked Service Integrations<a name="sfn-local-mock-cfg-file"></a>

To use mocked service integrations, you must first create a mock configuration file named `MockConfigFile.json` containing your mock configurations\. Then provide Step Functions Local with the mock configuration file\. This configuration file defines test cases, which contain mock states that use mocked service integration responses\. The following section contains information about the structure of mock configuration that includes the mock states and mocked responses:

**Topics**
+ [Introducing structure of mock configuration](#mock-cfg-struct)
+ [Configuring mocked service integrations](#mock-resp-struct-req)

## Introducing structure of mock configuration<a name="mock-cfg-struct"></a>

A mock configuration is a JSON object containing the following top\-level fields:
+ `StateMachines` \- The fields of this object represent state machines configured to use mocked service integrations\.
+ `MockedResponses` \- The fields of this object represent mocked responses for service integration calls\.

The following is an example of a mock configuration file which includes a `StateMachine` definition and `MockedResponses`\.

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

### Mock configuration field reference<a name="mock-cfg-field-ref"></a>

The following sections explain the top\-level object fields that you must define in your mock configuration\.
+ [StateMachines](#mock-cfg-sm-sect) 
+ [MockedResponses](#mock-cfg-mckd-resp-sect) 

#### StateMachines<a name="mock-cfg-sm-sect"></a>

The `StateMachines` object defines which state machines will use mocked service integrations\. The configuration for each state machine is represented as a top\-level field of `StateMachines`\. The field name is the name of the state machine and value is an object containing a single field named `TestCases`, whose fields represent test cases of that state machine\.

The following syntax shows a state machine with two test cases:

```
"MyStateMachine": {
  "TestCases": {
    "HappyPath": {
      ...
    },
    "SadPath": {
      ...
    }
  }
```

##### TestCases<a name="mock-cfg-sm-test-case"></a>

The fields of `TestCases` represent individual test cases for the state machine\. The name of each test case must be unique per state machine and the value of each test case is an object specifying a mocked response to use for Task states in the state machine\.

The following example of a `TestCase` links two `Task` states to two `MockedResponses`:

```
"HappyPath": {
  "SomeTaskState": "SomeMockedResponse",
  "AnotherTaskState": "AnotherMockedResponse"
}
```

#### MockedResponses<a name="mock-cfg-mckd-resp-sect"></a>

`MockedResponses` is an object containing multiple mocked response objects with unique field names\. A mocked response object defines the successful result or error output for each invocation of a mocked Task state\. You specify the invocation number using individual integer strings, such as `"0"`, `"1"`, `"2"`, and `"3"` or an inclusive range of integers, such as `"0\-1"`, `"2\-3`\.

When you mock a Task, you must specify a mocked response for every invocation\. A response must contain a single field named `Return` or `Throw` whose value is the result or error output for the mocked Task invocation\. If you do not specify a mocked response, the state machine execution will fail\.

The following is an example of a `MockedResponse` with `Throw` and `Return` objects\. In this example, the first three times the state machine is run, the response specified in `"0-2"` is returned, and the fourth time the state machine runs, the response specified in `"3"` is returned\.

```
"SomeMockedResponse": {
  "0-2": {
    "Throw": {
      ...
    }
  },
  "3": {
    "Return": {
      ...
    }
  }
}
```

**Note**  
If you are using a `Map` state, and want to ensure predictable responses for the `Map` state, set the value of `maxConcurrency` to `1`\. If you set a value greater than `1`, Step Functions Local will run multiple iterations concurrently, which will cause the overall execution order of states across iterations to be unpredictable\. This may further cause Step Functions Local to use different mocked responses for iteration states from one execution to the next\.

##### Return<a name="mock-cfg-resp-return"></a>

`Return` is represented as a field of the `MockedResponse` objects\. It specifies the successful result of a mocked Task state\.

The following is an example of a `Return` object that contains a mocked response for calling [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html) on a Lambda function:

```
"Return": {
  "StatusCode": 200,
  "Payload": {
    "StatusCode": 200,
    "body": "Hello from Lambda!"
  }
}
```

##### Throw<a name="mock-cfg-resp-throw"></a>

`Throw` is represented as a field of the `MockedResponse` objects\. It specifies the [error output](concepts-error-handling.md) of a failed Task\. The value of `Throw` must be an object containing an `Error` and `Cause` fields with string values\. In addition, the string value you specify in `Error` field in the `MockConfigFile.json` must match the errors handled in the `Retry` and `Catch` sections of your state machine\.

The following is an example of a `Throw` object that contains a mocked response for calling [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html) on a Lambda function:

```
"Throw": {
  "Error": "Lambda.TimeoutException",
  "Cause": "Lambda timed out."
}
```

## Configuring mocked service integrations<a name="mock-resp-struct-req"></a>

You can mock any service integration using Step Functions Local\. However, Step Functions Local doesn’t enforce the mocks to be the same as the real APIs\. A mocked Task will never call the service endpoint\. If you do not specify a mocked response, a Task will attempt to call the service endpoints\. In addition, Step Functions Local will automatically generate a task token when you mock a Task using the `.waitForTaskToken`\.
