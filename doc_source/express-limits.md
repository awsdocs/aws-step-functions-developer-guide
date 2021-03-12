# Quotas for Express Workflows<a name="express-limits"></a>

AWS Step Functions places quotas on the sizes of certain state machine parameters, such as the number of API actions that you can make during a certain time period or the number of state machines that you can define\. Although these quotas are designed to prevent a misconfigured state machine from consuming all of the resources of the system, many aren't hard quotas\.

**Note**  
If a particular stage of your state machine execution takes too long, you can configure a state machine timeout to cause a timeout event\.

**Topics**
+ [Quotas Related to State Machine Executions](#service-limits-state-machine-executions-express)
+ [Quotas Related to Task Executions](#service-limits-task-executions-express)
+ [Quotas Related to API Action Throttling](#service-limits-api-action-throttling-express)
+ [Quotas Related to State Throttling](#service-limits-api-state-throttling-express)
+ [Requesting a Quota Increase](#sfn-limits-how-to-increase)

## Quotas Related to State Machine Executions<a name="service-limits-state-machine-executions-express"></a>


****  

| Quota | Description | 
| --- | --- | 
|  Maximum execution time  |  5 minutes\. If an execution runs for more than the 5\-minute maximum, it will fail with a `States.Timeout` error and emit a `ExecutionsTimedOut` CloudWatch metric\.  | 
|  Maximum execution history size  | Unlimited\. | 
|  Maximum execution idle time  |  5 minutes \(constrained by the maximum execution time\)  | 
| Maximum execution history retention time |  To see execution history, Amazon CloudWatch Logs logging must be configured\. For more information, [Logging using CloudWatch Logs](cw-logs.md)\.  | 

## Quotas Related to Task Executions<a name="service-limits-task-executions-express"></a>


| Quota | Description | 
| --- | --- | 
|  Maximum task execution time  |  5 minutes \(constrained by the maximum execution time\)  | 
|  Maximum time Step Functions keeps a task in the queue  |  5 minutes \(constrained by maximum execution time\)  | 
|  Maximum activity pollers per Amazon Resource Name \(ARN\)  |  Does not apply to Express Workflows\.  | 
|  Maximum input or output size for a task, state, or execution  |  262,144 bytes of data as a UTF\-8 encoded string\. This quota affects tasks \(activity, Lambda function, or integrated service\), state or execution output, and input data when scheduling a task, entering a state, or starting an execution\.  | 

## Quotas Related to API Action Throttling<a name="service-limits-api-action-throttling-express"></a>

Some Step Functions API actions are throttled using a token bucket scheme to maintain service bandwidth\.

**Note**  
Throttling quotas are per account, per AWS Region\. AWS Step Functions may increase both the bucket size and refill rate at any time\. Do not rely on these throttling rates to limit your costs\.

### Quotas for `StartExecution` \- All Regions<a name="service-limites-start-execution-express"></a>


| API Name | Bucket Size | Refill Rate per Second | 
| --- | --- | --- | 
| StartExecution | 6,000 | 6,000 | 

## Quotas Related to State Throttling<a name="service-limits-api-state-throttling-express"></a>

Step Functions state transitions are throttled using a token bucket scheme to maintain service bandwidth\.

**Note**  
Throttling on the `StateTransition` service metric is reported as `ExecutionThrottled` in Amazon CloudWatch\. For more information, see the [`ExecutionThrottled` CloudWatch metric](procedure-cw-metrics.md#cloudwatch-step-functions-execution-metrics)\.


| Service Metric | Bucket Size | Refill Rate per Second  | 
| --- | --- | --- | 
|  `StateTransition` — *All regions* |  Unlimited  |  Unlimited  | 

## Requesting a Quota Increase<a name="sfn-limits-how-to-increase"></a>

Use the **Support Center** page in the AWS Management Console to request a quota increase for resources provided by AWS Step Functions on a per\-Region basis\. For more information, see [To Request a Quota Increase](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.