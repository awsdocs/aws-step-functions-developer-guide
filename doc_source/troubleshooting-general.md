# General troubleshooting<a name="troubleshooting-general"></a>

## I'm unable to create a state machine\.<a name="troubleshooting-general-unable-to-create"></a>

The IAM role associated with the state machine might not have [sufficient permissions](auth-and-access-control-sfn.md#access-control-sfn.title)\. Check the IAM role's permissions, including for AWS service integration tasks, X\-Ray, and CloudWatch logging\. Additional permissions are required for `.sync` task states\.  

## I'm unable to use a JsonPath to reference the previous task’s output\.<a name="troubleshooting-general-unable-to-use-json"></a>

For a JsonPath, a JSON key must end with `.$`\. This means a JsonPath can only be used in a key\-value pair\. If you want to use a JsonPath other places, such as an array, you can use [intrinsic functions](amazon-states-language-intrinsic-functions.md#amazon-states-language-intrinsic-functions.title)\. For example, you could use something similar to the following:

**Task A output:**

```
{
    "sample": "test"
}
```

**Task B:**

```
{
    "JsonPathSample.$": "$.sample"
}
```

**Tip**  
Use the [ data flow simulator in the Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/simulator) to test JSON path syntax, to better understand how data is manipulated within a state, and to see how data is passed between states\.

## There was a delay in state transitions\.<a name="troubleshooting-general-state-transition-delay"></a>

For standard workflows, there is a limit on the number of state transitions\. When you exceed the state transition limit, Step Functions delays state transitions until the bucket for the quota is filled\. State transition limit throttling can be monitored by reviewing the `StateTransition` metric in the [Dimension for Step Functions Service Metrics](procedure-cw-metrics.md#cloudwatch-step-functions-service-metrics-dimensions) section of the CloudWatch Metrics page\.

## When I start new Standard Workflow executions, they fail with the `ExecutionLimitExceeded` error\.<a name="troubleshooting-general-unable-to-start-standard-workflows"></a>

Step Functions has a limit of 1,000,000 open executions for each AWS account\. If you exceed this limit, Step Functions throws an `ExecutionLimitExceeded` error\. This limit does not apply to Express Workflows\. You can use the following [CloudWatch Metrics math](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/using-metric-math.html) to approximate the number of open executions: `ExecutionsStarted - (ExecutionsSucceeded + ExecutionsTimedOut + ExecutionsFailed + ExecutionsAborted)`\.

## A failure on one branch in a parallel state causes the whole execution to fail\.<a name="troubleshooting-general-branch-failure-causes-execution-failure"></a>

This is an expected behavior\. To avoid encountering failures when using a parallel state, configure Step Functions to [catch errors](concepts-error-handling.md#error-handling-fallback-states.title) thrown from each branch\. 