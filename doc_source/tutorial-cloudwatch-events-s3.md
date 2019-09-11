# Starting a State Machine Execution in Response to Amazon S3 Events<a name="tutorial-cloudwatch-events-s3"></a>

You can use Amazon CloudWatch Events to execute a Step Functions state machine in response to an event or on a schedule\. 

This tutorial shows you how to configure a state machine as a target for a CloudWatch Events rule\. This will start an execution when files are added to an Amazon S3 bucket\.

For a practical application, you could launch a state machine that performs operations on files that you add to the bucket, such as creating thumbnails or running Amazon Rekognition analysis on image and video files\.

For this tutorial you start an execution of a simple `Helloworld`state machine by adding a file to an Amazon S3 bucket\. Then you review example input of that execution to show what information is included in the input from CloudTrail\.

**Topics**
+ [Prerequisite: Create a State Machine](#tutorial-cloudwatch-events-s3-step-1)
+ [Step 1: Create a Bucket in Amazon S3](#tutorial-cloudwatch-events-s3-bucket)
+ [Step 2: Create a Trail in AWS CloudTrail](#tutorial-cloudwatch-events-s3-trail)
+ [Step 3: Create a CloudWatch Events Rule](#tutorial-cloudwatch-events-s3-cwe)
+ [Step 4: Test the CloudWatch Rule](#tutorial-cloudwatch-events-test-rule)
+ [Example of Execution Input](#tutorial-cloudwatch-events-example)

## Prerequisite: Create a State Machine<a name="tutorial-cloudwatch-events-s3-step-1"></a>

Before you can configure a CloudWatch Events target, you must create a state machine\.
+ To create a basic state machine, use the [Getting Started](getting-started.md) tutorial\.
+ If you already have a `Helloworld`state machine, proceed to the next step\.

## Step 1: Create a Bucket in Amazon S3<a name="tutorial-cloudwatch-events-s3-bucket"></a>

Now that you have a `Helloworld` state machine, you need an Amazon S3 bucket\. In Step 3 of this tutorial, you set up a rule so that when a file is added to this bucket, CloudWatch Events triggers an execution of the state machine\.

1. Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3/), and then choose **Create bucket**\.

1. Enter a **Bucket name**, such as `username-sfn-tutorial`\.
**Note**  
Bucket names must be unique across all existing bucket names in all AWS Regions in Amazon S3\. Use your own *username* to make this name unique\. You need to create all resources in the same AWS Region\.

1. Choose **Create**\.

## Step 2: Create a Trail in AWS CloudTrail<a name="tutorial-cloudwatch-events-s3-trail"></a>

After you create an Amazon S3 bucket, create a trail in CloudTrail\.

For API events in Amazon S3 to match your CloudWatch Events rule, you must configure a trail in CloudTrail to receive those events\.

1. Navigate to the [AWS CloudTrail console](https://console.aws.amazon.com/cloudtrail/), choose **View trails**, and then choose **Create trail**\.

1. For **Trail name**, enter `S3Event`\.

1. On the **S3** tab, choose **Add S3 bucket**\.

1. For **Bucket name**, enter the name of the Amazon S3 bucket you created earlier: `username-sfn-tutorial` \([Step 1: Create a Bucket in Amazon S3](#tutorial-cloudwatch-events-s3-bucket)\)\.

1. Under **Storage location**, choose **Yes** next to **Create a new S3 bucket**\.

1. For **S3 bucket**, enter a name for a new bucket to store information about the actions of the Amazon S3 bucket you created earlier\.
**Note**  
This bucket name must be unique across all of Amazon S3\. Include your *username* in the bucket name so that the name will be unique: `username-sfn-tutorial-storage`\.

1. Choose **Create**\.

## Step 3: Create a CloudWatch Events Rule<a name="tutorial-cloudwatch-events-s3-cwe"></a>

After you have a state machine, and have created the Amazon S3 bucket and a trail in AWS CloudTrail, create your Amazon CloudWatch Events rule\.

**Note**  
You must configure CloudWatch Events in the same AWS Region as the Amazon S3 bucket\.

### To create the rule<a name="tutorial-cloudwatch-events-s3-create-rule"></a>

1. Navigate to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/), choose **Events**, and then choose **Create Rule**\.

   The **Step 1: Create rule** page is displayed\.

1. In **Event source**, choose **Event Pattern**\.

1. For **Service Name**, choose **Simple Storage Service \(S3\)**\.

1. For **Event Type**, choose **Object Level Operations**\.

1. Choose **Specific operation\(s\)**, and then choose **PutObject**\.
**Note**  
If the object size is bigger than the Multipart threshold used in the `PutObject` operation, the AWS CloudTrail API logged will be `CompleteMultipartUpload` instead of `PutObject`\. See, [Multipart Upload Overview](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/mpuoverview.html) in the AWS CloudTrail User Guide\.

1. Choose **Specific bucket\(s\) by name** and enter the bucket name you created in Step 1 \(`username-sfn-tutorial`\)\.

The **Event Source** page should look like the following\.

![\[Create rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-events-s3-source.png)

### To create the target<a name="tutorial-cloudwatch-events-s3-create-rule2"></a>

1. In the **Targets** section, choose **Add target**\.

1. From the list, choose **Step Functions state machine**, and in the **State machine** list, choose the state machine from Step 1 \(`Helloworld`\)\.

1. CloudWatch Events can create the IAM role that your event needs to run:
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created before, choose **Use existing role**\.

1. Choose **Configure details**\.

   The **Step 2: Configure rule details** page is displayed\.

1. Enter a **Name** for your rule \(for example, `S3StepFunctions`\), choose **Enabled** for **State**, and then choose **Create rule**\.

   The **Configure rule details** section should look like the following\.  
![\[Configure rule details\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-events-s3-details.png)

   The rule is created and the **Rules** page is displayed, listing all your CloudWatch Events rules\.

## Step 4: Test the CloudWatch Rule<a name="tutorial-cloudwatch-events-test-rule"></a>

Now that everything is in place, test adding a file to the Amazon S3 bucket, and then look at the input of the resulting state machine execution\.

1. Add a file to your Amazon S3 bucket\.

   Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3/), select the bucket you created \(`username-sfn-tutorial`\), and then choose **Upload**\.

1. Add a file \(`test.png` in the following example\), and then choose **Upload**\.

   This launches an execution of your state machine, passing information from AWS CloudTrail as the input\.

1. Check the execution for your state machine\.

   Navigate to the [Step Functions console and select the state machine used in your CloudWatch Events rule \(`Helloworld`\)](https://console.aws.amazon.com/states/)\.

1. Select the most recent execution of that state machine and expand the **Input** section\.

   This input includes information such as the bucket name and the object name\. In a real\-world use case, a state machine can use this input to perform actions on that object\.

## Example of Execution Input<a name="tutorial-cloudwatch-events-example"></a>

The following example shows typical input to the state machine execution\.

```
{
  "version": "0",
  "id": "8d6f9246-b781-44f8-a026-f1c1ab2c61f0",
  "detail-type": "AWS API Call via CloudTrail",
  "source": "aws.s3",
  "account": "123456789012",
  "time": "2018-09-12T00:25:10Z",
  "region": "us-east-2",
  "resources": [],
  "detail": {
    "eventVersion": "1.05",
    "userIdentity": {
      "type": "IAMUser",
      "principalId": "AKIAIOSFODNN7EXAMPLE",
      "arn": "arn:aws:iam::123456789012:user/username",
      "accountId": "123456789012",
      "accessKeyId": "AKIAI44QH8DHBEXAMPLE",
      "userName": "username",
      "sessionContext": {
        "attributes": {
          "creationDate": "2018-09-11T20:10:38Z",
          "mfaAuthenticated": "true"
        }
      },
      "invokedBy": "signin.amazonaws.com"
    },
    "eventTime": "2018-09-12T00:25:10Z",
    "eventSource": "s3.amazonaws.com",
    "eventName": "PutObject",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "203.0.113.34",
    "userAgent": "signin.amazonaws.com",
    "requestParameters": {
      "X-Amz-Date": "20180912T002509Z",
      "bucketName": "username-sfn-tutorial",
      "X-Amz-Algorithm": "AWS4-HMAC-SHA256",
      "x-amz-acl": "private",
      "X-Amz-SignedHeaders": "content-type;host;x-amz-acl;x-amz-storage-class",
      "X-Amz-Expires": "300",
      "key": "test.png",
      "x-amz-storage-class": "STANDARD"
    },
    "responseElements": null,
    "additionalEventData": {
      "x-amz-id-2": "IOWQ4fDEXAMPLEQM+ey7N9WgVhSnQ6JEXAMPLEZb7hSQDASK+Jd1vEXAMPLEa3Km"
    },
    "requestID": "79104EXAMPLEB723",
    "eventID": "cdc4b7ed-e171-4cef-975a-ad829d4123e8",
    "readOnly": false,
    "resources": [
      {
        "type": "AWS::S3::Object",
        "ARN": "arn:aws:s3:::username-sfn-tutorial-2/test.png"
      },
      {
        "accountId": "123456789012",
        "type": "AWS::S3::Bucket",
        "ARN": "arn:aws:s3:::username-sfn-tutorial"
      }
    ],
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
  }
}
```