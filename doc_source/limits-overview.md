# Quotas<a name="limits-overview"></a>

AWS Step Functions places quotas on the sizes of certain state machine parameters, such as the number of API actions during a certain time period or the number of state machines that you can define\. Although these quotas are designed to prevent a misconfigured state machine from consuming all of the resources of the system, many aren't hard quotas\.  Use the **Support Center** page in the AWS Management Console to request a quota increase for resources provided by AWS Step Functions on a per\-Region basis\. For more information, see [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.

**Note**  
If a particular stage of your state machine execution or activity execution takes too long, you can configure a state machine timeout to cause a timeout event\.

**Topics**
+ [General quotas](#service-limits-general)
+ [Quotas related to accounts](#service-limits-accounts)
+ [Quotas related to state throttling](#service-limits-api-state-throttling)
+ [Quotas related to API action throttling](#service-limits-api-action-throttling-general)
+ [Quotas related to state machine executions](#service-limits-state-machine-executions)
+ [Quotas related to task executions](#service-limits-task-executions)
+ [Other quotas](#service-limits-large-regions)
+ [Restrictions related to tagging](#sfn-limits-tagging)

## General quotas<a name="service-limits-general"></a>


| Quota | Description | 
| --- | --- | 
|  Names in Step Functions  | State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/limits-overview.html) Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\. | 

## Quotas related to accounts<a name="service-limits-accounts"></a>


| Resource | Default quota | Can be increased to | 
| --- | --- | --- | 
| Maximum number of registered state machines | 10,000 | 22,000 to 23,000 | 
|  Maximum number of registered activities  |  10,000  |  Tens of thousands  | 
|  Maximum request size  |  1 MB per request\. This is the total data size per Step Functions API request, including the request header and all other associated request data\.  |  Hard quota   | 
|  Maximum open executions per account  | 1,000,000 executions per AWS Region\. Exceeding this will cause an ExecutionLimitExceeded error\. Does not apply to Express Workflows\. |  Millions  | 
|   Synchronous Express Workflows concurrent executions   |   1,000\-3,700 \(varies per region\)    |   Tens of thousands\. Synchronous Express Workflows do not contribute to the existing account capacity limits\. For more information, see [Synchronous and Asynchronous Express Workflows](concepts-express-synchronous.md)\.   | 

## Quotas related to state throttling<a name="service-limits-api-state-throttling"></a>

Step Functions state transitions are throttled using a token bucket scheme to maintain service bandwidth\. Standard Workflows and Express Workflows have different state transition throttling\. Standard Workflows quotas are soft quotas and can be increased\.

**Note**  
Throttling on the `StateTransition` service metric is reported as `ExecutionThrottled` in Amazon CloudWatch\. For more information, see the [`ExecutionThrottled` CloudWatch metric](procedure-cw-metrics.md#cloudwatch-step-functions-execution-metrics)\.


|   | Standard | Express | 
| --- |--- |--- |
| Service metric | Bucket size | Refill rate per second  | Bucket size | Refill rate per second  | 
| --- |--- |--- |--- |--- |
|  `StateTransition` — *In US East \(N\. Virginia\), US West \(Oregon\), and Europe \(Ireland\)*  |  5,000  |  1,500  |  Unlimited  |  Unlimited  | 
|  `StateTransition` — *All other regions*  |  800  |  500  |  Unlimited  |  Unlimited  | 

## Quotas related to API action throttling<a name="service-limits-api-action-throttling-general"></a>

Some Step Functions API actions are throttled using a token bucket scheme to maintain service bandwidth\. These quotas are soft quotas and can be increased\.

**Note**  
Throttling quotas are per account, per AWS Region\. AWS Step Functions may increase both the bucket size and refill rate at any time\. Synchronous Express execution API calls do not contribute to the existing account capacity limits\. While Step Functions will provide capacity on demand and will automatically scale with sustained workload, surges in workload may be throttled until capacity is available\. Do not rely on these throttling rates to limit your costs\. 


|   | Standard | Express | 
| --- |--- |--- |
| API name | Bucket size | Refill rate per second | Bucket size | Refill rate per second | 
| --- |--- |--- |--- |--- |
| StartExecution — In US East \(N\. Virginia\), US West \(Oregon\), and Europe \(Ireland\) | 1,300 | 300 | 6,000 | 6,000 | 
| StartExecution — All other regions | 800 | 150 | 6,000 | 6,000 | 

## Quotas related to state machine executions<a name="service-limits-state-machine-executions"></a>

The following table describes quotas related to state machine executions\. State machine execution quotas are hard quotas that can't be changed\.


****  

| Quota | Standard | Express | 
| --- | --- | --- | 
|  Maximum execution time  |  1 year\. If an execution runs for more than the 1\-year maximum, it will fail with a `States.Timeout` error and emit a `ExecutionsTimedOut` CloudWatch metric\.  |  5 minutes\. If an execution runs for more than the 5\-minute maximum, it will fail with a `States.Timeout` error and emit a `ExecutionsTimedOut` CloudWatch metric\.  | 
|  Maximum execution history size  | 25,000 events in a single state machine execution history\. If the execution history reaches this quota, the execution will fail\. To avoid this, see [Avoid reaching the history quota](bp-history-limit.md)\. | Unlimited\. | 
|  Maximum execution idle time  |  1 year \(constrained by the maximum execution time\)  |  5 minutes \(constrained by the maximum execution time\)  | 
| Execution history retention time |  90 days\. After this time, you can no longer retrieve or view the execution history\. There is no further quota for the number of closed executions that Step Functions retains\.  |  To see execution history, Amazon CloudWatch Logs logging must be configured\. For more information, [Logging using CloudWatch Logs](cw-logs.md)\.  | 

## Quotas related to task executions<a name="service-limits-task-executions"></a>

The following table describes quotas related to task executions, and are hard quotas that can't be changed\.


| Quota | Standard | Express | 
| --- | --- | --- | 
|  Maximum task execution time  |  1 year \(constrained by the maximum execution time\)  |  5 minutes \(constrained by the maximum execution time\)  | 
|  Maximum time Step Functions keeps a task in the queue  |  1 year \(constrained by the maximum execution time\)  |  5 minutes \(constrained by the maximum execution time\)  | 
|  Maximum activity pollers per Amazon Resource Name \(ARN\)  |  1,000 pollers calling `GetActivityTask` per ARN\. Exceeding this quota results in this error: *"The maximum number of workers concurrently polling for activity tasks has been reached\."*  |  Does not apply to Express Workflows\.  | 
|  Maximum input or output size for a task, state, or execution  |  262,144 bytes of data as a UTF\-8 encoded string\. This quota affects tasks \(activity, Lambda function, or integrated service\), state or execution output, and input data when scheduling a task, entering a state, or starting an execution\.  |  262,144 bytes of data as a UTF\-8 encoded string\. This quota affects tasks \(activity, Lambda function, or integrated service\), state or execution output, and input data when scheduling a task, entering a state, or starting an execution\.  | 

## Other quotas<a name="service-limits-large-regions"></a>

These quotas are soft quotas and can be increased\.


|   | In US East \(N\. Virginia\), US West \(Oregon\), and Europe \(Ireland\) | All other regions | 
| --- |--- |--- |
| API name | Bucket size | Refill rate per second | Bucket size | Refill rate per second | 
| --- |--- |--- |--- |--- |
| CreateActivity | 100 | 1 | 100 | 1 | 
| CreateStateMachine | 100 | 1 | 100 | 1 | 
| DeleteActivity | 100 | 1 | 100 | 1 | 
| DeleteStateMachine | 100 | 1 | 100 | 1 | 
| DescribeActivity | 200 | 1 | 200 | 1 | 
| DescribeExecution | 300 | 15 | 250 | 10 | 
| DescribeStateMachine | 200 | 20 | 200 | 20 | 
| DescribeStateMachineForExecution | 200 | 1 | 200 | 1 | 
| GetActivityTask | 3,000 | 500 | 1,500 | 300 | 
| GetExecutionHistory | 400 | 20 | 400 | 20 | 
| ListActivities | 100 | 10 | 100 | 5 | 
| ListExecutions | 200 | 5 | 100 | 2 | 
| ListStateMachines | 100 | 5 | 100 | 5 | 
| ListTagsForResource | 100 | 1 | 100 | 1 | 
| SendTaskFailure | 3,000 | 500 | 1,500 | 300 | 
| SendTaskHeartbeat | 3,000 | 500 | 1,500 | 300 | 
| SendTaskSuccess | 3,000 | 500 | 1,500 | 300 | 
| StopExecution | 1,000 | 200 | 500 | 25 | 
| TagResource | 200 | 1 | 200 | 1 | 
| UntagResource | 200 | 1 | 200 | 1 | 
| UpdateStateMachine | 100 | 1 | 100 | 1 | 

## Restrictions related to tagging<a name="sfn-limits-tagging"></a>

Be aware of these restrictions when tagging Step Functions resources\.

**Note**  
Tagging restrictions cannot be increased like other quotas\.


| Restriction | Description | 
| --- | --- | 
|  Maximum number of tags per resource  |  50  | 
|  Maximum key length  |  128 Unicode characters in UTF\-8  | 
|  Maximum value length  |  256 Unicode characters in UTF\-8  | 
|  Prefix restriction  |  Do not use the `aws:` prefix in your tag names or values because it is reserved for AWS use\. You can't edit or delete tag names or values with this prefix\. Tags with this prefix do not count against your tags per resource quota\.  | 
|  Character restrictions  |  Tags may only contain Unicode letters, digits, whitespace, or these symbols: `_ . : / = + - @`  | 