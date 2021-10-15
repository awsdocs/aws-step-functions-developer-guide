# Process High\-Volume Messages from Amazon SQS \(Express Workflows\)<a name="sample-project-express-high-volume-sqs"></a>

This sample project demonstrates how to use an AWS Step Functions Express Workflow to process messages or data from a high\-volume event source, such as Amazon Simple Queue Service \(Amazon SQS\)\. Because Express Workflows can be started at a very high rate, they are ideal for high\-volume event processing or streaming data workloads\.

Here are two commonly used methods to execute your state machine from an event source:
+ **Configure an Amazon CloudWatch Events rule to start a state machine execution whenever the event source emits an event\.** For more information, see [Creating a CloudWatch Events Rule That Triggers on an Event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html)\. 
+ **Map the event source to a Lambda function, and write function code to execute your state machine\.** The AWS Lambda function is invoked each time your event source emits an event, in turn starting a state machine execution\. For more information see [Using AWS Lambda with Amazon SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)\. 

This sample project uses the second method to start an execution each time the Amazon SQS queue sends a message\. You can use a similar configuration to trigger Express Workflows execution from other event sources, such as Amazon Simple Storage Service \(Amazon S3\), Amazon DynamoDB, and Amazon Kinesis\.

For more information about Express Workflows and Step Functions service integrations, see the following:
+ [Standard vs\. Express Workflows](concepts-standard-vs-express.md)
+ [Using AWS Step Functions with other services](concepts-service-integrations.md)
+ [Quotas](limits-overview.md)

## Create the State Machine and Provision Resources<a name="sample-project-high-volume-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?#/statemachines) and choose **Create a state machine**\.

1. Choose **Run a sample project**, and then choose **Process high\-volume messages from Amazon SQS**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Express high volume SQS workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-express-high-volume.png)

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + A Step Functions state machine
   + An Amazon SQS queue
   + A Lambda function

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Trigger Execution<a name="trigger-execution"></a>

