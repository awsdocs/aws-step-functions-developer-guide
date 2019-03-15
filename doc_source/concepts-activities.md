# Activities<a name="concepts-activities"></a>

Activities are an AWS Step Functions feature that enables you to have a task in your state machine where the work is performed by a *worker* that can be hosted on Amazon Elastic Compute Cloud \(Amazon EC2\), Amazon Elastic Container Service \(Amazon ECS\), mobile devices—basically anywhere\.

**Topics**
+ [Overview](#activities-overview)
+ [Waiting for an Activity Task to Complete](#activities-wait)
+ [Next Steps](#activities-nextsteps)
+ [Example Activity Worker in Ruby](example-ruby-activity-worker.md)

## Overview<a name="activities-overview"></a>

In AWS Step Functions, activities are a way to associate code running somewhere \(known as an *activity worker*\) with a specific task in a state machine\. You can create an activity using the Step Functions console, or by calling `[CreateActivity](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateActivity.html)`\. This provides an Amazon Resource Name \(ARN\) for your task state\. Use this ARN to poll the task state for work in your activity worker\. 

**Note**  
Activities are not versioned and are expected to be backward compatible\. If you must make a backward\-incompatible change to an activity, create a *new* activity in Step Functions using a unique name\.

An activity worker can be an application running on an Amazon EC2 instance, an AWS Lambda function, a mobile device: any application that can make an HTTP connection, hosted anywhere\. When Step Functions reaches an activity task state, the workflow waits for an activity worker to poll for a task\. An activity worker polls Step Functions by using `[GetActivityTask](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html)`, and sending the ARN for the related activity\. `GetActivityTask` returns a response including `input` \(a string of JSON input for the task\) and a [https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html#StepFunctions-GetActivityTask-response-taskToken](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html#StepFunctions-GetActivityTask-response-taskToken) \(a unique identifier for the task\)\. After the activity worker completes its work, it can provide a report of its success or failure by using `[SendTaskSuccess](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html)` or `[SendTaskFailure](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html)`\. These two calls use the `taskToken` provided by `GetActivityTask` to associate the result with that task\.

### APIs Related to Activity Tasks<a name="activities-api"></a>

Step Functions provides APIs for creating and listing activities, requesting a task, and for managing the flow of your state machine based on the results of your worker\.

The following are the Step Functions APIs that are related to activities: 
+ `[CreateActivity](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateActivity.html)`
+ `[GetActivityTask](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html)`
+ `[ListActivities](https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListActivities.html)`
+ `[SendTaskFailure](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html)`
+ `[SendTaskHeartbeat](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html)`
+ `[SendTaskSuccess](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html)`

**Note**  
Polling for activity tasks with `GetActivityTask` can cause latency in some implementations\. See [Avoid Latency When Polling for Activity Tasks](bp-activity-pollers.md)\.

## Waiting for an Activity Task to Complete<a name="activities-wait"></a>

Configure how long a state waits by setting `TimeoutSeconds` in the task definition\. To keep the task active and waiting, periodically send a heartbeat from your activity worker using `[SendTaskHeartbeat](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html)` within the time configured in `TimeoutSeconds`\. By configuring a long timeout duration and actively sending a heartbeat, an activity in Step Functions can wait up to a year for an execution to complete\. 

For example, if you need a workflow that waits for the outcome of a long process, do the following:

1. Create an activity by using the console, or by using `[CreateActivity](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateActivity.html)`\. Make a note of the activity ARN\.

1. Reference that ARN in an activity task state in your state machine definition and set `TimeoutSeconds`\.

1. Implement an activity worker that polls for work by using `[GetActivityTask](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html)`, referencing that activity ARN\. 

1. Use `[SendTaskHeartbeat](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html)` periodically within the time you set in `[HeartbeatSeconds](amazon-states-language-task-state.md)` in your state machine task definition to keep the task from timing out\.

1. Start an execution of your state machine\.

1. Start your activity worker process\.

The execution pauses at the activity task state and waits for your activity worker to poll for a task\. Once a `taskToken` is provided to your activity worker, your workflow will wait for `[SendTaskSuccess](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html)` or `[SendTaskFailure](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html)` to provide a status\. If the execution doesn't receive either of these or a `[SendTaskHeartbeat](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html)` call before the time configured in `TimeoutSeconds`, the execution will fail and the execution history will contain an `ExecutionTimedOut` event\.

## Next Steps<a name="activities-nextsteps"></a>

For a detailed look at creating a state machine that uses an activity worker, see [Creating an Activity State Machine](tutorial-creating-activity-state-machine.md)\.