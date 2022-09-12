# Manage Amazon ECS or Fargate Tasks with Step Functions<a name="connect-ecs"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized Amazon ECS/Fargate integration is different than the Amazon ECS or Fargate AWS SDK integration**  
The \.sync integration pattern is available\.
`ecs:runTask` can return an HTTP 200 response, but have a non\-empty `Failures` field as follows:  
**Request Response**: return the response and do not fail the task\. This is the same as no optimization\.
**Run a Job**: if a non\-empty `Failures` field is encountered, the task is failed with an `AmazonECS.Unknown` error\.
**Task Token**: if a non\-empty `Failures` field is encountered, the task is failed with an `AmazonECS.Unknown` error\.

Supported Amazon ECS/Fargate APIs and syntax:

**Note**  
Parameters in Step Functions are expressed in `PascalCase`, even when the native service API is `camelCase`\. This holds for the contents of Parameters whose types are non-primitives. For example, consider `PlacementStrategy`, which is an Array of [PlacementStrategy](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_PlacementStrategy.html) objects. The properties of those objects should also be `PascalCase`.

+ [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) starts a new task using the specified task definition\.
  + [Request syntax](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#API_RunTask_RequestSyntax)
  + Supported parameters: 
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-cluster](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-cluster)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-group](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-group)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-launchType](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-launchType)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-networkConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-networkConfiguration)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-overrides](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-overrides)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-placementConstraints](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-placementConstraints)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-placementStrategy](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-placementStrategy)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-platformVersion](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-platformVersion)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-propagateTags](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-propagateTags)
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-taskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-taskDefinition)
  + [Response syntax](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#API_RunTask_ResponseSyntax)

**Note**  
For the `Overrides` parameter, Step Functions does not support `executionRoleArn` or `taskRoleArn` as `ContainerOverrides`\.

## Passing Data to an Amazon ECS Task<a name="connect-ecs-pass-to"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

You can use `overrides` to override the default command for a container, and pass input to your Amazon ECS tasks\. See [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerOverride.html](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerOverride.html)\. In the example, we have used JsonPath to pass values to the `Task` from the input to the `Task` state\.

The following includes a `Task` state that runs an Amazon ECS task and waits for it to complete\.

```
{
 "StartAt": "Run an ECS Task and wait for it to complete",
 "States": {
   "Run an ECS Task and wait for it to complete": {
     "Type": "Task",
     "Resource": "arn:aws:states:::ecs:runTask.sync",
     "Parameters": {
                "Cluster": "cluster-arn",
                "TaskDefinition": "job-id",
                "Overrides": {
                    "ContainerOverrides": [
                        {
                            "Name": "container-name",
                            "Command.$": "$.commands" 
                        }
                    ]
                }
            },
     "End": true
    }
  }
}
```

The `"Command.$": "$.commands"` line in `ContainerOverrides` passes the commands from the state input to the container\.

For the previous example, each of the commands will be passed as a container override if the input to the execution is the following\.

```
{
  "commands": [
    "test command 1",
    "test command 2",
    "test command 3"
  ]
}
```

The following includes a `Task` state that runs an Amazon ECS task, and then waits for the task token to be returned\. See [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token)\.

```
{  
   "StartAt":"Manage ECS task",
   "States":{  
      "Manage ECS task":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::ecs:runTask.waitForTaskToken",
         "Parameters":{  
            "LaunchType":"FARGATE",
            "Cluster":"cluster-arn",
            "TaskDefinition":"job-id",
            "Overrides":{  
               "ContainerOverrides":[  
                  {  
                     "Name":"container-name",
                     "Environment":[  
                        {  
                           "Name":"TASK_TOKEN_ENV_VARIABLE",
                           "Value.$":"$$.Task.Token"
                        }
                     ]
                  }
               ]
            }
         },
         "End":true
      }
   }
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.
