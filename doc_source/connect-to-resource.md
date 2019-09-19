# Service Integration Patterns<a name="connect-to-resource"></a>

AWS Step Functions integrates with services directly in the Amazon States Language\. You can control these AWS services using three different service integration patterns\.
+ Call a service and let Step Functions progress to the next state immediately after it gets an HTTP response\.
+ Call a service and have Step Functions wait for a job to complete\.
+ Call a service with a task token and have Step Functions wait until that token is returned along with a payload\.

Each of these service integration patterns is controlled by how you create a URI in the `"Resource"` field of your [task definition](amazon-states-language-task-state.md)\.

**Topics**
+ [Request Response](#connect-default)
+ [Run a Job](#connect-sync)
+ [Wait for a Callback with the Task Token](#connect-wait-token)

**Note**  
For information about configuring AWS Identity and Access Management \(IAM\) for integrated services, see [IAM Policies for Integrated Services](service-integration-iam-templates.md)\.

## Request Response<a name="connect-default"></a>

When you specify a service in the `"Resource"` string of your task state, and you *only* provide the resource, Step Functions will wait for an HTTP response and then progress to the next state\. Step Functions will not wait for a job to complete\.

The following example shows how you can publish an Amazon SNS topic\.

```
"Send message to SNS":{  
   "Type":"Task",
   "Resource":"arn:aws:states:::sns:publish",
   "Parameters":{  
      "TopicArn":"arn:aws:sns:us-east-1:123456789012:myTopic",
      "Message":"Hello from Step Functions!"
   },
   "Next":"NEXT_STATE"
}
```

This example references the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) API of Amazon SNS\. The workflow will progress to the next state after calling the `Publish` API\.

## Run a Job<a name="connect-sync"></a>

For integrated services such as AWS Batch and Amazon ECS, Step Functions can wait for a request to complete before progressing to the next state\. To have Step Functions wait, specify the `"Resource"` field in your task state definition with the `.sync` suffix appended after the resource URI\. 

For example, when submitting an AWS Batch job, see the `"Resource"` field in the state machine definition that follows\.

```
"Manage Batch task": {
  "Type": "Task",
  "Resource": "arn:aws:states:::batch:submitJob.sync",
  "Parameters": {
    "JobDefinition": "arn:aws:batch:us-east-2:123456789012:job-definition/testJobDefinition",
    "JobName": "testJob",
    "JobQueue": "arn:aws:batch:us-east-2:123456789012:job-queue/testQueue"
  },
  "Next": "NEXT_STATE"
}
```

The `.sync` portion appended to the resource ARN tells Step Functions to wait for the job to complete\. After calling AWS Batch `submitJob`, the workflow pauses\. When the job is complete, Step Functions progresses to the next state\. For more information, see the AWS Batch sample project: [Manage a Batch Job \(AWS Batch, Amazon SNS\)](batch-job-notification.md)\. 

**Note**  
To see a list of what integrated services support waiting for a job to complete \(`.sync`\), see [Supported AWS Service Integrations for Step Functions](connect-supported-services.md)\.

## Wait for a Callback with the Task Token<a name="connect-wait-token"></a>

Callback tasks provide a way to pause a workflow until a task token is returned\. A task might need to wait for a human approval, integrate with a third party, or call legacy systems\. For tasks like these, you can pause AWS Step Functions indefinitely, and wait for an external process or workflow to complete\. For these situations Step Functions allows you to pass a task token to some integrated services\. The task will pause until it receives that task token back with a [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html) or [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html) call\.

**Note**  
To see a list of what integrated services support waiting for a task token \(`.waitForTaskToken`\), see [Supported AWS Service Integrations for Step Functions](connect-supported-services.md)\.

**Topics**
+ [Task Token Example](#connect-wait-example)
+ [Get a Token from the Context Object](#wait-token-contextobject)
+ [Configure a Heartbeat Timeout for a Waiting Task](#wait-token-hearbeat)

### Task Token Example<a name="connect-wait-example"></a>

In this example a Step Functions workflow needs to integrate with an external microservice to perform a credit check as a part of an approval workflow\. Step Functions publishes an Amazon SQS message that includes a task token as a part of the message\. An external system integrates with Amazon SQS, and pulls the message off the queue\. When that's finished, it returns the result and the original task token\. Step Functions then continues with its workflow\.

![\[SQS task waiting for a task token to be returned\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wait-for-task-token.png)![\[SQS task waiting for a task token to be returned\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[SQS task waiting for a task token to be returned\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

The `"Resource"` field of the task definition that references Amazon SQS includes `.waitForTaskToken` appended to the end\.

```
"Send message to SQS": {
  "Type": "Task",
  "Resource": "arn:aws:states:::sqs:sendMessage.waitForTaskToken",
  "Parameters": {
    "QueueUrl": "https://sqs.us-east-2.amazonaws.com/123456789012/myQueue",
    "MessageBody": {
        "Message": "Hello from Step Functions!",
        "TaskToken.$": "$$.Task.Token"
     }
  },
  "Next": "NEXT_STATE"
}
```

This tells Step Functions to pause and wait for the task token\. When you specify a resource using `.waitForTaskToken`, the task token can be accessed in the `"Parameters"` field of your state definition with a special path designation \(`$$.Task.Token`\)\. The initial `$$.` designates that the path accesses the [context object](#wait-token-contextobject), and gets the task token for the current task in a running execution\.

When it's complete, the external service calls [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html) or [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html) with the `taskToken` included\. Only then does the workflow continue to the next state\. 

**Note**  
To avoid waiting indefinitely if a process fails to send the task token along with `SendTaskSuccess` or `SendTaskFailure`, see [Configure a Heartbeat Timeout for a Waiting Task](#wait-token-hearbeat)\.

### Get a Token from the Context Object<a name="wait-token-contextobject"></a>

The context object is an internal JSON object that contains information about your execution\. Like state input, it can be accessed with a path from the `"Parameters"` field during an execution\. When accessed from within a task definition, it includes information about the specific execution, including the task token\.

```
{
    "Execution": {
        "Id": "arn:aws:states:us-east-1:123456789012:execution:stateMachineName:executionName",
        "Input": {
           "key": "value"
        },
        "Name": "executionName",
        "RoleArn": "arn:aws:iam::123456789012:role...",
        "StartTime": "2019-03-26T20:14:13.192Z"
    },
    "State": {
        "EnteredTime": "2019-03-26T20:14:13.192Z",
        "Name": "Test",
        "RetryCount": 3
    },
    "StateMachine": {
        "Id": "arn:aws:states:us-east-1:123456789012:stateMachine:stateMachineName",
        "Name": "name"
    },
    "Task": {
        "Token": "h7XRiCdLtd/83p1E0dMccoxlzFhglsdkzpK9mBVKZsp7d9yrT1W"
    }
}
```

You can access the task token by using a special path from inside the `"Parameters"` field of your task definition\. To access the input or the context object, you first specify that the parameter will be a path by appending a `.$` to the parameter name\. The following specifies nodes from both the input and the context object in a `"Parameters"` specification\.

```
"Parameters": {
   "Input.$": "$",
   "TaskToken.$": "$$.Task.Token"
 },
```

In both cases appending `.$` to the parameter name tells Step Functions to expect a path\. In the first case, `"$"` is a path that includes the entire input\. In the second case, `$$.` specifies that the path will access the context object, and `$$.Task.Token` sets the parameter to the value of the task token in the context object of a running execution\.

In the Amazon SQS example, `.waitForTaskToken` in the `"Resource"` field tells Step Functions to wait for the task token to be returned\. The `"TaskToken.$": "$$.Task.Token"` parameter passes that token as a part of the Amazon SQS message\.

```
"Send message to SQS": {
  "Type": "Task",
  "Resource": "arn:aws:states:::sqs:sendMessage.waitForTaskToken",
  "Parameters": {
    "QueueUrl": "https://sqs.us-east-2.amazonaws.com/123456789012/myQueue",
    "MessageBody": {
        "Message": "Hello from Step Functions!",
        "TaskToken.$": "$$.Task.Token"
     }
  },
  "Next": "NEXT_STATE"
}
```

For more information about the context object, see [The Context Object](input-output-contextobject.md) in the [Input and Output Processing](concepts-input-output-filtering.md) section in this guide\. 

### Configure a Heartbeat Timeout for a Waiting Task<a name="wait-token-hearbeat"></a>

A task that is waiting for a task token will wait until the execution reaches the one year service limit \(see, [Limits Related to State Throttling](limits.md#service-limits-api-state-throttling)\)\. To avoid stuck executions you can configure a heartbeat timeout interval in your state machine definition\. Use the [`HeartbeatSeconds`](amazon-states-language-task-state.md) field to specify the timeout interval\.

```
{
  "StartAt": "Push based to SQS",
  "States": {
    "Push to SQS": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sqs:sendMessage.waitForTaskToken",
      "HeartbeatSeconds": 600,
      "Parameters": {
        "MessageBody": { "myTaskToken.$": "$$.Task.Token" },
        "QueueUrl": "https://sqs.us-east-1.amazonaws.com/123456789012/push-based-queue"
      },
      "ResultPath": "$.SQS",
      "End": true
    }
  }
}
```

In this state machine definition, a task pushes a message to Amazon SQS and waits for an external process to call back with the provided task token\. The `"HeartbeatSeconds": 600` field sets the heartbeat timeout interval to 10 minutes\. The task will wait for the task token to be returned with one of these API actions:
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html) 
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html)
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html)

If the waiting task doesn't receive a valid task token within that 10 minute period, the task fails with a `States.Timeout` error name\.

For more information, see the callback task sample project: [Callback Pattern Example ](callback-task-sample-sqs.md)\.