# Starting a State Machine Execution in Response to Amazon S3 Events<a name="tutorial-cloudwatch-events-s3"></a>

You can use Amazon EventBridge to execute an AWS Step Functions state machine in response to an event or on a schedule\. 

This tutorial shows you how to configure a state machine as a target for an Amazon EventBridge rule\. This rule will start an execution when files are added to an Amazon Simple Storage Service \(Amazon S3\) bucket\.

For a practical application, you could launch a state machine that performs operations on files that you add to the bucket, such as creating thumbnails or running Amazon Rekognition analysis on image and video files\.

For this tutorial you start an execution of a simple `Helloworld` state machine by adding a file to an Amazon S3 bucket\. Then you review example input of that execution to show what information is included in the input from Amazon S3 event notification delivered to EventBridge\.

**Topics**
+ [Prerequisite: Create a State Machine](#tutorial-cloudwatch-events-s3-step-1)
+ [Step 1: Create a Bucket in Amazon S3](#tutorial-cloudwatch-events-s3-bucket)
+ [Step 2: Enable Amazon S3 Event Notification with EventBridge](#tutorial-cloudwatch-events-s3-eb-notification)
+ [Step 3: Create an Amazon EventBridge Rule](#tutorial-cloudwatch-events-s3-cwe)
+ [Step 4: Test the Rule](#tutorial-cloudwatch-events-test-rule)
+ [Example of Execution Input](#tutorial-cloudwatch-events-example)

## Prerequisite: Create a State Machine<a name="tutorial-cloudwatch-events-s3-step-1"></a>

Before you can configure an Amazon EventBridge target, you must create a state machine\.
+ To create a basic state machine, use the [Getting Started](getting-started.md) tutorial\.
+ If you already have a `Helloworld` state machine, proceed to the next step\.

## Step 1: Create a Bucket in Amazon S3<a name="tutorial-cloudwatch-events-s3-bucket"></a>

Now that you have a `Helloworld` state machine, you need to first create an Amazon S3 bucket which stores your files\. In Step 3 of this tutorial, you set up a rule so that when a file is added to this bucket, EventBridge triggers an execution of the state machine\.

1. Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3/), and then choose **Create bucket** to create the bucket in which you want to store your files and trigger an Amazon S3 event rule\.

1. Enter a **Bucket name**, such as `username-sfn-tutorial`\.
**Note**  
Bucket names must be unique across all existing bucket names in all AWS Regions in Amazon S3\. Use your own *username* to make this name unique\. You need to create all resources in the same AWS Region\.

1. Choose **Create bucket**\.

## Step 2: Enable Amazon S3 Event Notification with EventBridge<a name="tutorial-cloudwatch-events-s3-eb-notification"></a>

After you create the Amazon S3 bucket, configure it to send events to EventBridge whenever certain events happen in your S3 bucket, such as file uploads\. In Step 3, you create an EventBridge rule to route events to a target, such as a Step Functions state machine\. 

1. Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.

1. In the **Buckets** list, choose the name of the bucket that you want to enable events for\.

1. Choose **Properties**\.

1. Navigate to the **Event Notifications** section and find the **Amazon EventBridge** subsection\. Then choose **Edit**\.

1. Under **Send notifications to Amazon EventBridge for all events in this bucket**, choose **On**\.
**Note**  
After you enable EventBridge, it takes around five minutes for the changes to take effect\.

## Step 3: Create an Amazon EventBridge Rule<a name="tutorial-cloudwatch-events-s3-cwe"></a>

After you have a state machine, and have created the Amazon S3 bucket and configured it to send event notifications to EventBridge, create an EventBridge rule\.

**Note**  
You must configure EventBridge rule in the same AWS Region as the Amazon S3 bucket\.

### To create the rule<a name="tutorial-cloudwatch-events-s3-create-rule"></a>

1. Navigate to the [Amazon EventBridge console](https://console.aws.amazon.com/events/), choose **Create rule**\.

1. Enter a **Name** for your rule \(for example, `S3StepFunctions`\) and optionally enter a **Description** for the rule\.

1. In **Define pattern**, choose **Event pattern**\.

1. In **Event matching pattern**, choose **Pre\-defined pattern by service**\.

1. In **Service provider**, choose **AWS**\.

1. In **Service name**, choose **Simple Storage Service \(S3\)**\.

1. For **Event type**, choose **Amazon S3 Event Notification**\.

1. Choose **Specific operation\(s\)**, and then choose **Object Created**\.

1. Choose **Specific bucket\(s\) by name** and enter the bucket name you created in Step 1 \(`username-sfn-tutorial`\) to store your files\.

1. Retain the default selections in **Select event bus**\.

### To create the target<a name="tutorial-cloudwatch-events-s3-create-rule2"></a>

1. Choose **Step Functions state machine** in the **Target** dropdown list, and in the **State machine** list, choose the state machine from Step 1 \(`Helloworld`\)\.

1. Amazon EventBridge can create the IAM role that your event needs to run:
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created before, choose **Use existing role**\.

1. Choose **Create**\.

   The rule is created and the **Rules** page is displayed, listing all your Amazon EventBridge rules\.

## Step 4: Test the Rule<a name="tutorial-cloudwatch-events-test-rule"></a>

Now that everything is in place, test adding a file to the Amazon S3 bucket, and then look at the input of the resulting state machine execution\.

1. Add a file to your Amazon S3 bucket\.

   Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3/), select the bucket you created to store files \(`username-sfn-tutorial`\), and then choose **Upload**\.

1. Add a file \(`test.png` in the following example\), and then choose **Upload**\.

   This launches an execution of your state machine, passing information from AWS CloudTrail as the input\.

1. Check the execution for your state machine\.

   Navigate to the [Step Functions console and select the state machine used in your Amazon EventBridge rule \(`Helloworld`\)](https://console.aws.amazon.com/states/)\.

1. Select the most recent execution of that state machine and expand the **Execution Input** section\.

   This input includes information such as the bucket name and the object name\. In a real\-world use case, a state machine can use this input to perform actions on that object\.

## Example of Execution Input<a name="tutorial-cloudwatch-events-example"></a>

The following example shows a typical input to the state machine execution\.

```
{
    "version": "0",
    "id": "6c540ad4-0671-9974-6511-756fbd7771c3",
    "detail-type": "Object Created",
    "source": "aws.s3",
    "account": "123456789012",
    "time": "2022-02-19T01:36:58Z",
    "region": "us-east-1",
    "resources": [
      "arn:aws:s3:::username-sfn-tutorial"
    ],
    "detail": {
      "version": "0",
      "bucket": {
        "name": "username-sfn-tutorial"
      },
      "object": {
        "key": "test.png",
        "size": 800704,
        "etag": "f31d8546bb67845b4d3048cde533b937",
        "sequencer": "00621049BA9A8C712B"
      },
      "request-id": "79104EXAMPLEB723",
      "requester": "123456789012",
      "source-ip-address": "203.0.113.34",
      "reason": "PutObject"
    }
  }
```