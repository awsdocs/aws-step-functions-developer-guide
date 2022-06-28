# Call AWS CodeBuild with Step Functions<a name="connect-codebuild"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized CodeBuild integration is different than the CodeBuild AWS SDK integration**  
The [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern is supported\.
After you call `StopBuild` or `StopBuildBatch`, the build or build batch is not immediately deletable until some internal work is completed within CodeBuild to finalize the state of the build or builds\. If you attempt to use `BatchDeleteBuilds` or `DeleteBuildBatch` during this period, the build or build batch may not be deleted\. The optimized service integrations for `BatchDeleteBuilds` and `DeleteBuildBatch` include an internal retry to simplify the use case of deleting immediately after stopping\.

The AWS Step Functions service integration with AWS CodeBuild enables you to use Step Functions to trigger, stop, and manage builds, and to share build reports\. Using Step Functions, you can design and run continuous integration pipelines for validating your software changes for applications\.

Not all APIs support all integration patterns, as shown in the following table\.


| API | Request Response | Run a Job \(\.sync\) | 
| --- | --- | --- | 
| StartBuild | ✓ | ✓ | 
| StopBuild | ✓ |  | 
| BatchDeleteBuilds | ✓ |  | 
| BatchGetReports | ✓ |  | 
| StartBuildBatch | ✓ | ✓ | 
| StopBuildBatch | ✓ |  | 
| RetryBuildBatch | ✓ | ✓ | 
| DeleteBuildBatch | ✓ |  | 

**Note**  
Parameters in Step Functions are expressed in `PascalCase`, even when the native service API is `camelCase`\. 

Supported CodeBuild APIs and syntax:
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestSyntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestSyntax)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_CreateEndpoint_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#API_StartBuild_ResponseSyntax)
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuild.html](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuild.html)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuild.html#API_StopBuild_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuild.html#API_StopBuild_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuild.html#API_StopBuild_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuild.html#API_StopBuild_ResponseSyntax)
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchDeleteBuilds.html](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchDeleteBuilds.html)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchDeleteBuilds.html#API_BatchDeleteBuilds_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchDeleteBuilds.html#API_BatchDeleteBuildss_RequestSyntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchDeleteBuilds.html#API_BatchDeleteBuildss_RequestSyntax)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchDeleteBuilds.html#API_BatchDeleteBuilds_ResponseSyntax)
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchGetReports.html](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchGetReports.html)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchGetReports.html#API_BatchGetReports_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchGetReports.html#API_BatchGetReports_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchGetReports.html#API_BatchGetReports_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_BatchGetReports.html#API_BatchGetReports_ResponseSyntax)
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#API_StartBuildBatch_ResponseSyntax)
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuildBatch.html](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuildBatch.html)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuildBatch.html#API_StopBuildBatch_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuildBatch.html#API_StopBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuildBatch.html#API_StopBuildBatch_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StopBuildBatch.html#API_StopBuildBatch_ResponseSyntax)
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_RequestParameters)
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RetryBuildBatch.html#API_RetryBuildBatch_ResponseSyntax)
+ [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_DeleteBuildBatch.html](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_DeleteBuildBatch.html)
  + [Request syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_DeleteBuildBatch.html#API_DeleteBuildBatch_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/codebuild/latest/APIReference/API_DeleteBuildBatch.html#API_DeleteBuildBatch_RequestParameters](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_DeleteBuildBatch.html#API_DeleteBuildBatch_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_DeleteBuildBatch.html#API_DeleteBuildBatch_ResponseSyntax)

## <a name="codebuild-notes"></a>

**Note**  
You can use the `JSONPath` recursive descent \(`..`\) operator for `BatchDeleteBuilds`\. This returns an array, and enables you to turn the `Arn` field from `StartBuild` into a plural `Ids` parameter, as shown in the following example\.  

```
"BatchDeleteBuilds": {
    "Type": "Task",
    "Resource": "arn:aws:states:::codebuild:batchDeleteBuilds",
    "Parameters": {
        "Ids.$": "$.Build..Arn"
    },
    "Next": "MyNextState"
},
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.