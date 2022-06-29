# Troubleshooting service integrations<a name="troubleshooting-service-integrations"></a>

## My job is complete in the downstream service, but in Step Functions the task state remains "In progress" or its completion is delayed\.<a name="troubleshooting-service-integrations-task-delay"></a>

For `.sync` service integration patterns, Step Functions uses EventBridge rules, downstream APIs, or a combination of both to detect the downstream job status\. For some services, Step Functions does not create EventBridge rules to monitor\. For example, for the AWS Glue service integration, instead of using EventBridge rules, Step Functions makes a `glue:GetJobRun` call\. Because of the frequency of API calls, there is a difference between the downstream task completion and the Step Functions task completion time\. Step Functions requires IAM permissions to manage the EventBridge rules and to make calls to the downstream service\. For more details about how insufficient permissions on your execution role can affect the completion of tasks, see [Additional permissions for tasks using the Run a Job pattern](service-integration-iam-templates.md#connect-iam-sync-async)\. 

## I want to return a JSON output from a nested state machine execution\.<a name="troubleshooting-service-integrations-json-from-nested"></a>

There are two Step Functions synchronous service integrations for Step Functions: `startExecution.sync` and `startExecution.sync:2`\. Both wait for the nested state machine to complete, but they return different `Output` formats\. You can use `startExecution.sync:2` to return a JSON output under `Output`\. 

## I can't invoke a Lambda function from another account\.<a name="troubleshooting-service-integrations-invoke-lambda-from-account"></a>

In the `Task` state’s `Resource` field, use `arn:aws:states:::lambda:invoke` and pass the `FunctionArn` in parameters\. The IAM role that is associated with the state machine must have the right permissions to invoke cross\-account Lambda functions: `lambda:invokeFunction`\. 

```
{  
   "StartAt":"CallLambda",
   "States":{  
      "CallLambda":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::lambda:invoke",
         "Parameters":{  
            "FunctionName":"arn:aws:lambda:us-west-2:123456789012:function:my-function"
         },
         "End":true
      }
   }
}
```

## I'm unable to see task tokens passed from `.waitForTaskToken` states\.<a name="troubleshooting-service-integrations-unable-to-see-task-tokens"></a>

 In the `Task` state’s `Parameters` field, you must pass a task token\. For example, you could use something similar to the following code\.

```
{  
   "StartAt":"taskToken",
   "States":{  
      "taskToken":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::lambda:invoke.waitForTaskToken",
         "Parameters":{  
            "FunctionName":"get-model-review-decision",
            "Payload":{  
               "token.$":"$$.Task.Token"
            },
         },
         "End":true
      }
   }
}
```

**Note**  
You can try to use `.waitForTaskToken` with any API action\. However, some APIs don't have any suitable parameters\.