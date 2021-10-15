# Use Step Functions and AWS Batch with error handling<a name="sample-batch-error-handling"></a>

This sample project demonstrates how to use Step Functions to run a batch job with error\-handling, with various state types, using AWS Batch and Amazon SNS\. Deploying this sample project will create an AWS Step Functions state machine, an AWS Batch job, and an Amazon SNS topic\.

In this project, Step Functions uses a state machine to call the AWS Batch job synchronously\. It then waits for the job to succeed or fail, retries and catches errors when a job fails, then sends an Amazon SNS topic with a message about whether the job succeeded or failed\.

## Create the State Machine and Provision Resources<a name="sample-batch-error-handling-create"></a>

1. Open the [Error\-handling using AWS Batch and Amazon SNS](https://console.aws.amazon.com/states/home#/sampleProjects?batchErrorHandling) sample project\. The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Manage AWS Batch workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-manage-batch.png)

1. Choose **Next**\.

   The **Deploy and run** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + An AWS Batch job
   + An Amazon SNS topic

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Start a New Execution<a name="sample-batch-error-handling-start-execution"></a>

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Optionally, you can go to the newly created state machine on the Step Functions **Dashboard**, and then choose **New execution**\.

1. When an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**\.

## Example State Machine Code<a name="sample-batch-error-handling-code-examples"></a>

The state machine in this sample project integrates with AWS Batch and Amazon SNS by passing parameters directly to those resources\. 

Browse through this example state machine to see how Step Functions controls AWS Batch and Amazon SNS by connecting to the Amazon Resource Name \(ARN\) in the `Resource` field, and by passing `Parameters` to the service API\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
  "Comment": "An example of the Amazon States Language for notification on an AWS Batch job completion",
  "StartAt": "Submit Batch Job",
  "TimeoutSeconds": 3600,
  "States": {
    "Submit Batch Job": {
      "Type": "Task",
      "Resource": "arn:aws:states:::batch:submitJob.sync",
      "Parameters": {
        "JobName": "BatchJobNotification",
        "JobQueue": "arn:aws:batch:us-west-2:123456789012:job-queue/BatchJobQueue-123456789abcdef",
        "JobDefinition": "arn:aws:batch:us-west-2:123456789012:job-definition/BatchJobDefinition-123456789abcdef:1"
      },
      "Next": "Notify Success",
      "Retry": [
        {
          "ErrorEquals": [
            "States.ALL"
          ],
          "IntervalSeconds": 30,
          "MaxAttempts": 2,
          "BackoffRate": 1.5
        }
      ],
      "Catch": [
        {
          "ErrorEquals": [ "States.ALL" ],
          "Next": "Notify Failure"
        }
      ]
    },
    "Notify Success": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "Message": "Batch job submitted through Step Functions succeeded",
        "TopicArn": "arn:aws:sns:us-west-2:123456789012:StepFunctionsSample-BatchJobManagement12345678-9abc-def0-1234-567890abcdef-SNSTopic-A2B3C4D5E6F7G"
      },
      "End": true
    },
    "Notify Failure": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "Message": "Batch job submitted through Step Functions failed",
        "TopicArn": "arn:aws:sns:us-west-2:123456789012:StepFunctionsSample-BatchJobManagement12345678-9abc-def0-1234-567890abcdef-SNSTopic-A2B3C4D5E6F7G"
      },
      "End": true
    }
  }
}
```

## IAM Example<a name="sample-batch-error-handling-iam-example"></a>

This example AWS Identity and Access Management \(IAM\) policy generated by the sample project includes the least privilege necessary to execute the state machine and related resources\. We recommend that you include only those permissions that are necessary in your IAM policies\. 

**Example `BatchJobNotificationAccessPolicy`**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sns:Publish"
            ],
            "Resource": [
                "arn:aws:sns:us-west-2:123456789012:StepFunctionsSample-BatchJobManagement12345678-9abc-def0-1234-567890abcdef-SNSTopic-A2B3C4D5E6F7G"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "batch:SubmitJob",
                "batch:DescribeJobs",
                "batch:TerminateJob"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "events:PutTargets",
                "events:PutRule",
                "events:DescribeRule"
            ],
            "Resource": [
                "arn:aws:events:us-west-2:123456789012:rule/StepFunctionsGetEventsForBatchJobsRule"
            ],
            "Effect": "Allow"
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.