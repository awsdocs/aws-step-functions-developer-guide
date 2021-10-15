# Manage AWS Batch with Step Functions<a name="connect-batch"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

Supported AWS Batch APIs:

**How the Optimized AWS Batch integration is different than the AWS Batch AWS SDK integration**  
The [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern is available\.
Note that there are no optimizations for the [Request Response](connect-to-resource.md#connect-default) or [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration patterns\.

**Note**  
Parameters in Step Functions are expressed in `PascalCase`, even when the native service API is `camelCase`\. 
+ [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html)
  + [Request syntax](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#API_SubmitJob_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-arrayProperties](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-arrayProperties)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-containerOverrides](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-containerOverrides)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-dependsOn](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-dependsOn)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-jobDefinition](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-jobDefinition)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-jobName](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-jobName)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-jobQueue](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-jobQueue)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-parameters](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-parameters)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-retryStrategy](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-retryStrategy)
    + [https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-timeout](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#Batch-SubmitJob-request-timeout)
  + [Response syntax](https://docs.aws.amazon.com/batch/latest/APIReference/API_SubmitJob.html#API_SubmitJob_ResponseSyntax)

The following includes a `Task` state that submits an AWS Batch job and waits for it to complete\.

```
{
  "StartAt": "BATCH_JOB",
  "States": {
    "BATCH_JOB": {
      "Type": "Task",
      "Resource": "arn:aws:states:::batch:submitJob.sync",
      "Parameters": {
        "JobDefinition": "preprocessing",
        "JobName": "PreprocessingBatchJob",
        "JobQueue": "SecondaryQueue",
        "Parameters.$": "$.batchjob.parameters",
        "ContainerOverrides": {
          "ResourceRequirements": [
            {
              "Type": "VCPU",
              "Value": "4"
            }
          ]
        }
      },
      "End": true
    }
  }
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.