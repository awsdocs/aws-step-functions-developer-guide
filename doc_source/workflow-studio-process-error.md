# Error handling<a name="workflow-studio-process-error"></a>

By default, when a state reports an error, Step Functions causes the execution to fail entirely\. For actions and some flow states, you can configure how Step Functions handles errors\. Even if you have configured error handling, some errors may still cause an execution to fail\. For more information, see [Error handling in Step Functions](concepts-error-handling.md)\. In Workflow Studio, configure error handling in the the **Error handling** panel\. 

![\[Error handling options\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_error_01.png)

## Retry on errors<a name="workflow-studio-process-error-retry"></a>

You can add one or more rules to action states and the `Parallel` flow state to retry the task when an error occurs\. These rules are called *retriers*\. To add a retrier, choose **Add new retrier**, then configure its options: 
+ \(Optional\) The **Comment** field can be used to add your own comment\. It will not affect the workflow, but can be used to annotate your workflow\. 
+ Choose an **Error** that will trigger the retrier, or enter your own custom error name\.
+ \(Optional\) Set an **Interval**\. This is the time in seconds before Step Functions make its first retry\. Additional retries will follow at intervals that you can configure with **Max attempts** and **Backoff rate**\.
+ \(Optional\) Set **Max attempts**\. This is the maximum number retries before Step Functions will cause the execution to fail\.
+ \(Optional\) Set the **Backoff rate**\. This is a multiplier that determines by how much the retry interval will increase with each attempt\.

## Catch errors<a name="workflow-studio-process-error-catch"></a>

You can add one or more rules to action states and to the `Parallel` and `Map` flow state to catch an error\. These rules are called *catchers*\. To add a catcher, choose **Add new catcher**, then configure its options:
+ \(Optional\) The **Comment** field can be used to add your own comment\. It will not affect the workflow, but can be used to annotate your workflow\. 
+ Choose an **Error** that will trigger the catcher, or enter your own custom error name\.
+ Choose an **Fallback state**\. This is the state that the workflow will move to next, after an error is caught\.
+ \(Optional\) Add a `ResultPath` to add the error to the original state input\. The [`ResultPath`](input-output-resultpath.md) must be in valid JSONPAth syntax\. This will be sent to the fallback state\.

## Timeouts<a name="workflow-studio-process-error-timeout"></a>

You can configure a timeout for action states to set the maximum number of seconds your state can run before it fails\. Use timeouts to prevent stuck executions\. To configure a timeout, enter the number of seconds your state should wait before the execution fails\. For more information, see `TimeoutSeconds` in the [Task](amazon-states-language-task-state.md) section\.

## Heartbeat<a name="workflow-studio-process-error-heartbeat"></a>

You can configure a *heartbeat* or periodic notification sent by your task\. If you set a heartbeat interval, and your state doesn't send heartbeat notifications in the configured intervals, the task is marked as failed\. To configure a heartbeat, set a positive, non\-zero integer number of seconds\. For more information, see `HeartBeatSeconds` in the [Task](amazon-states-language-task-state.md) section\. 

**Note**  
Not all error handling options are available for all states\. Lambda Invoke has one retrier configured by default\.