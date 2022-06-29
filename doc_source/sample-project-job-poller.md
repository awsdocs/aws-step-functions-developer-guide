# Poll for Job Status \(Lambda, AWS Batch\)<a name="sample-project-job-poller"></a>

This sample project creates an AWS Batch job poller\. It implements an AWS Step Functions state machine that uses AWS Lambda to create a `Wait` state loop that checks on an AWS Batch job\. 

This sample project creates and configures all resources so that your Step Functions workflow will submit an AWS Batch job, and will wait for that job to complete before ending successfully\. 

**Note**  
You can also implement this pattern without using a Lambda function\. For information about controlling AWS Batch directly, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\. 

This sample project creates the state machine, two Lambda functions, and an AWS Batch queue, and configures the related IAM permissions\. 

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

## Create the State Machine and Provision Resources<a name="sample-project-job-poller-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines) and choose **Create state machine**\.

1. Choose **Run a sample project**, and then choose **Job Poller**\.

   The state machine **Definition** and **Visual Workflow** are displayed\.  
![\[Job poller visual workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-job-status-poller-preview.png)![\[Job poller visual workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Job poller visual workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)
**Note**  
The **Definition** section in this state machine references the AWS resources that will be created for this sample project\.

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + A `SubmitJob` Lambda function
   + A `CheckJob` Lambda function
   + A `SampleJobQueue` Batch Job Queue

1. Choose **Deploy resources**\. 
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Starting an Execution<a name="job-status-poller-start-execution"></a>

After all the resources are provisioned and deployed, the **Start execution** dialog box is displayed with example input similar to the following\.

```
{
    "jobName": "my-job",
    "jobDefinition": "arn:aws:batch:us-east-1:123456789012:job-definition/SampleJobDefinition-343f54b445d5312:1",
    "jobQueue": "arn:aws:batch:us-east-1:123456789012:job-queue/SampleJobQueue-4d9d696031e1449",
    "wait_time": 60
  }
```

**Note**  
`wait_time` instructs the `Wait` state to loop every 60 seconds\.

### To start a new execution<a name="create-job-status-poller-state-machine-start-execution"></a>

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

1. \(Optional\) After the execution is complete, choose individual states on the **Graph inspector**, and then choose the **Step input** and **Step output** tabs to view each state's input and output respectively\.

   For example, to view the changing status of your AWS Batch job and the looping results of your execution, choose **Step output**\.  
![\[State machine execution.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-job-status-poller-state-machine-execution-output.png)![\[State machine execution.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

## Example State Machine Code<a name="sample-job-poller-code-example"></a>

The state machine in this sample project integrates with AWS Lambda to submit an AWS Batch job\. Browse through this example state machine to see how Step Functions controls Lambda and AWS Batch\. 

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
    "Comment": "An example of the Amazon States Language that runs an AWS Batch job and monitors the job until it completes.",
    "StartAt": "Submit Job",
    "States": {
      "Submit Job": {
        "Type": "Task",
        "Resource": "arn:aws:lambda:us-east-1:111122223333:function:StepFunctionsSample-JobStatusPol-SubmitJobFunction-jDaYcl4cx55r",
        "ResultPath": "$.guid",
        "Next": "Wait X Seconds"
      },
      "Wait X Seconds": {
        "Type": "Wait",
        "SecondsPath": "$.wait_time",
        "Next": "Get Job Status"
      },
      "Get Job Status": {
        "Type": "Task",
        "Resource": "arn:aws:lambda:us-east-1:111122223333:function:StepFunctionsSample-JobStatusPoll-CheckJobFunction-1JkJwY10vonI",
        "Next": "Job Complete?",
        "InputPath": "$.guid",
        "ResultPath": "$.status"
      },
      "Job Complete?": {
        "Type": "Choice",
        "Choices": [
          {
            "Variable": "$.status",
            "StringEquals": "FAILED",
            "Next": "Job Failed"
          },
          {
            "Variable": "$.status",
            "StringEquals": "SUCCEEDED",
            "Next": "Get Final Job Status"
          }
        ],
        "Default": "Wait X Seconds"
      },
      "Job Failed": {
        "Type": "Fail",
        "Cause": "AWS Batch Job Failed",
        "Error": "DescribeJob returned FAILED"
      },
      "Get Final Job Status": {
        "Type": "Task",
        "Resource": "arn:aws::lambda:us-east-1:111122223333:function:StepFunctionsSample-JobStatusPoll-CheckJobFunction-1JkJwY10vonI",
        "InputPath": "$.guid",
        "End": true
      }
    }
  }
```