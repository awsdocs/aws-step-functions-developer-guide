# Lambda orchestration example<a name="sample-lambda-orchestration"></a>

This sample project demonstrates how to integrate AWS Lambda functions in Step Functions state machines\. This sample project creates the following:
+ Five Lambda functions
+ An Amazon Simple Queue Service queue
+ An Amazon Simple Notification Service topic
+ Related AWS Identity and Access Management \(IAM\) roles

In this project, Step Functions uses Lambda functions to check a stock price and determine a buy or sell trading recommendation\. The user is then provided this recommendation and can choose whether to buy or sell the stock\. The result of the trade is returned using an SNS topic\.

For more information about Step Functions service integrations, see the following:
+ [Using AWS Step Functions with other services](concepts-service-integrations.md)
+ IAM policies for:
  + [AWS Lambda](lambda-iam.md)
  + [Amazon Simple Queue Service](sqs-iam.md)
  + [Amazon Simple Notification Service](sns-iam.md)

**Note**  
This sample project may incur charges\.  
For new AWS users, a free usage tier is available\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the free tier, see [Pricing](http://aws.amazon.com/step-functions/pricing)\.

## Create the State Machine and Provision Resources<a name="sample-lambda-orchestration-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Lambda orchestration example**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Training model workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-lambda-orchestration.png)

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + Five Lambda functions
   + A state machine
   + An Amazon SQS queue
   + An Amazon SNS topic
   + Related IAM roles

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Start a New Execution<a name="sample-lambda-orchestration-start-execution"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. On the **State machines** page, choose the **StockTradingStateMachine** state machine that was created by the sample project, and then choose **Start execution**\.

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. \(Optional\) Go to the newly created state machine on the Step Functions **Dashboard**, and then choose **New execution**\.

1. When an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**\.

## Example State Machine Code<a name="sample-lambda-orchestration-code-examples"></a>

The state machine in this sample project integrates with AWS Lambda by passing parameters directly to those resources, uses an Amazon SQS queue to manage the request for human approval, and uses an Amazon SNS topic to return the results of the query\.

Browse through this example state machine to see how Step Functions controls Lambda and Amazon SQS\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
    "StartAt": "Check Stock Price",
    "States": {
        "Check Stock Price": {
            "Type": "Task",
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-HelloLam-CheckStockPriceLambda-444455556666",
            "Next": "Generate Buy/Sell recommendation"
        },
        "Generate Buy/Sell recommendation": {
            "Type": "Task",
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-Hello-GenerateBuySellRecommend-123456789012",
            "ResultPath": "$.recommended_type",
            "Next": "Request Human Approval"
        },
        "Request Human Approval": {
            "Type": "Task",
            "Resource": "arn:aws:states:::sqs:sendMessage.waitForTaskToken",
            "Parameters": {
                "QueueUrl": "https://sqs.us-west-1.amazonaws.com/111122223333/StepFunctionsSample-HelloLambda4444-5555-6666-RequestHumanApprovalSqs-777788889999",
                "MessageBody": {
                    "Input.$": "$",
                    "TaskToken.$": "$$.Task.Token"
                }
            },
            "ResultPath": null,
            "Next": "Buy or Sell?"
        },
        "Buy or Sell?": {
            "Type": "Choice",
            "Choices": [
                {
                    "Variable": "$.recommended_type",
                    "StringEquals": "buy",
                    "Next": "Buy Stock"
                },
                {
                    "Variable": "$.recommended_type",
                    "StringEquals": "sell",
                    "Next": "Sell Stock"
                }
            ]
        },
        "Buy Stock": {
            "Type": "Task",
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-HelloLambda-BuyStockLambda-000000000000",
            "Next": "Report Result"
        },
        "Sell Stock": {
            "Type": "Task",
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-HelloLambda-SellStockLambda-111111111111",
            "Next": "Report Result"
        },
        "Report Result": {
            "Type": "Task",
            "Resource": "arn:aws:states:::sns:publish",
            "Parameters": {
                "TopicArn": "arn:aws:sns:us-west-1:111122223333:StepFunctionsSample-HelloLambda1111-2222-3333-ReportResultSnsTopic-222222222222",
                "Message": {
                    "Input.$": "$"
                }
            },
            "End": true
        }
    }
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.

## IAM Example<a name="sample-lambda-orchestration-iam-example"></a>

These example AWS Identity and Access Management \(IAM\) policies generated by the sample project include the least privilege necessary to execute the state machine and related resources\. We recommend that you include only those permissions that are necessary in your IAM policies\. 

```
{
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-HelloLam-CheckStockPriceLambda-444455556666",
            "Effect": "Allow"
        }
    ]
}
```

```
{
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-Hello-GenerateBuySellRecommend-123456789012",
            "Effect": "Allow"
        }
    ]
}
```

```
{
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-HelloLambda-BuyStockLambda-777788889999",
            "Effect": "Allow"
        }
    ]
}
```

```
{
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": "arn:aws:lambda:us-west-1:111122223333:function:StepFunctionsSample-HelloLambda-SellStockLambda-000000000000",
            "Effect": "Allow"
        }
    ]
}
```

```
{
    "Statement": [
        {
            "Action": [
                "sqs:SendMessage*"
            ],
            "Resource": "arn:aws:sqs:us-west-1:111122223333:StepFunctionsSample-HelloLambda4444-5555-6666-RequestHumanApprovalSqs-111111111111",
            "Effect": "Allow"
        }
    ]
}
```

```
{
    "Statement": [
        {
            "Action": [
                "sns:Publish"
            ],
            "Resource": "arn:aws:sns:us-west-1:111122223333:StepFunctionsSample-HelloLambda1111-2222-3333-ReportResultSnsTopic-222222222222",
            "Effect": "Allow"
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.