# Train a Machine Learning Model<a name="sample-train-model"></a>

This sample project demonstrates how to use SageMaker and AWS Step Functions to train a machine learning model and how to batch transform a test dataset\. This sample project creates the following:
+ An AWS Lambda function
+ An Amazon Simple Storage Service \(Amazon S3\) bucket
+ An AWS Step Functions state machine
+ Related AWS Identity and Access Management \(IAM\) roles

In this project, Step Functions uses a Lambda function to seed an Amazon S3 bucket with a test dataset\. It then trains a machine learning model and performs a batch transform, using the [SageMaker service integration](connect-sagemaker.md)\.

For more information about SageMaker and Step Functions service integrations, see the following:
+ [Using AWS Step Functions with other services](concepts-service-integrations.md)
+ [Manage SageMaker with Step Functions](connect-sagemaker.md)

**Note**  
This sample project may incur charges\.  
For new AWS users, a free usage tier is available\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the Free Tier, see [SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/)\.

## Create the State Machine and Provision Resources<a name="sample-train-model-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Train a machine learning model**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Training model workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-train-model.png)

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + A Lambda function
   + An Amazon S3 bucket
   + A Step Functions state machine
   + Related IAM roles

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Start a New Execution<a name="sample-train-model-start-execution"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. On the **State machines** page, choose the **TrainAndBatchTransformStateMachine** state machine that was created by the sample project, and then choose **Start execution**\.

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. \(Optional\) Go to the newly created state machine on the Step Functions **Dashboard**, and then choose **New execution**\.

1. When an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**\.

## Example State Machine Code<a name="sample-train-model-code-examples"></a>

The state machine in this sample project integrates with SageMaker and AWS Lambda by passing parameters directly to those resources, and uses an Amazon S3 bucket for the training data source and output\.

Browse through this example state machine to see how Step Functions controls Lambda and SageMaker\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
  "StartAt": "Generate dataset",
  "States": {
    "Generate dataset": {
      "Resource": "arn:aws:lambda:us-west-2:123456789012:function:TrainAndBatchTransform-SeedingFunction-17RNSOTG97HPV",
      "Type": "Task",
      "Next": "Train model (XGBoost)"
    },
    "Train model (XGBoost)": {
      "Resource": "arn:aws:states:::sagemaker:createTrainingJob.sync",
      "Parameters": {
        "AlgorithmSpecification": {
          "TrainingImage": "433757028032.dkr.ecr.us-west-2.amazonaws.com/xgboost:latest",
          "TrainingInputMode": "File"
        },
        "OutputDataConfig": {
          "S3OutputPath": "s3://trainandbatchtransform-s3bucket-1jn1le6gadwfz/models"
        },
        "StoppingCondition": {
          "MaxRuntimeInSeconds": 86400
        },
        "ResourceConfig": {
          "InstanceCount": 1,
          "InstanceType": "ml.m4.xlarge",
          "VolumeSizeInGB": 30
        },
        "RoleArn": "arn:aws:iam::123456789012:role/TrainAndBatchTransform-SageMakerAPIExecutionRole-Y9IX3DLF6EUO",
        "InputDataConfig": [
          {
            "DataSource": {
              "S3DataSource": {
                "S3DataDistributionType": "ShardedByS3Key",
                "S3DataType": "S3Prefix",
                "S3Uri": "s3://trainandbatchtransform-s3bucket-1jn1le6gadwfz/csv/train.csv"
              }
            },
            "ChannelName": "train",
            "ContentType": "text/csv"
          }
        ],
        "HyperParameters": {
          "objective": "reg:logistic",
          "eval_metric": "rmse",
          "num_round": "5"
        },
        "TrainingJobName.$": "$$.Execution.Name"
      },
      "Type": "Task",
      "Next": "Save Model"
    },
    "Save Model": {
      "Parameters": {
        "PrimaryContainer": {
          "Image": "433757028032.dkr.ecr.us-west-2.amazonaws.com/xgboost:latest",
          "Environment": {},
          "ModelDataUrl.$": "$.ModelArtifacts.S3ModelArtifacts"
        },
        "ExecutionRoleArn": "arn:aws:iam::123456789012:role/TrainAndBatchTransform-SageMakerAPIExecutionRole-Y9IX3DLF6EUO",
        "ModelName.$": "$.TrainingJobName"
      },
      "Resource": "arn:aws:states:::sagemaker:createModel",
      "Type": "Task",
      "Next": "Batch transform"
    },
    "Batch transform": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sagemaker:createTransformJob.sync",
      "Parameters": {
        "ModelName.$": "$$.Execution.Name",
        "TransformInput": {
          "CompressionType": "None",
          "ContentType": "text/csv",
          "DataSource": {
            "S3DataSource": {
              "S3DataType": "S3Prefix",
              "S3Uri": "s3://trainandbatchtransform-s3bucket-1jn1le6gadwfz/csv/test.csv"
            }
          }
        },
        "TransformOutput": {
          "S3OutputPath": "s3://trainandbatchtransform-s3bucket-1jn1le6gadwfz/output"
        },
        "TransformResources": {
          "InstanceCount": 1,
          "InstanceType": "ml.m4.xlarge"
        },
        "TransformJobName.$": "$$.Execution.Name"
      },
      "End": true
    }
  }
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.

## IAM Example<a name="sample-train-model-iam-example"></a>

These example AWS Identity and Access Management \(IAM\) policies generated by the sample project include the least privilege necessary to execute the state machine and related resources\. We recommend that you include only those permissions that are necessary in your IAM policies\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "cloudwatch:PutMetricData",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:DescribeLogStreams",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

The following policy allows the Lambda function to seed the Amazon S3 bucket with sample data\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::trainandbatchtransform-s3bucket-1jn1le6gadwfz/*",
            "Effect": "Allow"
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.