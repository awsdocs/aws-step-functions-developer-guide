# Call Athena with Step Functions<a name="connect-athena"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized Athena integration is different than the Athena AWS SDK integration**  
The [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern is supported\.
There are no optimizations for the [Request Response](connect-to-resource.md#connect-default) integration pattern\.
The [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration pattern is not supported\.

 The AWS Step Functions service integration with Amazon Athena enables you to use Step Functions to start and stop query execution, and get query results\. Using Step Functions, you can run ad\-hoc or scheduled data queries, and retrieve results targeting your S3 data lakes\. Athena is serverless, so there is no infrastructure to set up or manage, and you pay only for the queries you run\.

To integrate AWS Step Functions with Amazon Athena, you use the provided Athena service integration APIs\.

The service integration APIs are the same as the corresponding Athena APIs\. Not all APIs support all integration patterns, as shown in the following table\.


| API | Request Response | Run a Job \(\.sync\) | 
| --- | --- | --- | 
| StartQueryExecution | ✓ | ✓ | 
| StopQueryExecution | ✓ |  | 
| GetQueryExecution | ✓ |  | 
| GetQueryResults | ✓ |  | 

Supported Amazon Athena APIs:

**Note**  
There is a quota for the maximum input or result data size for a task in Step Functions\. This restricts you to 262,144 bytes of data as a UTF\-8 encoded string when you send to, or receive data from, another service\. See [Quotas related to state machine executions](limits-overview.md#service-limits-state-machine-executions)\.
+ [https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html)
  + [Request syntax](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestSyntax)
  + Supported parameters: 
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters)
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters)
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters)
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters)
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_RequestParameters)
  + [https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_ResponseSyntax](https://docs.aws.amazon.com/athena/latest/APIReference/API_StartQueryExecution.html#API_StartQueryExecution_ResponseSyntax)
+ [https://docs.aws.amazon.com/athena/latest/APIReference/API_StopQueryExecution.html](https://docs.aws.amazon.com/athena/latest/APIReference/API_StopQueryExecution.html)
  + [Request syntax](https://docs.aws.amazon.com/athena/latest/APIReference/API_StopQueryExecution.html#API_StopQueryExecution_RequestSyntax)
  + Supported parameters: 
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_StopQueryExecution.html#API_StopQueryExecution_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_StopQueryExecution.html#API_StopQueryExecution_RequestParameters)
+ [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html)
  + [Request syntax](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html#API_GetQueryExecution_RequestSyntax)
  + Supported parameters: 
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html#API_GetQueryExecution_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html#API_GetQueryExecution_RequestParameters)
  + [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html#API_GetQueryExecution_ResponseSyntax](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html#API_GetQueryExecution_ResponseSyntax)
+ [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html)
  + [Request syntax](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryExecution.html#API_GetQueryExecution_RequestSyntax)
  + Supported parameters: 
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_RequestParameters)
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_RequestParameters)
    + [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_RequestParameters](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_RequestParameters)
  + [https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_ResponseSyntax](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html#API_GetQueryResults_ResponseSyntax)

The following includes a Task state that starts an Athena query\.

```
"Start an Athena query": {
  "Type": "Task",
  "Resource": "arn:aws:states:::athena:startQueryExecution.sync",
  "Parameters": {
    "QueryString": "SELECT * FROM \"myDatabase\".\"myTable\" limit 1",
    "WorkGroup": "primary",
    "ResultConfiguration": {
       "OutputLocation": "s3://athenaQueryResult"
    }
  },
  "Next": "Get results of the query"
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.