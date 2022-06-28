# Gather Amazon S3 bucket info using AWS SDK service integrations<a name="tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations"></a>

This tutorial shows you how to deploy an [AWS SDK integration](supported-services-awssdk.md) project that will create one Standard state machine and one Express state machine, each with the same definition\. These will gather information about your Amazon S3 buckets, then list your buckets along with version information for each bucket in the current region\. 

Deploying the AWS CloudFormation stack included in this tutorial will create all necessary resources, including:
+ An AWS Step Functions Standard state machine
+ An AWS Step Functions Express state machine
+ Related AWS Identity and Access Management roles and permissions

For more information, see [Working with CloudFormation Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) and the `[AWS::StepFunctions::StateMachine](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-stepfunctions-statemachine.html)` resource in the *AWS CloudFormation User Guide*\.

**Topics**
+ [Step 1: Create a Template](#aws-sdk-deploy)
+ [Step 2: Create a Stack](#aws-sdk-create-stack)
+ [Step 3: View your state machine in Workflow Studio](#aws-sdk-wfs)
+ [Step 4: Run a Standard state machine execution](#aws-sdk-run-standard)
+ [Step 5: Run an Express state machine execution](#aws-sdk-run-express)
+ [Template Source Code](#aws-sdk-yaml)

## Step 1: Create an AWS CloudFormation Template<a name="aws-sdk-deploy"></a>

1. Copy the example code from the [AWS CloudFormation Template Source Code](#aws-sdk-yaml) section\.  
![\[\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/aws-sdk-copy.png)![\[\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

1. Paste the source of the AWS CloudFormation template into a file on your local machine\.

   For this example the file is called `aws-sdk.yaml`\.

## Step 2: Create a Stack<a name="aws-sdk-create-stack"></a>

1. Log into the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home)\.

1. Choose **Create Stack**\.

1. Under **Choose a template**, select **Upload a template to Amazon S3** and then **Choose File**\.

1. Browse to the `aws-sdk.yaml` file you created earlier that includes the [template source code](#aws-sdk-yaml)\.

1. Choose **Open** and then **Next**\.

1. Under **Specify Details** enter a **Stack name**\.

1. On the **Options** page, scroll down and choose **Next**\.

1. On the **Review** page, choose:

   **I acknowledge that AWS CloudFormation might create IAM resources\.**

   **I acknowledge that AWS CloudFormation might create IAM resources with custom names\.**

   **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY\_AUTO\_EXPAND**

1. Choose **Create**\. AWS CloudFormation begins to create your stack and displays the **CREATE\_IN\_PROGRESS** status\. When the process is complete, AWS CloudFormation displays the **CREATE\_COMPLETE** status\.

1. \(Optional\) To display the resources in your stack, select the stack and choose the **Resources** tab\.  
![\[Display resources\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations-cf-resources.png)![\[Display resources\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

## Step 3: View your state machine in Workflow Studio<a name="aws-sdk-wfs"></a>

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. Choose one of your new state machines, either **Gather\-S3\-Bucket\-Info\-Standard** or **Gather\-S3\-Bucket\-Info\-Express**\.   
![\[Choose a state machine\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations-cf-choose.png)![\[Choose a state machine\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

1. Choose **Edit**\.

   The definition of your state machine is shown\.

1. Choose **Workflow Studio**\. You can view and edit your state machine in Step Functions Workflow Studio\.  
![\[Display Workflow Studio\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations-cf-wfs.png)![\[Display Workflow Studio\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

## Step 4: Run a Standard state machine execution<a name="aws-sdk-run-standard"></a>

1. If you are not already signed in, sign in to the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. On the **State machines** page, choose **Gather\-S3\-Bucket\-Info\-Standard**\.

1. Choose **Start execution**\.

1. \(Optional\) enter some input for the execution\.

   ```
   {
       "Comment": "Testing the AWS SDK tutorial."
   }
   ```

1. Choose **Start execution**\.

   The **Gather\-S3\-Bucket\-Info\-Standard** execution starts\.

1. After the execution finishes, you can view the results in the **Execution output** tab\.   
![\[Execution waiting for callback\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations-standard.png)![\[Execution waiting for callback\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

## Step 5: Run an Express state machine execution<a name="aws-sdk-run-express"></a>

1. If you are not already signed in, sign in to the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. On the **State machines** page, choose **Gather\-S3\-Bucket\-Info\-Express**\.

1. Choose **Start execution**\.

1. \(Optional\) enter some input for the execution\.

   ```
   {
       "Comment": "Testing the AWS SDK Express tutorial."
   }
   ```

1. Choose **Start execution**\.

   The **Gather\-S3\-Bucket\-Info\-Express** execution starts\.

1. After the execution finishes, you can view the results in the **Execution output** section of the **Details**\.   
![\[Execution results\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations-express.png)![\[Execution results\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

## AWS CloudFormation Template Source Code<a name="aws-sdk-yaml"></a>

Use this AWS CloudFormation template to deploy an example of an AWS SDK integration for AWS Step Functions\.

```
# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0
AWSTemplateFormatVersion: "2010-09-09"
Transform: AWS::Serverless-2016-10-31
Description: "An example of using Aws-SDK service integrations to gather info on S3 buckets"  

Resources:
  ApplicationRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument: 
        Version: "2012-10-17"
        Statement: 
          - 
            Effect: "Allow"
            Principal: 
              Service: 
                - states.amazonaws.com
            Action: 
              - "sts:AssumeRole"
      Policies:
        - PolicyName: AppPolicy
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              -
                Effect: Allow
                Action:
                - xray:PutTraceSegments
                - xray:PutTelemetryRecords
                - xray:GetSamplingRules
                - xray:GetSamplingTargets
                - s3:ListAllMyBuckets
                - s3:ListBucket
                - s3:GetBucketVersioning
                - s3:GetBucketLocation
                Resource: '*'
  
  StateMachineGetS3BucketInfoStandard:
    Type: AWS::Serverless::StateMachine
    Properties:
      Type: "STANDARD"
      Name: "Gather-S3-Bucket-Info-Standard"
      Role: !GetAtt ApplicationRole.Arn
      Definition:
        Comment: An example of using Aws-SDK service integrations to gather info on S3 buckets
        StartAt: ListBuckets
        States:
          ListBuckets:
            Type: Task
            Parameters: {}
            Resource: arn:aws:states:::aws-sdk:s3:listBuckets
            Next: Map
            Retry:
            - ErrorEquals:
              - States.ALL
              BackoffRate: 1
              IntervalSeconds: 1
              MaxAttempts: 3
          Map:
            Type: Map
            Iterator:
              StartAt: GetBucketLocation
              States:
                GetBucketLocation:
                  Type: Task
                  Parameters:
                    Bucket.$: "$.Name"
                  Resource: arn:aws:states:::aws-sdk:s3:getBucketLocation
                  ResultPath: "$.BucketLocationInfo"
                  Next: Is In Current Region?
                  Retry:
                  - ErrorEquals:
                    - States.ALL
                    BackoffRate: 1
                    IntervalSeconds: 1
                    MaxAttempts: 3
                Is In Current Region?:
                  Type: Choice
                  Choices:
                  - And:
                    - Variable: "$.BucketLocationInfo.LocationConstraint "
                      IsPresent: true
                    - Variable: "$.BucketLocationInfo.LocationConstraint "
                      StringEquals: ca-central-1
                    Next: GetBucketVersioning
                  Default: Add Unknown Version Info
                Add Unknown Version Info:
                  Type: Pass
                  End: true
                  ResultPath: "$.BucketVersioningInfo"
                  Result:
                    Status: Unknown
                GetBucketVersioning:
                  Type: Task
                  End: true
                  Parameters:
                    Bucket.$: "$.Name"
                  Resource: arn:aws:states:::aws-sdk:s3:getBucketVersioning
                  ResultPath: "$.BucketVersioningInfo"
                  Retry:
                  - ErrorEquals:
                    - States.ALL
                    BackoffRate: 1
                    IntervalSeconds: 1
                    MaxAttempts: 3
            End: true
            ItemsPath: "$.Buckets"

  StateMachineGetS3BucketInfoExpress:
      Type: AWS::Serverless::StateMachine
      Properties:
        Type: "EXPRESS"
        Name: "Gather-S3-Bucket-Info-Express"
        Role: !GetAtt ApplicationRole.Arn
        Definition:
          Comment: An example of using Aws-SDK service integrations to gather info on S3 buckets
          StartAt: ListBuckets
          States:
            ListBuckets:
              Type: Task
              Parameters: {}
              Resource: arn:aws:states:::aws-sdk:s3:listBuckets
              Next: Map
              Retry:
              - ErrorEquals:
                - States.ALL
                BackoffRate: 1
                IntervalSeconds: 1
                MaxAttempts: 3
            Map:
              Type: Map
              Iterator:
                StartAt: GetBucketLocation
                States:
                  GetBucketLocation:
                    Type: Task
                    Parameters:
                      Bucket.$: "$.Name"
                    Resource: arn:aws:states:::aws-sdk:s3:getBucketLocation
                    ResultPath: "$.BucketLocationInfo"
                    Next: Is In Current Region?
                    Retry:
                    - ErrorEquals:
                      - States.ALL
                      BackoffRate: 1
                      IntervalSeconds: 1
                      MaxAttempts: 3
                  Is In Current Region?:
                    Type: Choice
                    Choices:
                    - And:
                      - Variable: "$.BucketLocationInfo.LocationConstraint "
                        IsPresent: true
                      - Variable: "$.BucketLocationInfo.LocationConstraint "
                        StringEquals: ca-central-1
                      Next: GetBucketVersioning
                    Default: Add Unknown Version Info
                  Add Unknown Version Info:
                    Type: Pass
                    End: true
                    ResultPath: "$.BucketVersioningInfo"
                    Result:
                      Status: Unknown
                  GetBucketVersioning:
                    Type: Task
                    End: true
                    Parameters:
                      Bucket.$: "$.Name"
                    Resource: arn:aws:states:::aws-sdk:s3:getBucketVersioning
                    ResultPath: "$.BucketVersioningInfo"
                    Retry:
                    - ErrorEquals:
                      - States.ALL
                      BackoffRate: 1
                      IntervalSeconds: 1
                      MaxAttempts: 3
              End: true
              ItemsPath: "$.Buckets"
```