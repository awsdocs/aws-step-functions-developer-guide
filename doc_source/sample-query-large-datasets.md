# Query large datasets \(Amazon Athena, Amazon S3, AWS Glue, Amazon SNS\)<a name="sample-query-large-datasets"></a>

This sample project demonstrates how to ingest a large data set in Amazon S3 and partition it through AWS Glue Crawlers, then execute Amazon Athena queries against that partition\. Deploying this sample project creates an AWS Step Functions state machine, an Amazon S3 Bucket, an AWS Glue crawler, and an Amazon SNS topic\.

In this project, Step Functions uses a state machine to invoke an AWS Glue crawler that partitions a large dataset in Amazon S3\. Once the AWS Glue crawler returns a success message, the workflow executes Athena queries against that partition\. Once query execution is successfully complete, an Amazon SNS notification is sent to an Amazon SNS topic\.

## Create the State Machine and Provision Resources<a name="sample-query-large-datasets-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects for Step Functions**, and then choose **Query large datasets**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Query large datasets workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/athena_proj_query_large_dataset.png)

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + An AWS Glue crawler
   + An Amazon S3 Bucket
   + Amazon Athena queries
   + An Amazon SNS topic

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Start a New Execution<a name="sample-query-large-datasets-start-execution"></a>

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. \(Optional\) You can go to the newly created state machine on the Step Functions **Dashboard**, and then choose **New execution**\.

1. When an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**\.

## Example State Machine Code<a name="sample-query-large-datasets-code-examples"></a>

The state machine in this sample project integrates with Amazon S3, AWS Glue, Amazon Athena and Amazon SNS by passing parameters directly to those resources\.

Browse through this example state machine to see how Step Functions controls Amazon S3, AWS Glue, Amazon Athena and Amazon SNS by connecting to the Amazon Resource Name \(ARN\) in the `Resource` field, and by passing `Parameters` to the service API\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
    "Comment": "An example demonstrates how to ingest a large data set in Amazon S3 and partition it through aws Glue Crawlers, then execute Amazon Athena queries against that partition.",
    "StartAt": "Start Crawler",
    "States": {
      "Start Crawler": {
        "Type": "Task",
        "Next": "Get Crawler status",
        "Parameters": {
          "Name": "<GLUE_CRAWLER_NAME>"
        },
        "Resource": "arn:aws:states:::aws-sdk:glue:startCrawler"
      },
      "Get Crawler status": {
        "Type": "Task",
        "Parameters": {
          "Name": "<GLUE_CRAWLER_NAME>"
        },
        "Resource": "arn:aws:states:::aws-sdk:glue:getCrawler",
        "Next": "Check Crawler status"
      },
      "Check Crawler status": {
        "Type": "Choice",
        "Choices": [
          {
            "Variable": "$.Crawler.State",
            "StringEquals": "RUNNING",
            "Next": "Wait"
          }
        ],
        "Default": "Start an Athena query"
      },
      "Wait": {
        "Type": "Wait",
        "Seconds": 30,
        "Next": "Get Crawler status"
      },
      "Start an Athena query": {
        "Resource": "arn:aws:states:::athena:startQueryExecution.sync",
        "Parameters": {
            "QueryString": "<ATHENA_QUERYSTRING>",
            "WorkGroup": "<ATHENA_WORKGROUP>"
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
          "TopicArn": "<SNS_TOPIC_ARN>",
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

## IAM Examples<a name="sample-query-large-datasets-iam-example"></a>

These example AWS Identity and Access Management \(IAM\) policies generated by the sample project include the least privilege necessary to execute the state machine and related resources\. We recommend that you include only those permissions that are necessary in your IAM policies\.

**AthenaGetQueryResults**  


```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "athena:getQueryResults"
            ],
            "Resource": [
                "arn:aws:athena:us-east-1:946551482283:workgroup/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ]
        }
    ]
}
```

**AthenaStartQueryExecution**  


```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "athena:startQueryExecution",
                "athena:stopQueryExecution",
                "athena:getQueryExecution",
                "athena:getDataCatalog"
            ],
            "Resource": [
                "arn:aws:athena:us-east-1:946551482283:workgroup/stepfunctions-athena-sample-project-workgroup-8v7bshiv70",
                "arn:aws:athena:us-east-1:946551482283:datacatalog/*"
            ]
        },
        {
            "Effect": "Allow",
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
            "Resource": [
                "arn:aws:s3:::*"
            ]
        },
        {
            "Effect": "Allow",
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
                "arn:aws:glue:us-east-1:946551482283:catalog",
                "arn:aws:glue:us-east-1:946551482283:database/*",
                "arn:aws:glue:us-east-1:946551482283:table/*",
                "arn:aws:glue:us-east-1:946551482283:userDefinedFunction/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "lakeformation:GetDataAccess"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

**SNSPublish**  


```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sns:Publish"
            ],
            "Resource": [
                "arn:aws:sns:us-east-1:946551482283:StepFunctionsSample-AthenaIngestLargeDataset92bc4949-abf8-4a1e-9236-5b7c81b3efa3-SNSTopic-8Y5ZLI5AASXV"
            ]
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.