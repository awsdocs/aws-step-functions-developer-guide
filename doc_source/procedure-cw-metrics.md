# Monitoring Step Functions Using CloudWatch<a name="procedure-cw-metrics"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of AWS Step Functions and your AWS solutions\. You should collect as much monitoring data from the AWS services that you use so that you can more easily debug any multi\-point failures\. Before you start monitoring Step Functions, you should create a monitoring plan that answers the following questions:
+ What are your monitoring goals?
+ What resources will you monitor?
+ How often will you monitor these resources?
+ What monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

The next step is to establish a baseline for normal Step Functions performance in your environment\. To do this, measure performance at various times and under different load conditions\. As you monitor Step Functions, consider storing historical monitoring data\. Such data can give you a baseline to compare against current performance data, to identify normal performance patterns and performance anomalies, and to devise ways to address issues\.

For example, with Step Functions, you can monitor how many activities or Lambda tasks fail due to a heartbeat timeout\. When performance falls outside your established baseline, you might have to change your heartbeat interval\.

To establish a baseline you should, at a minimum, monitor the following metrics:
+  `ActivitiesStarted` 
+  `ActivitiesTimedOut` 
+  `ExecutionsStarted` 
+  `ExecutionsTimedOut` 
+  `LambdaFunctionsStarted` 
+  `LambdaFunctionsTimedOut` 

The following sections describe metrics that Step Functions provides to Amazon CloudWatch\. You can use these metrics to track your state machines and activities and to set alarms on threshold values\. You can view metrics using the AWS Management Console\.

