# Dynamically Process Data with a Map State<a name="sample-map-state"></a>

This sample project demonstrates dynamic parallelism using a `Map` state\. This sample project creates the following\.
+ Two AWS Lambda functions
+ An Amazon Simple Queue Service queue
+ An Amazon Simple Notification Service topic
+ An Amazon DynamoDB table
+ An AWS Step Functions state machine

In this project, Step Functions uses an AWS Lambda function to pull messages off an Amazon SQS queue, and pass a JSON array of those message to a `Map` state\. For each message in the queue, the state machine writes the message to DynamoDB, invokes the other Lambda function to remove the message from Amazon SQS, and then publishes the message to the Amazon SNS topic\.

For more information on `Map` states and Step Functions service integrations, see the following\.
+ [Map](amazon-states-language-map-state.md)
+ [AWS Service Integrations](concepts-service-integrations.md)

**To create the state machine and provision all resources**

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Iterate steps with a Map state**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Map state workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-map.png)

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + An Amazon Simple Queue Service queue
   + An Amazon Simple Notification Service topic
   + An Amazon DynamoDB table
   + Two AWS Lambda functions
   + An AWS Step Functions state machine

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

Once the resources of the sample project have been deployed, you'll need to add items to the Amazon SQS queue and subscribe to the Amazon SNS topic before you start an execution of the state machine\.

## Subscribe to the Amazon SNS Topic<a name="sample-map-subscribe-topic"></a>

1. Open the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Select **Topics** and choose the topic that was created by the `Map` state sample project\.

   The name will be similar to **MapSampleProj\-SNSTopic\-1CQO4HQ3IR1KN**\.

1. Under **Subscriptions** choose **Create subscription**\.

   The **Create subscription** page is displayed, listing the **Topic ARN** for the topic\.

1. Under **Protocol** select **Email**\.

1. Under **Endpoint** enter an email address to subscribe to the topic\.

1. Select **Create subscription**\.
**Note**  
You must confirm the subscription in your email before it is active\.

1. Open the **Subscription Confirmation** email in the related account and follow open the **Confirm subscription** URL\.

   The **Subscription confirmed\!** page is displayed\.

## Add Messages to the Amazon SQS Queue<a name="sample-map-create-queue"></a>

1. Open the [Amazon SQS console](https://console.aws.amazon.com/sqs/home)\.

1. Select the queue that was created by the `Map` state sample project\.

   The name will be similar to **MapSampleProj\-SQSQueue\-1UDIC9VZDORN7**\.

1. On the **Queue Actions** drop\-down, select **Send a Message**\.

1. On the **Send a Message** window, type a message and select the **Send Message** button\.

1. Select **Send Another Message**\.

   Continue entering messages until you have several in the Amazon SQS queue\.

1. Select **Close**\.

## Start a New Execution<a name="sample-map-start-execution"></a>

**Note**  
Queues in Amazon Simple Notification Service are eventually consistent\. For best results, wait a few minutes between populating your queue and running an execution of your state machine\.

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. On the **State machines** page, choose the **MapStateStateMachine** state machine that was created by the sample project and select **Start execution**\.

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Optionally, you can go to the newly created state machine on the Step Functions **Dashboard**, and then choose **New execution**\.

1. When an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**\.

## Example State Machine Code<a name="sample-map-code-examples"></a>

The state machine in this sample project integrates with Amazon SQS, Amazon SNS, and Lambda by passing parameters directly to those resources\. 

Browse through this example state machine to see how Step Functions controls Lambda, DynamoDB, Amazon SNS by connecting to the Amazon Resource Name \(ARN\) in the `Resource` field, and by passing `Parameters` to the service API\.

For more information about how AWS Step Functions can control other AWS services, see [AWS Service Integrations](concepts-service-integrations.md)\.

```
{
  "Comment": "An example of the Amazon States Language for reading messages from an SQS queue and iteratively processing each message.",
  "StartAt": "Read messages from SQS Queue",
  "States": {
    "Read Messages from SQS Queue": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "OutputPath": "$.Payload",
      "Parameters": {
        "FunctionName": "MapSampleProj-ReadFromSQSQueueLambda-1MY3M63RMJVA9"
      },
      "Next": "Are there messages to process?"
    },
    "Are there messages to process?": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$",
          "StringEquals": "No messages",
          "Next": "Finish"
        }
      ],
      "Default": "Process messages"
    },
    "Process messages": {
      "Type": "Map",
      "Next": "Finish",
      "ItemsPath": "$",
      "Parameters": {
        "MessageNumber.$": "$$.Map.Item.Index",
        "MessageDetails.$": "$$.Map.Item.Value"
      },
      "Iterator": {
        "StartAt": "Write message to DynamoDB",
        "States": {
          "Write message to DynamoDB": {
            "Type": "Task",
            "Resource": "arn:aws:states:::dynamodb:putItem",
            "ResultPath": null,
            "Parameters": {
              "TableName": "MapSampleProj-DDBTable-YJDJ1MKIN6C5",
              "ReturnConsumedCapacity": "TOTAL",
              "Item": {
                "MessageId": {
                  "S.$": "$.MessageDetails.MessageId"
                },
                "Body": {
                  "S.$": "$.MessageDetails.Body"
                }
              }
            },
            "Next": "Remove message from SQS queue"
          },
          "Remove message from SQS queue": {
            "Type": "Task",
            "Resource": "arn:aws:states:::lambda:invoke",
            "InputPath": "$.MessageDetails",
            "ResultPath": null,
            "Parameters": {
              "FunctionName": "MapSampleProj-DeleteFromSQSQueueLambda-198J2839ZO5K2",
              "Payload": {
                "ReceiptHandle.$": "$.ReceiptHandle"
              }
            },
            "Next": "Publish message to SNS topic"
          },
          "Publish message to SNS topic": {
            "Type": "Task",
            "Resource": "arn:aws:states:::sns:publish",
            "InputPath": "$.MessageDetails",
            "Parameters": {
              "Subject": "Message from Step Functions!",
              "Message.$": "$.Body",
              "TopicArn": "arn:aws:sns:us-east-1:012345678910:MapSampleProj-SNSTopic-1CQO4HQ3IR1KN"
            },
            "End": true
          }
        }
      }
    },
    "Finish": {
      "Type": "Succeed"
    }
  }
}
```

## IAM Example<a name="sample-map-iam-example"></a>

This example AWS Identity and Access Management \(IAM\) policy generated by the sample project includes the least privilege necessary to execute the state machine and related resources\. We recommend that you include only those permissions that are necessary in your IAM policies\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:us-east-1:012345678901:function:MapSampleProj-ReadFromSQSQueueLambda-1MY3M63RMJVA9",
                "arn:aws:lambda:us-east-1:012345678901:function:MapSampleProj-DeleteFromSQSQueueLambda-198J2839ZO5K2"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "dynamodb:PutItem"
            ],
            "Resource": [
                "arn:aws:dynamodb:us-east-1:012345678901:table/MapSampleProj-DDBTable-YJDJ1MKIN6C5"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "sns:Publish"
            ],
            "Resource": [
                "arn:aws:sns:us-east-1:012345678901:MapSampleProj-SNSTopic-1CQO4HQ3IR1KN"
            ],
            "Effect": "Allow"
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](service-integration-iam-templates.md)\.