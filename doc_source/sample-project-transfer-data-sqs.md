# Transfer Data Records \(Lambda, DynamoDB, SQS\)<a name="sample-project-transfer-data-sqs"></a>

This sample project demonstrates how to read values from a DynamoDB table and send them to Amazon SQS using AWS Step Functions\. Deploying this sample project will create a Step Functions state machine, a DynamoDB table, a Lambda function, and an Amazon SQS topic\. In this project, Step Functions uses the Lambda function to populate the DynamoDB table and then uses a for loop to read each of the entries, and sends each entry to Amazon SQS\.

To create the **Transfer Data Records** state machine and provision all resources:

1. Log in to the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/), and choose **Create a state machine**\.

1. Select **Sample Projects** and choose **Transfer Data Records**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Transfer data with SQS workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-transfer-sqs.png)
**Note**  
The **Code** section in this state machine references the AWS resources that will be created for this sample project\.

1. Select **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project the resources include:
   + A Lambda function for seeding the DynamoDB table
   + An Amazon SQS queue
   + A DynamoDB table

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes as these resources and related IAM permissions are created\. While the **Deploy resources** page displays, you can open the **Stack ID** link to see which resources are being provisioned\.

## To start a new execution<a name="sample-sqs-start-execution"></a>

1. On the **New execution** page, enter an execution name \(optional\) and choose **Start Execution\.**

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Optionally, you can go to the newly\-created state machine on the Step Functions **Dashboard**, select **New execution**\.

1. Once an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**

## Example State machine code<a name="sample-sqs-code-examples"></a>

The state machine in this sample project integrates with DynamoDB and Amazon SQS by passing parameters directly to those resources\. Browse through this example state machine to see how Step Functions controls DynamoDB and Amazon SQS by connecting to the ARN in the `Resource` field, and by passing `Parameters` to the service API\.

For more information on how AWS Step Functions can control other AWS services, see: [AWS Service Integrations](concepts-connectors.md)\.

```
{
  "Comment" : "An example of the Amazon States Language for reading messages from a DynamoDB table and sending them to SQS",
  "StartAt": "Seed the DynamoDB Table",
  "TimeoutSeconds": 3600,
  "States": {
    "Seed the DynamoDB Table": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:sqsconnector-SeedingFunction-T3U43VYDU5OQ",
      "ResultPath": "$.List",
      "Next": "For Loop Condition"
    },
    "For Loop Condition": {
      "Type": "Choice",
      "Choices": [
        {
          "Not": {
            "Variable": "$.List[0]",
            "StringEquals": "DONE"
          },
          "Next": "Read Next Message from DynamoDB"
        }
      ],
      "Default": "Succeed"
    },
    "Read Next Message from DynamoDB": {
      "Type": "Task",
      "Resource": "arn:aws:states:::dynamodb:getItem",
      "Parameters": {
        "TableName": "sqsconnector-DDBTable-1CAFOJWP8QD6I",
        "Key": {
          "MessageId": {"S.$": "$.List[0]"}
        }
      },
      "ResultPath": "$.DynamoDB",
      "Next": "Send Message to SQS"
    },
    "Send Message to SQS": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sqs:sendMessage",
      "Parameters": {
        "MessageBody.$": "$.DynamoDB.Item.Message.S",
        "QueueUrl": "https://sqs.us-east-1.amazonaws.com/123456789012/sqsconnector-SQSQueue-QVGQBW134PWK"
      },
      "ResultPath": "$.SQS",
      "Next": "Pop Element from List"
    },
    "Pop Element from List": {
      "Type": "Pass",
      "Parameters": {
        "List.$": "$.List[1:]"
      },
      "Next": "For Loop Condition"
    },
    "Succeed": {
      "Type": "Succeed"
    }
  }
}
```

For more information on passing parameters and managing results, see:
+ [Pass Parameters to a Service API](connectors-parameters.md)
+ [`ResultPath`](input-output-resultpath.md)

## IAM Example<a name="sample-sqs-iam-example"></a>

This example IAM policy generated by the sample project includes the least privilege necessary to execute the state machine and related resources\. It is a best practice to include only those permissions necessary in your IAM policies 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "dynamodb:GetItem"
            ],
            "Resource": [
                "arn:aws:dynamodb:ap-northeast-1:123456789012:table/TransferDataRecords-DDBTable-3I41R5L5EAGT"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "sqs:SendMessage"
            ],
            "Resource": [
                "arn:aws:sqs:ap-northeast-1:123456789012:TransferDataRecords-SQSQueue-BKWXTS09LIW1"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "lambda:invokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:ap-northeast-1:123456789012:function:TransferDataRecords-SeedingFunction-VN4KY2TPAZSR"
            ],
            "Effect": "Allow"
        }
    ]
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](connectors-iam-templates.md)\.