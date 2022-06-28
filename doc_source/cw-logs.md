# Logging using CloudWatch Logs<a name="cw-logs"></a>

Standard Workflows record execution history in AWS Step Functions, although you can optionally configure logging to Amazon CloudWatch Logs\.

Unlike Standard Workflows, Express Workflows don't record execution history in AWS Step Functions\. To see execution history and results for an Express Workflow, you must configure logging to Amazon CloudWatch Logs\. Publishing logs doesn't block or slow down executions\.

**Note**  
When you configure logging, [CloudWatch Logs charges](http://aws.amazon.com/cloudwatch/pricing) will apply\.

## Configure logging<a name="monitoring-logging-configure"></a>

When you create a Standard Workflow using the Step Functions console, it will not be configured to enable logging to CloudWatch Logs\. An Express Workflow created using the Step Functions console will by default be configured to enable logging to CloudWatch Logs\. 

For Express workflows, Step Functions can create a role with the necessary AWS Identity and Access Management \(IAM\) policy for CloudWatch Logs\. If you create a Standard Workflow, or an Express Workflow using the API, CLI, or AWS CloudFormation, Step Functions will not enable logging by default, and you will need ensure your role has the necessary permissions\.

For each execution started from the console, Step Functions provides a link to CloudWatch Logs, configured with the correct filter to fetch log events specific for that execution\. 

To configure logging, you can pass the [LoggingConfiguration](https://docs.aws.amazon.com/step-functions/latest/apireference/API_LoggingConfiguration.html) parameter when using [CreateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateStateMachine.html) or [UpdateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_UpdateStateMachine.html)\. You can further analyze your data in CloudWatch Logs by using CloudWatch Logs Insights\. For more information see [Analyzing Log Data with CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)\.

## CloudWatch Logs payloads<a name="cloudwatch-payload"></a>

Execution history events may contain either input or output properties in their definitions\.  If escaped input or escaped output sent to CloudWatch Logs exceeds 248KB, it will be truncated as a result of CloudWatch Logs quotas\.
+  You can determine whether a payload has been truncated by reviewing the `inputDetails` and `outputDetails` properties\. For more information, see the [`HistoryEventExecutionDataDetails` Data Type](https://docs.aws.amazon.com/step-functions/latest/apireference/API_HistoryEventExecutionDataDetails.html)\. 
+  For Standard Workflows, you can see the full execution history by using [https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetExecutionHistory.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetExecutionHistory.html)\. 
+  `GetExecutionHistory` is not available for Express Workflows\. If you want to see the full input and output, you can use Amazon S3 ARNs\. For more information, see [Use Amazon S3 ARNs instead of passing large payloads](avoid-exec-failures.md)\. 

## IAM Policies for logging to CloudWatch Logs<a name="cloudwatch-iam-policy"></a>

You will also need to configure your state machine's execution IAM role to have the proper permission to log to CloudWatch Logs as shown in the following example\.

**IAM policy example**  
The following is an example policy you can use to configure your permissions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogDelivery",
                "logs:GetLogDelivery",
                "logs:UpdateLogDelivery",
                "logs:DeleteLogDelivery",
                "logs:ListLogDeliveries",
                "logs:PutLogEvents",
                "logs:PutResourcePolicy",
                "logs:DescribeResourcePolicies",
                "logs:DescribeLogGroups"
            ],
            "Resource": "*"
        }
    ]
}
```

If you're unable to access the CloudWatch Logs, make sure you've done the following:

1. Configured your state machine's execution IAM role to have the proper permission to log to CloudWatch Logs\.

   If you're using the `[CreateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateStateMachine.html)` or `[UpdateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_UpdateStateMachine.html)` requests, make sure you've specified the IAM role in the `roleArn` parameter containing the permissions as shown in the [preceding example](#iam-policy-eg-for-cwl)\.

1. Checked the CloudWatch Logs resource policy doesn't exceed the 5120 character limit for CloudWatch Logs resource policies\.

   If you've exceeded the character limit, remove unnecessary permissions from the CloudWatch Logs resource policy, or prefix the log group name with `/aws/vendedlogs`, which will grant permissions to the log group without appending more characters to the resource policy\. When you create a log group in the Step Functions console, the log group names are prefixed with `/aws/vendedlogs/states`\. For more information, see [Amazon CloudWatch Logs resource policy size restrictions](bp-cwl.md)\.