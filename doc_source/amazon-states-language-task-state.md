# Task<a name="amazon-states-language-task-state"></a>

A `Task` state \(`"Type": "Task"`\) represents a single unit of work performed by a state machine\.

All work in your state machine is done by *tasks*\. A task performs work by using an activity or an AWS Lambda function, or by passing parameters to the API actions of other services\.

AWS Step Functions can invoke Lambda functions directly from a task state\. A Lambda function is a cloud\-native task that runs on AWS Lambda\. You can write Lambda functions in a variety of programming languages, using the AWS Management Console or by uploading code to Lambda\. For more information see [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md)\.

**Note**  
Step Functions can coordinate some AWS services directly from a task state\. For more information see [Service Integrations](concepts-service-integrations.md)\.

An activity consists of program code that waits for an operator to perform an action or to provide input\. You can host activities on Amazon EC2, on Amazon ECS, or even on mobile devices\. Activities poll Step Functions using the `GetActivityTask` and `SendTaskSuccess`, `SendTaskFailure`, and `SendTaskHeartbeat` API actions\.

The Amazon States Language represents tasks by setting a state's type to `Task` and by providing the task with the Amazon Resource Name \(ARN\) of the activity or Lambda function\. 

In addition to the [common state fields](amazon-states-language-common-fields.md), `Task` states have the following fields\.

** `Resource` \(Required\)**  
A URI, especially an Amazon Resource Name \(ARN\) that uniquely identifies the specific task to execute\.

