# Manage Amazon SageMaker Training Jobs with Step Functions<a name="connect-sagemaker"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information, see:
+ [Service Integrations](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

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
When you create a state machine that integrates with Amazon SageMaker, AWS Step Functions will not automatically create a policy for `CreateTransformJob`\. You must attach an inline policy to the created role\. For more information, see this example IAM policy: [`CreateTrainingJob`](sagemaker-iam.md#sagemaker-iam-createtrainingjob)\.

## Amazon SageMaker Transform Job Example<a name="sagemaker-example-transform"></a>

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

## Amazon SageMaker Training Job Example<a name="sagemaker-example-training"></a>

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

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](service-integration-iam-templates.md)\.