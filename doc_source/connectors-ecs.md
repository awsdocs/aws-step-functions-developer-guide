# Manage Amazon ECS/Fargate Tasks With Step Functions<a name="connectors-ecs"></a>

Step Functions can control some AWS services directly from the Amazon States Language\. For more information, see:
+ [Service Integrations](concepts-connectors.md)
+ [Pass Parameters to a Service API](connectors-parameters.md)

Supported Amazon ECS APIs and syntax:
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
    + [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-taskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#ECS-RunTask-request-taskDefinition)
  + [Response syntax](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html#API_RunTask_ResponseSyntax)

**Note**  
For the `Overrides` parameter, Step Functions does not support `executionRoleArn` or `taskRoleArn` as `containerOverrides`\.

## Passing data to an Amazon ECS task<a name="connectors-ecs-pass-to"></a>

Step Functions can control some AWS services directly from the Amazon States Language\. For more information, see:
+ [Service Integrations](concepts-connectors.md)
+ [Pass Parameters to a Service API](connectors-parameters.md)

You can use `overrides` to override the default command for a container, and pass input to your ECS Tasks\. See, [https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerOverride.html](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerOverride.html)\. In the example, we have used JsonPath to pass values to the Task from the input to the Task state\.

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

For the above example, if the input to the execution is:

```
{
  "commands": [
    "test command 1",
    "test command 2",
    "test command 3"
  ]
}
```

The `"Command.$": "$.commands"` line in `ContainerOverrides` passes the commands from the state input to the container\.

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](connectors-iam-templates.md)\.