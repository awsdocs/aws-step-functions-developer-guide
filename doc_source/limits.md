# Limits<a name="limits"></a>

AWS Step Functions places limits on the sizes of certain state machine parameters, such as the number of API actions that you can make during a certain time period or the number of state machines that you can define\. Although these limits are designed to prevent a misconfigured state machine from consuming all of the resources of the system, many aren't hard limits\.

**Note**  
If a particular stage of your state machine execution or activity execution takes too long, you can configure a state machine timeout to cause a timeout event\.

**Topics**
+ [General Limits](#service-limits-general)
+ [Limits Related to Accounts](#service-limits-accounts)
+ [Limits Related to State Machine Executions](#service-limits-state-machine-executions)
+ [Limits Related to Task Executions](#service-limits-task-executions)
+ [Limits Related to API Action Throttling](#service-limits-api-action-throttling)
+ [Limits Related to State Throttling](#service-limits-api-state-throttling)
+ [Restrictions Related to Tagging](#sfn-limits-tagging)
+ [Requesting a Limit Increase](#sfn-limits-how-to-increase)

## General Limits<a name="service-limits-general"></a>


| Limit | Description | 
| --- | --- | 
|  Names in Step Functions  | State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/limits.html) Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\. | 

## Limits Related to Accounts<a name="service-limits-accounts"></a>


| Limit | Description | 
| --- | --- | 
|  Maximum number of registered activities  |  10,000  | 
| Maximum number of registered state machines | 10,000 | 
|  Maximum number of API actions  |  Beyond infrequent spikes, applications can be throttled if they make a large number of API actions in a very short period of time\.  | 
|  Maximum request size  |  1 MB per request\. This is the total data size per Step Functions API request, including the request header and all other associated request data\.  | 

## Limits Related to State Machine Executions<a name="service-limits-state-machine-executions"></a>


****  

| Limit | Description | 
| --- | --- | 
|  Maximum open executions  | 1,000,000\. Exceeding this limit will cause an ExecutionLimitExceeded error\. | 
|  Maximum execution time  |  1 year\. If an execution runs for more than the 1\-year limit, it will fail with a `States.Timeout` error and emit a `ExecutionsTimedout` CloudWatch metric\.  | 
|  Maximum execution history size  | 25,000 events\. If the execution history reaches this limit the execution will fail\. To avoid this, see [Avoid Reaching the History Limit](bp-history-limit.md)\. | 
|  Maximum execution idle time  |  1 year \(constrained by execution time limit\)  | 
| Maximum execution history retention time |  90 days\. After this time, you can no longer retrieve or view the execution history\. There is no further limit to the number of closed executions that Step Functions retains\.  | 

## Limits Related to Task Executions<a name="service-limits-task-executions"></a>


| Limit | Description | 
| --- | --- | 
|  Maximum task execution time  |  1 year \(constrained by execution time limit\)  | 
|  Maximum time Step Functions keeps a task in the queue  |  1 year \(constrained by execution time limit\)  | 
|  Maximum activity pollers per Amazon Resource Name \(ARN\)  |  1,000 pollers calling `GetActivityTask` per ARN\. Exceeding this limit results in this error: *"The maximum number of workers concurrently polling for activity tasks has been reached\."*  | 
|  Maximum input or result data size for a task, state, or execution  |  32,768 characters\. This limit affects tasks \(activity or Lambda function\), state or execution result data, and input data when scheduling a task, entering a state, or starting an execution\.  | 

## Limits Related to API Action Throttling<a name="service-limits-api-action-throttling"></a>

Some Step Functions API actions are throttled using a token bucket scheme to maintain service bandwidth\.

**Note**  
Throttling limits are per account, per AWS Region\. AWS Step Functions may increase both the bucket size and refill rate at any time\. Do not rely on these throttling rates to limit your costs\.

### Limits In US East \(N\. Virginia\), US West \(Oregon\), and EU \(Ireland\)<a name="service-limits-large-regions"></a>


| API Name | Bucket Size | Refill Rate per Second | 
| --- | --- | --- | 
| CreateActivity | 100 | 1 | 
| CreateStateMachine | 100 | 1 | 
| DeleteActivity | 100 | 1 | 
| DeleteStateMachine | 100 | 1 | 
| DescribeActivity | 200 | 1 | 
| DescribeExecution | 300 | 15 | 
| DescribeStateMachine | 200 | 20 | 
| DescribeStateMachineForExecution | 200 | 1 | 
| GetActivityTask | 3,000 | 500 | 
| GetExecutionHistory | 400 | 20 | 
| ListActivities | 100 | 10 | 
| ListExecutions | 200 | 5 | 
| ListStateMachines | 100 | 5 | 
| ListTagsForResource | 100 | 1 | 
| SendTaskFailure | 3,000 | 500 | 
| SendTaskHeartbeat | 3,000 | 500 | 
| SendTaskSuccess | 3,000 | 500 | 
| StartExecution | 1,300 | 300 | 
| StopExecution | 1,000 | 200 | 
| TagResource | 200 | 1 | 
| UntagResource | 200 | 1 | 
| UpdateStateMachine | 100 | 1 | 

### Limits In All Other Regions<a name="service-limits-other-regions"></a>


| API Name | Bucket Size | Refill Rate per Second | 
| --- | --- | --- | 
| CreateActivity | 100 | 1 | 
| CreateStateMachine | 100 | 1 | 
| DeleteActivity | 100 | 1 | 
| DeleteStateMachine | 100 | 1 | 
| DescribeActivity | 200 | 1 | 
| DescribeExecution | 250 | 10 | 
| DescribeStateMachine | 200 | 20 | 
| DescribeStateMachineForExecution | 200 | 1 | 
| GetActivityTask | 1,500 | 300 | 
| GetExecutionHistory | 400 | 20 | 
| ListActivities | 100 | 5 | 
| ListExecutions | 100 | 2 | 
| ListStateMachines | 100 | 5 | 
| ListTagsForResource | 100 | 1 | 
| SendTaskFailure | 1,500 | 300 | 
| SendTaskHeartbeat | 1,500 | 300 | 
| SendTaskSuccess | 1,500 | 300 | 
| StartExecution | 800 | 150 | 
| StopExecution | 500 | 25 | 
| TagResource | 200 | 1 | 
| UntagResource | 200 | 1 | 
| UpdateStateMachine | 200 | 1 | 

## Limits Related to State Throttling<a name="service-limits-api-state-throttling"></a>

Step Functions state transitions are throttled using a token bucket scheme to maintain service bandwidth\.

**Note**  
Throttling on the `StateTransition` service metric is reported as `ExecutionThrottled` in Amazon CloudWatch\. For more information, see the [`ExecutionThrottled` CloudWatch metric](procedure-cw-metrics.md#cloudwatch-step-functions-execution-metrics)\.


| Service Metric | Bucket Size | Refill Rate per Second  | 
| --- | --- | --- | 
|  `StateTransition` — *In US East \(N\. Virginia\), US West \(Oregon\), and EU \(Ireland\)*  |  5,000  |  1,500  | 
|  `StateTransition` — *All other regions* |  800  |  500  | 

## Restrictions Related to Tagging<a name="sfn-limits-tagging"></a>

Be aware of these restrictions when tagging Step Functions resources\.

**Note**  
Tagging restrictions cannot be increased like other limits\.


| Restriction | Description | 
| --- | --- | 
|  Maximum number of tags per resource  |  50  | 
|  Maximum key length  |  128 Unicode characters in UTF\-8  | 
|  Maximum value length  |  256 Unicode characters in UTF\-8  | 
|  Prefix restriction  |  Do not use the `aws:` prefix in your tag names or values because it is reserved for AWS use\. You can't edit or delete tag names or values with this prefix\. Tags with this prefix do not count against your tags per resource limit\.  | 
|  Character restrictions  |  Tags may only contain unicode letters, digits, whitespace, or these symbols: `_ . : / = + - @`  | 

## Requesting a Limit Increase<a name="sfn-limits-how-to-increase"></a>

Use the **Support Center** page in the AWS Management Console to request a limit increase for resources provided by AWS Step Functions on a per\-Region basis\. For more information, see [To Request a Limit Increase](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.