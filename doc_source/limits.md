# Quotas for Standard Workflows<a name="limits"></a>

AWS Step Functions places quotas on the sizes of certain state machine parameters, such as the number of API actions that you can make during a certain time period or the number of state machines that you can define\. Although these quotas are designed to prevent a misconfigured state machine from consuming all of the resources of the system, many aren't hard quotas\.

**Note**  
If a particular stage of your state machine execution or activity execution takes too long, you can configure a state machine timeout to cause a timeout event\.

**Topics**
+ [Quotas Related to State Machine Executions](#service-limits-state-machine-executions)
+ [Quotas Related to Task Executions](#service-limits-task-executions)
+ [Quotas Related to API Action Throttling](#service-limits-api-action-throttling)
+ [Quotas Related to State Throttling](#service-limits-api-state-throttling)
+ [Requesting a Quota Increase](#sfn-limits-how-to-increase)

## Quotas Related to State Machine Executions<a name="service-limits-state-machine-executions"></a>


****  

| Quota | Description | 
| --- | --- | 
|  Maximum execution time  |  1 year\. If an execution runs for more than the 1\-year maximum, it will fail with a `States.Timeout` error and emit a `ExecutionsTimedOut` CloudWatch metric\.  | 
|  Maximum execution history size  | 25,000 events\. If the execution history reaches this quota, the execution will fail\. To avoid this, see [Avoid Reaching the history quota](bp-history-limit.md)\. | 
|  Maximum execution idle time  |  1 year \(constrained by the maximum execution time\)  | 
| Maximum execution history retention time |  90 days\. After this time, you can no longer retrieve or view the execution history\. There is no further quota for the number of closed executions that Step Functions retains\.  | 

## Quotas Related to Task Executions<a name="service-limits-task-executions"></a>


| Quota | Description | 
| --- | --- | 
|  Maximum task execution time  |  1 year \(constrained by the maximum execution time\)  | 
|  Maximum time Step Functions keeps a task in the queue  |  1 year \(constrained by the maximum execution time\)  | 
|  Maximum activity pollers per Amazon Resource Name \(ARN\)  |  1,000 pollers calling `GetActivityTask` per ARN\. Exceeding this quota results in this error: *"The maximum number of workers concurrently polling for activity tasks has been reached\."*  | 
|  Maximum input or output size for a task, state, or execution  |  262,144 bytes of data as a UTF\-8 encoded string\. This quota affects tasks \(activity, Lambda function, or integrated service\), state or execution output, and input data when scheduling a task, entering a state, or starting an execution\.  | 

## Quotas Related to API Action Throttling<a name="service-limits-api-action-throttling"></a>

Some Step Functions API actions are throttled using a token bucket scheme to maintain service bandwidth\.

**Note**  
Throttling quotas are per account, per AWS Region\. AWS Step Functions may increase both the bucket size and refill rate at any time\. Do not rely on these throttling rates to limit your costs\.

### Quotas In US East \(N\. Virginia\), US West \(Oregon\), and Europe \(Ireland\)<a name="service-limits-large-regions-standard"></a>


| API Name | Bucket Size | Refill Rate per Second | 
| --- | --- | --- | 
| StartExecution | 1,300 | 300 | 

### Quotas In All Other Regions<a name="service-limits-other-regions-standard"></a>


| API Name | Bucket Size | Refill Rate per Second | 
| --- | --- | --- | 
| StartExecution | 800 | 150 | 

## Quotas Related to State Throttling<a name="service-limits-api-state-throttling"></a>

Step Functions state transitions are throttled using a token bucket scheme to maintain service bandwidth\.

**Note**  
Throttling on the `StateTransition` service metric is reported as `ExecutionThrottled` in Amazon CloudWatch\. For more information, see the [`ExecutionThrottled` CloudWatch metric](procedure-cw-metrics.md#cloudwatch-step-functions-execution-metrics)\.


| Service Metric | Bucket Size | Refill Rate per Second  | 
| --- | --- | --- | 
|  `StateTransition` — *In US East \(N\. Virginia\), US West \(Oregon\), and Europe \(Ireland\)*  |  5,000  |  1,500  | 
|  `StateTransition` — *All other regions* |  800  |  500  | 

## Requesting a Quota Increase<a name="sfn-limits-how-to-increase"></a>

Use the **Support Center** page in the AWS Management Console to request a quota increase for resources provided by AWS Step Functions on a per\-Region basis\. For more information, see [To Request a Quota Increase](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.