# Task<a name="amazon-states-language-task-state"></a>

A `Task` state \(`"Type": "Task"`\) represents a single unit of work performed by a state machine\.

In addition to the [common state fields](amazon-states-language-states.md#amazon-states-language-common-fields), `Task` states have the following fields:

** `Resource` \(Required\)**  
A URI, especially an Amazon Resource Name \(ARN\) that uniquely identifies the specific task to execute\.

** `ResultPath` \(Optional\)**  
Specifies where \(in the input\) to place the results of executing the task specified in `Resource`\. The input is then filtered as prescribed by the `OutputPath` field \(if present\) before being used as the state's output\. For more information, see [path](amazon-states-language-input-output-processing.md)\.

** `Retry` \(Optional\)**  
An array of objects, called Retriers, that define a retry policy in case the state encounters runtime errors\. For more information, see [Retrying After an Error](amazon-states-language-errors.md#amazon-states-language-retrying-after-error)\.

** `Catch` \(Optional\)**  
An array of objects, called Catchers, that define a fallback state which is executed in case the state encounters runtime errors and its retry policy has been exhausted or is not defined\. For more information, see [Fallback States](amazon-states-language-errors.md#amazon-states-language-fallback-states)\.

** `TimeoutSeconds` \(Optional\)**  
If the task runs longer than the specified seconds, then this state fails with a `States.Timeout` Error Name\. Must be a positive, non\-zero integer\. If not provided, the default value is `99999999`\.

** `HeartbeatSeconds` \(Optional\)**  
If more time than the specified seconds elapses between heartbeats from the task, then this state fails with an `States.Timeout` Error Name\. Must be a positive, non\-zero integer less than the number of seconds specified in the `TimeoutSeconds` field\. If not provided, the default value is `99999999`\.

A `Task` state must set either the `End` field to `true` if the state ends the execution, or must provide a state in the `Next` field that will be run upon completion of the `Task` state\.

Here is an example:

```
"ActivityState": {
  "Type": "Task",
  "Resource": "arn:aws:states:us-east-1:123456789012:activity:HelloWorld",
  "TimeoutSeconds": 300,
  "HeartbeatSeconds": 60,
  "Next": "NextState"
}
```

In this example, `ActivityState` will schedule the `HelloWorld` activity for execution in the `us-east-1` region on the caller's AWS account\. When `HelloWorld` completes, the next state \(here called `NextState`\) will be run\.

If this task fails to complete within 300 seconds, or does not send heartbeat notifications in intervals of 60 seconds, then the task is marked as `failed`\. It's a good practice to set a timeout value and a heartbeat interval for long\-running activities\.

## Specifying Resource ARNs in Tasks<a name="amazon-states-language-task-state-specifying-resource-arns"></a>

The `Resource` field's Amazon Resource Name \(ARN\) is specified using the following pattern:

```
arn:partition:service:region:account:task_type:name
```

Where:

+  `partition` is the AWS Step Functions partition to use, most commonly `aws`\.

+  `service` indicates the AWS service used to execute the task, and is either:

  +  `states` for an [activity](#amazon-states-language-task-state-activity)\.

  +  `lambda` for a [Lambda function](#amazon-states-language-task-state-lambda)\.

+  `region` is the [AWS region](http://docs.aws.amazon.com/general/latest/gr/rande.html) in which the Step Functions activity/state machine type or Lambda function has been created\.

+  `account` is your AWS account id\.

+  `task_type` is the type of task to run\. It will be one of the following values:

  +  `activity` – an [activity](#amazon-states-language-task-state-activity)\.

  +  `function` – a [Lambda function](#amazon-states-language-task-state-lambda)\.

+  `name` is the registered resource name \(activity name or Lambda function name\)\.

**Note**  
Step Functions does not support referencing ARNs across partitions \(For example: "aws\-cn" cannot invoke tasks in the "aws" partition, and vice versa\);

## Task Types<a name="task-types"></a>

The following task types are supported:

+  [activity](#amazon-states-language-task-state-activity) 

+  [Lambda functions](#amazon-states-language-task-state-lambda) 

The following sections will provide more detail about each type\.

### Activity<a name="amazon-states-language-task-state-activity"></a>

Activities represent workers \(processes or threads\), implemented and hosted by you, that perform a specific task\.

Activity `resource` ARNs use the following syntax:

```
arn:partition:states:region:account:activity:name
```

For more information about these fields, see [Specifying Resource ARNs in Tasks](#amazon-states-language-task-state-specifying-resource-arns)\.

**Note**  
activities must be created with Step Functions \(using a [CreateActivity](http://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateActivity.html), API action, or the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\) before their first use\.

For more information about creating an activity and implementing workers, see [Activities](concepts-activities.md)\.

### Lambda Functions<a name="amazon-states-language-task-state-lambda"></a>

Lambda functions execute a function using AWS Lambda\. To specify a Lambda function, use the ARN of the Lambda function in the `Resource` field\.

Lambda function `Resource` ARNs use the following syntax:

```
arn:partition:lambda:region:account:function:function_name
```

For more information about these fields, see [Specifying Resource ARNs in Tasks](#amazon-states-language-task-state-specifying-resource-arns)\.

For example:

```
"LambdaState": {
  "Type": "Task",
  "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloWorld",
  "Next": "NextState"
}
```

Once the Lambda function specified in the `Resource` field completes, its output is sent to the state identified in the `Next` field \("NextState"\)\.