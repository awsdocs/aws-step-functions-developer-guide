# Manage Amazon SageMaker Training Jobs With Step Functions<a name="connectors-sagemaker"></a>

Step Functions can control some AWS services directly from the Amazon States Language\. For more information, see:
+ [Service Integrations](concepts-connectors.md)
+ [Pass Parameters to a Service API](connectors-parameters.md)

Supported Amazon SageMaker APIs and syntax:
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html#API_CreateTrainingJob_ResponseSyntax)
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#API_CreateTransformJob_ResponseSyntax)
**Note**  
AWS Step Functions will not automatically create a policy for `CreateTransformJob` when you create a state machine that integrates with Amazon SageMaker\. You must attach an inline policy to the created role\. For more infomation, see this example IAM policy: [`CreateTrainingJob`](sagemaker-iam.md#sagemaker-iam-createtrainingjob)\.

The following includes a `Task` state that creates a Amazon SageMaker transform job, specifying the Amazon S3 location for `DataSource` and `TransformOutput`\.

```
{
"SageMaker CreateTransformJob": {
  "Type": "Task",
  "Resource": "arn:aws:states:::sagemaker:createTransformJob.sync",
  "Parameters": {
    "ModelName": "SageMakerCreateTransformJobModel-9iFBKsYti9vr",
    "TransformInput": {
      "CompressionType": "None",
      "ContentType": "text/csv",
      "DataSource": {
        "S3DataSource": {
          "S3DataType": "S3Prefix",
          "S3Uri": "s3://my-s3bucket-example-1/TransformJobDataInput.txt"
        }
      }
    },
    "TransformOutput": {
      "S3OutputPath": "s3://my-s3bucket-example-1/TransformJobOutputPath"
    },
    "TransformResources": {
      "InstanceCount": 1,
      "InstanceType": "ml.m4.xlarge"
    },
    "TransformJobName": "sfn-binary-classification-prediction"
  },
  "Next": "ValidateOutput"
},
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](connectors-iam-templates.md)\.