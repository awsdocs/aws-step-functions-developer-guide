# Call Athena with Step Functions<a name="connect-athena"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information, see the following:
+ [Service Integrations](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

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
There is a quota for the maximum input or result data size for a task in Step Functions\. This restricts you to 262,144 bytes of data as a UTF\-8 encoded string when you send to, or receive data from, another service\. See [Quotas Related to State Machine Executions](limits.md#service-limits-state-machine-executions)\.
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

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](service-integration-iam-templates.md)\.