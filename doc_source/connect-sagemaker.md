# Manage SageMaker with Step Functions<a name="connect-sagemaker"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized SageMaker integration is different than the SageMaker AWS SDK integration**  
The [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern is supported\.
There are no optimizations for the [Request Response](connect-to-resource.md#connect-default) integration pattern\.
The [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration pattern is not supported\.

Supported SageMaker APIs and syntax:
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html#API_CreateEndpoint_ResponseSyntax)
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpointConfig.html#API_CreateEndpointConfig_ResponseSyntax)
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html#API_CreateHyperParameterTuningJob_ResponseSyntax)
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_ResponseSyntax)
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateModel.html#API_CreateModel_RequestParameters)
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateProcessingJob.html#API_CreateProcessingJob_ResponseSyntax)
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
**Note**  
AWS Step Functions will not automatically create a policy for `CreateTransformJob`\. You must attach an inline policy to the created role\. For more information, see this example IAM policy: [`CreateTrainingJob`](sagemaker-iam.md#sagemaker-iam-createtrainingjob)\.
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
+ [https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html)
  + [Request syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html#API_UpdateEndpoint_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html#API_UpdateEndpoint_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html#API_UpdateEndpoint_RequestParameters)
    + [https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html#API_UpdateEndpoint_RequestParameters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html#API_UpdateEndpoint_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sagemaker/latest/dg/API_UpdateEndpoint.html#API_UpdateEndpoint_ResponseSyntax)

## SageMaker Transform Job Example<a name="sagemaker-example-transform"></a>

The following includes a `Task` state that creates an Amazon SageMaker transform job, specifying the Amazon S3 location for `DataSource` and `TransformOutput`\.

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

## SageMaker Training Job Example<a name="sagemaker-example-training"></a>

The following includes a `Task` state that creates an Amazon SageMaker training job\.

```
{  
   "SageMaker CreateTrainingJob":{  
      "Type":"Task",
      "Resource":"arn:aws:states:::sagemaker:createTrainingJob.sync",
      "Parameters":{  
         "TrainingJobName":"search-model",
         "ResourceConfig":{  
            "InstanceCount":4,
            "InstanceType":"ml.c4.8xlarge",
            "VolumeSizeInGB":20
         },
         "HyperParameters":{  
            "mode":"batch_skipgram",
            "epochs":"5",
            "min_count":"5",
            "sampling_threshold":"0.0001",
            "learning_rate":"0.025",
            "window_size":"5",
            "vector_dim":"300",
            "negative_samples":"5",
            "batch_size":"11"
         },
         "AlgorithmSpecification":{  
            "TrainingImage":"...",
            "TrainingInputMode":"File"
         },
         "OutputDataConfig":{  
            "S3OutputPath":"s3://bucket-name/doc-search/model"
         },
         "StoppingCondition":{  
            "MaxRuntimeInSeconds":100000
         },
         "RoleArn":"arn:aws:iam::123456789012:role/docsearch-stepfunction-iam-role",
         "InputDataConfig":[  
            {  
               "ChannelName":"train",
               "DataSource":{  
                  "S3DataSource":{  
                     "S3DataType":"S3Prefix",
                     "S3Uri":"s3://bucket-name/doc-search/interim-data/training-data/",
                     "S3DataDistributionType":"FullyReplicated"
                  }
               }
            }
         ]
      },
      "Retry":[  
         {  
            "ErrorEquals":[  
               "SageMaker.AmazonSageMakerException"
            ],
            "IntervalSeconds":1,
            "MaxAttempts":100,
            "BackoffRate":1.1
         },
         {  
            "ErrorEquals":[  
               "SageMaker.ResourceLimitExceededException"
            ],
            "IntervalSeconds":60,
            "MaxAttempts":5000,
            "BackoffRate":1
         },
         {  
            "ErrorEquals":[  
               "States.Timeout"
            ],
            "IntervalSeconds":1,
            "MaxAttempts":5,
            "BackoffRate":1
         }
      ],
      "Catch":[  
         {  
            "ErrorEquals":[  
               "States.ALL"
            ],
            "ResultPath":"$.cause",
            "Next":"Sagemaker Training Job Error"
         }
      ],
      "Next":"Delete Interim Data Job"
   }
}
```

## SageMaker Labeling Job Example<a name="sagemaker-example-labeling"></a>

The following includes a `Task` state that creates an Amazon SageMaker labeling job\.

```
{
  "StartAt": "SageMaker CreateLabelingJob",
  "TimeoutSeconds": 3600,
  "States": {
    "SageMaker CreateLabelingJob": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sagemaker:createLabelingJob.sync",
      "Parameters": {
        "HumanTaskConfig": {
          "AnnotationConsolidationConfig": {
            "AnnotationConsolidationLambdaArn": "arn:aws:lambda:us-west-2:123456789012:function:ACS-TextMultiClass"
          },
          "NumberOfHumanWorkersPerDataObject": 1,
          "PreHumanTaskLambdaArn": "arn:aws:lambda:us-west-2:123456789012:function:PRE-TextMultiClass",
          "TaskDescription": "Classify the following text",
          "TaskKeywords": [
            "tc",
            "Labeling"
          ],
          "TaskTimeLimitInSeconds": 300,
          "TaskTitle": "Classify short bits of text",
          "UiConfig": {
            "UiTemplateS3Uri": "s3://s3bucket-example/TextClassification.template"
          },
          "WorkteamArn": "arn:aws:sagemaker:us-west-2:123456789012:workteam/private-crowd/ExampleTesting"
        },
        "InputConfig": {
          "DataAttributes": {
            "ContentClassifiers": [
              "FreeOfPersonallyIdentifiableInformation",
              "FreeOfAdultContent"
            ]
          },
          "DataSource": {
            "S3DataSource": {
              "ManifestS3Uri": "s3://s3bucket-example/manifest.json"
            }
          }
        },
        "LabelAttributeName": "Categories",
        "LabelCategoryConfigS3Uri": "s3://s3bucket-example/labelcategories.json",
        "LabelingJobName": "example-job-name",
        "OutputConfig": {
          "S3OutputPath": "s3://s3bucket-example/output"
        },
        "RoleArn": "arn:aws:iam::123456789012:role/service-role/AmazonSageMaker-ExecutionRole",
        "StoppingConditions": {
          "MaxHumanLabeledObjectCount": 10000,
          "MaxPercentageOfInputDatasetLabeled": 100
        }
      },
      "Next": "ValidateOutput"
    },
    "ValidateOutput": {
      "Type": "Choice",
      "Choices": [
        {
          "Not": {
            "Variable": "$.LabelingJobArn",
            "StringEquals": ""
          },
          "Next": "Succeed"
        }
      ],
      "Default": "Fail"
    },
    "Succeed": {
      "Type": "Succeed"
    },
    "Fail": {
      "Type": "Fail",
      "Error": "InvalidOutput",
      "Cause": "Output is not what was expected. This could be due to a service outage or a misconfigured service integration."
    }
  }
}
```

## SageMaker Processing Job Example<a name="sagemaker-example-processing"></a>

The following includes a `Task` state that creates an Amazon SageMaker processing job\.

```
{
  "StartAt": "SageMaker CreateProcessingJob Sync",
  "TimeoutSeconds": 3600,
  "States": {
    "SageMaker CreateProcessingJob Sync": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sagemaker:createProcessingJob.sync",
      "Parameters": {
        "AppSpecification": {
          "ImageUri": "737474898029.dkr.ecr.sa-east-1.amazonaws.com/sagemaker-scikit-learn:0.20.0-cpu-py3"
        },
        "ProcessingResources": {
          "ClusterConfig": {
            "InstanceCount": 1,
            "InstanceType": "ml.t3.medium",
            "VolumeSizeInGB": 10
          }
        },
        "RoleArn": "arn:aws:iam::123456789012:role/SM-003-CreateProcessingJobAPIExecutionRole",
        "ProcessingJobName.$": "$.id"
      },
      "Next": "ValidateOutput"
    },
    "ValidateOutput": {
      "Type": "Choice",
      "Choices": [
        {
          "Not": {
            "Variable": "$.ProcessingJobArn",
            "StringEquals": ""
          },
          "Next": "Succeed"
        }
      ],
      "Default": "Fail"
    },
    "Succeed": {
      "Type": "Succeed"
    },
    "Fail": {
      "Type": "Fail",
      "Error": "InvalidConnectorOutput",
      "Cause": "Connector output is not what was expected. This could be due to a service outage or a misconfigured connector."
    }
  }
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.