**Topics**
+ [Metrics That Report a Time Interval](#monitoring-using-cloudwatch-time-interval-metrics)
+ [Metrics That Report a Count](#monitoring-using-cloudwatch-count-metrics)
+ [Execution Metrics](#cloudwatch-step-functions-execution-metrics)
+ [Activity Metrics](#cloudwatch-step-functions-activity-metrics)
+ [Lambda Function Metrics](#cloudwatch-step-functions-lambda-function-metrics)
+ [Service Integration Metrics](#cloudwatch-step-functions-service-integration-metrics)
+ [Service Metrics](#cloudwatch-step-functions-service-metrics)
+ [API Metrics](#cloudwatch-step-functions-api-metrics)
+ [Viewing Metrics for Step Functions](monitoring-using-cloudwatch-console.md)
+ [Setting Alarms for Step Functions](monitoring-using-cloudwatch-console-set-alarm.md)

## Metrics That Report a Time Interval<a name="monitoring-using-cloudwatch-time-interval-metrics"></a>

Some of the Step Functions CloudWatch metrics are *time intervals*, always measured in milliseconds\. These metrics generally correspond to stages of your execution for which you can set state machine, activity, and Lambda function timeouts, with descriptive names\.

For example, the `ActivityRunTime` metric measures the time it takes for an activity to complete after it begins to execute\. You can set a timeout value for the same time period\.

In the CloudWatch console, you can get the best results if you choose **average** as the display statistic for time interval metrics\.

## Metrics That Report a Count<a name="monitoring-using-cloudwatch-count-metrics"></a>

Some of the Step Functions CloudWatch metrics report results as a *count*\. For example, `ExecutionsFailed` records the number of failed state machine executions\.

In the CloudWatch console, you can get the best results if you choose **sum** as the display statistic for count metrics\.

## Execution Metrics<a name="cloudwatch-step-functions-execution-metrics"></a>

The `AWS/States` namespace includes the following metrics for Step Functions executions\.


| Metric | Description | 
| --- | --- | 
| ExecutionTime | The interval, in milliseconds, between the time the execution starts and the time it closes\. | 
| ExecutionThrottled | The number of StateEntered events and retries that have been throttled\. This is related to StateTransition throttling\. For more information, see [Limits Related to State Throttling](https://docs.aws.amazon.com/step-functions/latest/dg/limits.html#service-limits-api-state-throttling) in the AWS Step Functions Developer Guide\. | 
| ExecutionsAborted | The number of aborted or terminated executions\. | 
| ExecutionsFailed | The number of failed executions\. | 
| ExecutionsStarted | The number of started executions\. | 
| ExecutionsSucceeded | The number of successfully completed executions\. | 
| ExecutionsTimedOut | The number of executions that time out for any reason\. | 

### Dimension for Step Functions Execution Metrics<a name="cloudwatch-step-functions-execution-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  StateMachineArn  |  The Amazon Resource Name \(ARN\) of the state machine for the execution in question\.  | 

## Activity Metrics<a name="cloudwatch-step-functions-activity-metrics"></a>

The `AWS/States` namespace includes the following metrics for Step Functions activities\.


| Metric | Description | 
| --- | --- | 
| ActivityRunTime  | The interval, in milliseconds, between the time the activity starts and the time it closes\. | 
| ActivityScheduleTime | The interval, in milliseconds, for which the activity stays in the schedule state\. | 
| ActivityTime | The interval, in milliseconds, between the time the activity is scheduled and the time it closes\. | 
| ActivitiesFailed | The number of failed activities\. | 
| ActivitiesHeartbeatTimedOut | The number of activities that time out due to a heartbeat timeout\. | 
| ActivitiesScheduled | The number of scheduled activities\. | 
| ActivitiesStarted | The number of started activities\. | 
| ActivitiesSucceeded | The number of successfully completed activities\. | 
| ActivitiesTimedOut | The number of activities that time out on close\. | 

### Dimension for Step Functions Activity Metrics<a name="cloudwatch-step-functions-activity-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  `ActivityArn`  |  The ARN of the activity\.  | 

## Lambda Function Metrics<a name="cloudwatch-step-functions-lambda-function-metrics"></a>

The `AWS/States` namespace includes the following metrics for Step Functions Lambda functions\.


|  Metric  |  Description  | 
| --- | --- | 
| LambdaFunctionRunTime | The interval, in milliseconds, between the time the Lambda function starts and the time it closes\. | 
| LambdaFunctionScheduleTime | The interval, in milliseconds, for which the Lambda function stays in the schedule state\. | 
| LambdaFunctionTime | The interval, in milliseconds, between the time the Lambda function is scheduled and the time it closes\. | 
| LambdaFunctionsFailed | The number of failed Lambda functions\. | 
| LambdaFunctionsScheduled | The number of scheduled Lambda functions\. | 
| LambdaFunctionsStarted | The number of started Lambda functions\. | 
| LambdaFunctionsSucceeded | The number of successfully completed Lambda functions\. | 
| LambdaFunctionsTimedOut | The number of Lambda functions that time out on close\. | 

### Dimension for Step Functions Lambda Function Metrics<a name="cloudwatch-step-functions-lambda-function-metrics-dimensions"></a>


|  Dimension  |  Description  | 
| --- | --- | 
|  `LambdaFunctionArn`  |  The ARN of the Lambda function\.  | 

## Service Integration Metrics<a name="cloudwatch-step-functions-service-integration-metrics"></a>

The `AWS/States` namespace includes the following metrics for Step Functions service integrations\. For more information see [AWS Service Integrations](concepts-service-integrations.md)\. 


|  Metric  |  Description  | 
| --- | --- | 
| ServiceIntegrationRunTime | The interval, in milliseconds, between the time the Service Task starts and the time it closes\. | 
| ServiceIntegrationScheduleTime | The interval, in milliseconds, for which the Service Task stays in the schedule state\. | 
| ServiceIntegrationTime | The interval, in milliseconds, between the time the Service Task is scheduled and the time it closes\. | 
| ServiceIntegrationsFailed | The number of failed Service Tasks\. | 
| ServiceIntegrationsScheduled | The number of scheduled Service Tasks\. | 
| ServiceIntegrationsStarted | The number of started Service Tasks\. | 
| ServiceIntegrationsSucceeded | The number of successfully completed Service Tasks\. | 
| ServiceIntegrationsTimedOut | The number of Service Tasks that time out on close\. | 

### Dimension for Step Functions Service Integration Metrics<a name="cloudwatch-step-functions-service-integration-metrics-dimensions"></a>


|  Dimension  |  Description  | 
| --- | --- | 
|  `ServiceIntegrationResourceArn`  |  The resource ARN of the integrated service\.  | 

## Service Metrics<a name="cloudwatch-step-functions-service-metrics"></a>

The `AWS/States` namespace includes the following metrics for the Step Functions service\.


| Metric | Description | 
| --- | --- | 
| ThrottledEvents  | The count of requests that have been throttled\. | 
| ProvisionedBucketSize |  The count of available requests per second\.  | 
| ProvisionedRefillRate |  The count of requests per second that are allowed into the bucket\.  | 
| ConsumedCapacity |  The count of requests per second\.  | 

### Dimension for Step Functions Service Metrics<a name="cloudwatch-step-functions-service-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  `StateTransition`  |  Filters data to show State Transitions metrics\.  | 

## API Metrics<a name="cloudwatch-step-functions-api-metrics"></a>

The `AWS/States` namespace includes the following metrics for the Step Functions API\.


| Metric | Description | 
| --- | --- | 
| ThrottledEvents  | The count of requests that have been throttled\. | 
| ProvisionedBucketSize |  The count of available requests per second\.  | 
| ProvisionedRefillRate |  The count of requests per second that are allowed into the bucket\.  | 
| ConsumedCapacity |  The count of requests per second\.  | 

### Dimension for Step Functions API Metrics<a name="cloudwatch-step-functions-api-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  `APIName`  |  Filters data to an API of the specified API name\.  | 