1. Open the [Amazon SQS console](https://console.aws.amazon.com/sqs)\.

1. Select the queue that was created by the sample project\.

   The name will be similar to **Example\-SQSQueue\-wJalrXUtnFEMI**\.

1. In the **Queue Actions** list, select **Send a Message**\.

1. Use the copy button to copy the following message, and on the **Send a Message** window, enter it, and choose **Send Message**\.
**Note**  
In this sample message, the `input:` line has been formatted with line breaks to fit the page\. Use the copy button or otherwise ensure that it is entered as a single line with no breaks\. 

   ```
   {
         "input": "QW5kIGxpa2UgdGhlIGJhc2VsZXNzIGZhYnJpYyBvZiB0aGlzIHZpc2lvbiwgVGhlIGNsb3VkLWNhcHBlZCB0b3dlcnMsIHRoZSBnb3JnZW
                   91cyBwYWxhY2VzLCBUaGUgc29sZW1uIHRlbXBsZXMsIHRoZSBncmVhdCBnbG9iZSBpdHNlbGbigJQgWWVhLCBhbGwgd2hpY2ggaXQgaW5o
                   ZXJpdOKAlHNoYWxsIGRpc3NvbHZlLCBBbmQgbGlrZSB0aGlzIGluc3Vic3RhbnRpYWwgcGFnZWFudCBmYWRlZCwgTGVhdmUgbm90IGEgcm
                   FjayBiZWhpbmQuIFdlIGFyZSBzdWNoIHN0dWZmIEFzIGRyZWFtcyBhcmUgbWFkZSBvbiwgYW5kIG91ciBsaXR0bGUgbGlmZSBJcyByb3Vu
                   ZGVkIHdpdGggYSBzbGVlcC4gU2lyLCBJIGFtIHZleGVkLiBCZWFyIHdpdGggbXkgd2Vha25lc3MuIE15IG9sZCBicmFpbiBpcyB0cm91Ym
                   xlZC4gQmUgbm90IGRpc3R1cmJlZCB3aXRoIG15IGluZmlybWl0eS4gSWYgeW91IGJlIHBsZWFzZWQsIHJldGlyZSBpbnRvIG15IGNlbGwg
                   QW5kIHRoZXJlIHJlcG9zZS4gQSB0dXJuIG9yIHR3byBJ4oCZbGwgd2FsayBUbyBzdGlsbCBteSBiZWF0aW5nIG1pbmQu"
   }
   ```

1. Choose **Close**\.

1. Open the [Step Functions console]()\.

1.  Go to your [Amazon CloudWatch Logs log group](https://console.aws.amazon.com/cloudwatch/home?#logs:) and inspect the logs\. The name of the log group will look like **example\-ExpressLogGroup\-wJalrXUtnFEMI**\. 

## Example Lambda Function Code<a name="sample-high-volume-lambda"></a>

The following is Lambda function code that shows how the initiating Lambda function starts a state machine execution using the AWS SDK\. 

```
import boto3

def lambda_handler(event, context):
    message_body = event['Records'][0]['body']
    client = boto3.client('stepfunctions')
    response = client.start_execution(
        stateMachineArn='${ExpressStateMachineArn}',
        input=message_body
    )
```

## Example State Machine Code<a name="sample-high-volume-code-examples"></a>

The Express Workflow in this sample project consists of a set of Lambda functions for text processing\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
  "Comment": "An example of using Express workflows to run text processing for each message sent from an SQS queue.",
  "StartAt": "Decode base64 string",
  "States": {
    "Decode base64 string": {
      "Type": "Task",
      "Resource": "arn:<PARTITION>:states:::lambda:invoke",
      "OutputPath": "$.Payload",
      "Parameters": {
        "FunctionName": "<BASE64_DECODER_LAMBDA_FUNCTION_NAME>",
        "Payload.$": "$"
      },
      "Next": "Generate statistics"
    },
    "Generate statistics": {
      "Type": "Task",
      "Resource": "arn:<PARTITION>:states:::lambda:invoke",
      "OutputPath": "$.Payload",
      "Parameters": {
        "FunctionName": "<TEXT_STATS_GENERATING_LAMBDA_FUNCTION_NAME>",
        "Payload.$": "$"
      },
      "Next": "Remove special characters"
    },
    "Remove special characters": {
      "Type": "Task",
      "Resource": "arn:<PARTITION>:states:::lambda:invoke",
      "OutputPath": "$.Payload",
      "Parameters": {
        "FunctionName": "<STRING_CLEANING_LAMBDA_FUNCTION_NAME>",
        "Payload.$": "$"
      },
      "Next": "Tokenize and count"
    },
    "Tokenize and count": {
      "Type": "Task",
      "Resource": "arn:<PARTITION>:states:::lambda:invoke",
      "OutputPath": "$.Payload",
      "Parameters": {
        "FunctionName": "<TOKENIZING_AND_WORD_COUNTING_LAMBDA_FUNCTION_NAME>",
        "Payload.$": "$"
      },
      "End": true
    }
  }
}
```

## IAM Example<a name="sample-high-volume-iam-example"></a>

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
                "arn:aws:lambda:us-east-1:123456789012:function:example-Base64DecodeLambda-wJalrXUtnFEMI",
                "arn:aws:lambda:us-east-1:123456789012:function:example-StringCleanerLambda-je7MtGbClwBF",
                "arn:aws:lambda:us-east-1:123456789012:function:example-TokenizerCounterLambda-wJalrXUtnFEMI",
                "arn:aws:lambda:us-east-1:123456789012:function:example-GenerateStatsLambda-je7MtGbClwBF"
            ],
            "Effect": "Allow"
        }
    ]
}
```

 The folowing policy ensures that there are sufficient permissions for CloudWatch Logs\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "logs:CreateLogDelivery",
                "logs:GetLogDelivery",
                "logs:UpdateLogDelivery",
                "logs:DeleteLogDelivery",
                "logs:ListLogDeliveries",
                "logs:PutResourcePolicy",
                "logs:DescribeResourcePolicies",
                "logs:DescribeLogGroups"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow"
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.