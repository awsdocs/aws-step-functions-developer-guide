# Start an Athena query<a name="sample-athena-query"></a>

This sample project demonstrates how to use Step Functions and Amazon Athena to start an Athena query and send a notification with query results\. This sample project creates the following:
+ An Amazon Athena query
+ An AWS Glue crawler
+ An Amazon Simple Notification Service topic
+ Related AWS Identity and Access Management \(IAM\) roles

In this project, Step Functions uses Lambda functions and an AWS Glue crawler to generate a set of example data\. It then performs a query using the [Athena service integration](connect-athena.md) and returns the results using an SNS topic\.

For more information about Athena and Step Functions service integrations, see the following:
+ [Using AWS Step Functions with other services](concepts-service-integrations.md)
+ [Call Athena with Step Functions](connect-athena.md)

**Note**  
This sample project may incur charges\.  
For new AWS users, a free usage tier is available\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the Free Tier, see [Athena Pricing](https://aws.amazon.com/athena/pricing/)\.

## Create the State Machine and Provision Resources<a name="sample-athena-query-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Start an Athena query**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Training model workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-athena-query.png)

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + Two Lambda functions
   + A state machine
   + An SNS topic
   + Related IAM roles

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Start a New Execution<a name="sample-athena-query-start-execution"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. On the **State machines** page, choose the **AthenaStateMachine** state machine that was created by the sample project, and then choose **Start execution**\.

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. \(Optional\) Go to the newly created state machine on the Step Functions **Dashboard**, and then choose **New execution**\.

1. When an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**\.

## Example State Machine Code<a name="sample-athena-query-code-examples"></a>

The state machine in this sample project integrates with Athena and AWS Lambda by passing parameters directly to those resources, and uses an SNS topic to return the results of the query\.

Browse through this example state machine to see how Step Functions controls Lambda and Athena\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
  "StartAt": "Generate example log",
  "States": {
    "Generate example log": {
      "Resource": "arn:aws:lambda:us-east-1:111122223333:function:StepFunctionsSample-Athena-LambdaForDataGeneration-AKIAIOSFODNN7EXAMPLE",
      "Type": "Task",
      "Next": "Run Glue crawler"
    },
    "Run Glue crawler": {
      "Resource": "arn:aws:lambda:us-east-1:111122223333:function:StepFunctionsSample-Athen-LambdaForInvokingCrawler-AKIAI44QH8DHBEXAMPLE",
      "Type": "Task",
      "Next": "Start an Athena query"
    },
    "Start an Athena query": {
      "Resource": "arn:aws:states:::athena:startQueryExecution.sync",
      "Parameters": {
        "QueryString": "SELECT * FROM \"athena-sample-project-db-wJalrXUtnFEMI\".\"log\" limit 1",
        "WorkGroup": "stepfunctions-athena-sample-project-workgroup-wJalrXUtnFEMI"
      },
      "Type": "Task",
      "Next": "Get query results"
    },
    "Get query results": {
      "Resource": "arn:aws:states:::athena:getQueryResults",
      "Parameters": {
        "QueryExecutionId.$": "$.QueryExecution.QueryExecutionId"
      },
      "Type": "Task",
      "Next": "Send query results"
    },
    "Send query results": {
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "TopicArn": "arn:aws:sns:us-east-1:111122223333:StepFunctionsSample-AthenaDataQueryd1111-2222-3333-777788889999-SNSTopic-ANPAJ2UCCR6DPCEXAMPLE",
        "Message": {
          "Input.$": "$.ResultSet.Rows"
        }
      },
      "Type": "Task",
      "End": true
    }
  }
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.

## IAM Example<a name="sample-athena-query-iam-example"></a>

These example AWS Identity and Access Management \(IAM\) policies generated by the sample project include the least privilege necessary to execute the state machine and related resources\. We recommend that you include only those permissions that are necessary in your IAM policies\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:us-east-1:111122223333:function:StepFunctionsSample-Athena-LambdaForDataGeneration-AKIAIOSFODNN7EXAMPLE",
                "arn:aws:lambda:us-east-1:111122223333:function:StepFunctionsSample-Athen-LambdaForInvokingCrawler-AKIAI44QH8DHBEXAMPLE"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "sns:Publish"
            ],
            "Resource": [
                "arn:aws:sns:us-east-1:111122223333:StepFunctionsSample-AthenaDataQueryd1111-2222-3333-777788889999-SNSTopic-ANPAJ2UCCR6DPCEXAMPLE"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "athena:getQueryResults",
                "athena:startQueryExecution",
                "athena:stopQueryExecution",
                "athena:getQueryExecution",
                "athena:getDataCatalog"
            ],
            "Resource": [
                "arn:aws:athena:us-east-1:111122223333:workgroup/stepfunctions-athena-sample-project-workgroup-wJalrXUtnFEMI",
                "arn:aws:athena:us-east-1:111122223333:datacatalog/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:ListBucket",
                "s3:ListBucketMultipartUploads",
                "s3:ListMultipartUploadParts",
                "s3:AbortMultipartUpload",
                "s3:CreateBucket",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "glue:CreateDatabase",
                "glue:GetDatabase",
                "glue:GetDatabases",
                "glue:UpdateDatabase",
                "glue:DeleteDatabase",
                "glue:CreateTable",
                "glue:UpdateTable",
                "glue:GetTable",
                "glue:GetTables",
                "glue:DeleteTable",
                "glue:BatchDeleteTable",
                "glue:BatchCreatePartition",
                "glue:CreatePartition",
                "glue:UpdatePartition",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:BatchGetPartition",
                "glue:DeletePartition",
                "glue:BatchDeletePartition"
            ],
            "Resource": [
                "arn:aws:glue:us-east-1:111122223333:database/*",
                "arn:aws:glue:us-east-1:111122223333:table/*",
                "arn:aws:glue:us-east-1:111122223333:catalog"
            ],
            "Effect": "Allow"
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.