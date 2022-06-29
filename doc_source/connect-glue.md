# Manage AWS Glue Jobs with Step Functions<a name="connect-glue"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized AWS Glue integration is different than the AWS GlueAWS SDK integration**  
The [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern is available\.
The `JobName` field is extracted from the request and inserted into the response, which normally only contains `JobRunID`\.

Supported AWS Glue API:
+ [https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-jobs-runs.html#aws-glue-api-jobs-runs-StartJobRun](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-jobs-runs.html#aws-glue-api-jobs-runs-StartJobRun)

The following includes a `Task` state that starts an AWS Glue job\.

```
"Glue StartJobRun": {
      "Type": "Task",
      "Resource": "arn:aws:states:::glue:startJobRun.sync",
      "Parameters": {
        "JobName": "GlueJob-JTrRO5l98qMG"
      },
      "Next": "ValidateOutput"
    },
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.