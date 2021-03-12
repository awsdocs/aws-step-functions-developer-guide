# Quotas<a name="limits-overview"></a>

AWS Step Functions places quotas on the sizes of certain state machine parameters, such as the number of API actions that you can make during a certain time period or the number of state machines that you can define\. Although these quotas are designed to prevent a misconfigured state machine from consuming all of the resources of the system, many aren't hard quotas\.

**Note**  
If a particular stage of your state machine execution or activity execution takes too long, you can configure a state machine timeout to cause a timeout event\.

 Some quotas are the same for Standard workflows and Express workflows, and are shown below\. Quotas that are different can be found on the [Quotas for Standard Workflows](limits.md) and [Quotas for Express Workflows](express-limits.md)\. 

**Topics**
+ [Quotas for Standard Workflows](limits.md)
+ [Quotas for Express Workflows](express-limits.md)
+ [General Quotas](#service-limits-general)
+ [Quotas Related to Accounts](#service-limits-accounts)
+ [Quotas Related to API Action Throttling](#service-limits-api-action-throttling-general)
+ [Restrictions Related to Tagging](#sfn-limits-tagging)
+ [Requesting a Quota Increase](#sfn-limits-how-to-increase-overview)

## General Quotas<a name="service-limits-general"></a>


| Quota | Description | 
| --- | --- | 
|  Names in Step Functions  | State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/limits-overview.html) Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\. | 

## Quotas Related to Accounts<a name="service-limits-accounts"></a>


| Quota | Description | 
| --- | --- | 
|  Maximum number of registered activities  |  10,000  | 
| Maximum number of registered state machines | 10,000 | 
|  Maximum number of API actions  |  Beyond infrequent spikes, applications can be throttled if they make a large number of API actions in a very short period of time\.  | 
|  Maximum request size  |  1 MB per request\. This is the total data size per Step Functions API request, including the request header and all other associated request data\.  | 
|  Maximum open executions per account  | 1,000,000 executions per AWS account\. Exceeding this will cause an ExecutionLimitExceeded error\. Does not apply to Express Workflows\. | 

## Quotas Related to API Action Throttling<a name="service-limits-api-action-throttling-general"></a>

Some Step Functions API actions are throttled using a token bucket scheme to maintain service bandwidth\.

**Note**  
Throttling quotas are per account, per AWS Region\. AWS Step Functions may increase both the bucket size and refill rate at any time\. Do not rely on these throttling rates to limit your costs\.

### Quotas In US East \(N\. Virginia\), US West \(Oregon\), and Europe \(Ireland\)<a name="service-limits-large-regions"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/limits-overview.html)

### Quotas In All Other Regions<a name="service-limits-other-regions"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/limits-overview.html)

## Restrictions Related to Tagging<a name="sfn-limits-tagging"></a>

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

## Requesting a Quota Increase<a name="sfn-limits-how-to-increase-overview"></a>

Use the **Support Center** page in the AWS Management Console to request a quota increase for resources provided by AWS Step Functions on a per\-Region basis\. For more information, see [To Request a Quota Increase](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.