** `Parameters` \(Optional\)**  
Used to pass information to the API actions of connected resources\. The parameters can use a mix of static JSON and [JsonPath](https://github.com/json-path/JsonPath)\. For more information, see [Pass Parameters to a Service API](connect-parameters.md)\.

** `ResultPath` \(Optional\)**  
Specifies where \(in the input\) to place the results of executing the task that's specified in `Resource`\. The input is then filtered as specified by the `OutputPath` field \(if present\) before being used as the state's output\. For more information, see [path](concepts-input-output-filtering.md)\.

** `Retry` \(Optional\)**  
An array of objects, called Retriers, that define a retry policy if the state encounters runtime errors\. For more information, see [Examples Using Retry and Using Catch](concepts-error-handling.md#error-handling-examples)\.

** `Catch` \(Optional\)**  
An array of objects, called Catchers, that define a fallback state\. This state is executed if the state encounters runtime errors and its retry policy is exhausted or isn't defined\. For more information, see [Fallback States](concepts-error-handling.md#error-handling-fallback-states)\.

** `TimeoutSeconds` \(Optional\)**  
If the task runs longer than the specified seconds, this state fails with a `States.Timeout` error name\. Must be a positive, non\-zero integer\. If not provided, the default value is `99999999`\. The count begins after the task has been started, for example, when `ActivityStarted` or `LambdaFunctionStarted` are logged in the **Execution event history**\.

** `HeartbeatSeconds` \(Optional\)**  
If more time than the specified seconds elapses between heartbeats from the task, this state fails with a `States.Timeout` error name\. Must be a positive, non\-zero integer less than the number of seconds specified in the `TimeoutSeconds` field\. If not provided, the default value is `99999999`\. For Activities, the count begins when `GetActivityTask` receives a token and `ActivityStarted` is logged in the **Execution event history**\.

A `Task` state must set either the `End` field to `true` if the state ends the execution, or must provide a state in the `Next` field that is run when the `Task` state is complete\.

## Task State Example<a name="task-state-example"></a>

Here is an example\.

```
"ActivityState": {
  "Type": "Task",
  "Resource": "arn:aws:states:us-east-1:123456789012:activity:HelloWorld",
  "TimeoutSeconds": 300,
  "HeartbeatSeconds": 60,
  "Next": "NextState"
}
```

In this example, `ActivityState` will schedule the `HelloWorld` activity for execution in the `us-east-1` AWS Region on the caller's AWS account\. When `HelloWorld` completes, the next state \(here called `NextState`\) will be run\.

If this task fails to complete within 300 seconds, or doesn't send heartbeat notifications in intervals of 60 seconds, the task is marked as `failed`\. It's a good practice to set a timeout value and a heartbeat interval for long\-running activities\.

## Specifying Resource ARNs in Tasks<a name="amazon-states-language-task-state-specifying-resource-arns"></a>

The `Resource` field's Amazon Resource Name \(ARN\) is specified using the following pattern\.

```
arn:partition:service:region:account:task_type:name
```

In this pattern:
+  `partition` is the AWS Step Functions partition to use, most commonly `aws`\.
+  `service` indicates the AWS service used to execute the task, and is:
  +  `states` for an [activity](#amazon-states-language-task-state-activity)\.
  +  `lambda` for a [Lambda function](#amazon-states-language-task-state-lambda)\.
+  `region` is the [AWS Region](https://docs.aws.amazon.com/general/latest/gr/rande.html) in which the Step Functions activity or state machine type or Lambda function has been created\.
+  `account` is your AWS account ID\.
+  `task_type` is the type of task to run\. It is one of the following values:
  +  `activity` – An [activity](#amazon-states-language-task-state-activity)\.
  +  `function` – A [Lambda function](#amazon-states-language-task-state-lambda)\.
  +  `servicename` – The name of a supported connected service \(see [Supported AWS Service Integrations for Step Functions](connect-supported-services.md)\)\.
+  `name` is the registered resource name \(activity name, Lambda function name, or service API action\)\.

**Note**  
Step Functions doesn't support referencing ARNs across partitions \(for example, "aws\-cn" can't invoke tasks in the "aws" partition, and vice versa\)\.

## Task Types<a name="task-types"></a>

The following task types are supported:
+  [activity](#amazon-states-language-task-state-activity) 
+  [Lambda functions](#amazon-states-language-task-state-lambda) 
+  [A supported AWS service](concepts-service-integrations.md) 

The following sections provide more detail about each task type\.

### Activity<a name="amazon-states-language-task-state-activity"></a>

Activities represent workers \(processes or threads\), implemented and hosted by you, that perform a specific task\.

Activity `resource` ARNs use the following syntax\.

```
arn:partition:states:region:account:activity:name
```

For more information about these fields, see [Specifying Resource ARNs in Tasks](#amazon-states-language-task-state-specifying-resource-arns)\.

**Note**  
You must create activities with Step Functions \(using a [CreateActivity](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateActivity.html), API action, or the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\) before their first use\.

For more information about creating an activity and implementing workers, see [Activities](concepts-activities.md)\.

### Lambda Functions<a name="amazon-states-language-task-state-lambda"></a>

Lambda tasks execute a function using AWS Lambda\. To specify a Lambda function, use the Amazon Resource Name \(ARN\) of the Lambda function in the `Resource` field\.

Lambda function `Resource` ARNs use the following syntax\.

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

After the Lambda function specified in the `Resource` field completes, its output is sent to the state identified in the `Next` field \("NextState"\)\.

### A Supported AWS Service<a name="amazon-states-language-task-state-connector"></a>

When you reference a connected resource, Step Functions directly calls the API actions of a supported service\. Specify the service and action in the `Resource` field\.

Connected service `Resource` ARNs use the following syntax\.

```
arn:partition:states:region:account:servicename:APIname
```

**Note**  
To create a synchronous connection to a connected resource, append `.sync` to the *APIname* entry in the ARN\. For more information, see [Service Integrations](concepts-service-integrations.md)\.

For example:

```
{
 "StartAt": "BATCH_JOB",
 "States": {
   "BATCH_JOB": {
     "Type": "Task",
     "Resource": "arn:aws:states:::batch:submitJob.sync",
     "Parameters": {  
       "JobDefinition": "preprocessing",
       "JobName": "PreprocessingBatchJob",
       "JobQueue": "SecondaryQueue",
       "Parameters.$": "$.batchjob.parameters",
       "RetryStrategy": {
          "attempts": 5
        }
     },
     "End": true
    }
  }
}
```