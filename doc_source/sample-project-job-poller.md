# Poll for Job Status \(Lambda, AWS Batch\)<a name="sample-project-job-poller"></a>

This sample project creates an AWS Batch job poller\. It implements an AWS Step Functions state machine that uses AWS Lambda to create a `Wait` state loop that checks on an AWS Batch job\. 

This sample project creates and configures all resources so that your Step Functions workflow will submit an AWS Batch job, and will wait for that job to complete before ending successfully\. 

**Note**  
You can also implement this pattern without using a Lambda function\. For information about controlling AWS Batch directly, see [AWS Service Integrations](concepts-service-integrations.md)\. 

This sample project creates the state machine, two Lambda functions, and an AWS Batch queue, and configures the related IAM permissions\. 

**To create the **Job Poller** state machine and provision all resources**

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Job Status Poller**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Job poller visual workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-job-status-poller-preview.png)![\[Job poller visual workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Job poller visual workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)
**Note**  
The **Code** section in this state machine references the AWS resources that will be created for this sample project\.

1. Choose **Create Resources**\.

   The **Create Project Resources** page is displayed, listing the resources that will be created\. For this sample project the resources include:
   + A `SubmitJob` Lambda function
   + A `CheckJob` Lambda function
   + A `SampleJobQueue` Batch Job Queue
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Create Project Resources** page displays **Creating resources**, you can open the **Stack ID** link to see which resources are being provisioned\.

   When complete, the **New execution** page is displayed, with example input similar to this\.

   ```
   {
     "jobName": "my-job",
     "jobDefinition": "arn:aws:batch:us-east-2:123456789012:job-definition/SampleJobDefinition-343f54b445d5312:1",
     "jobQueue": "arn:aws:batch:us-east-2:123456789012:job-queue/SampleJobQueue-4d9d696031e1449",
     "wait_time": 60
   }
   ```

## Starting an Execution<a name="job-status-poller-start-execution"></a>

After you create your state machine, you can start an execution\.

### To start a new execution<a name="create-job-status-poller-state-machine-start-execution"></a>

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Optionally, you can go to the newly created state machine on the Step Functions **Dashboard**, choose **New execution**, and then enter the input code using the name or Amazon Resource Name \(ARN\) of each newly created resource\.

   For example, the input for the previous execution using only the resource names would be as follows\.

   ```
   {
     "jobName": "my-job",
     "jobDefinition": "SampleJobDefinition-343f54b445d5312",
     "jobQueue": "SampleJobQueue-4d9d696031e1449",
     "wait_time": 60
   }
   ```
**Note**  
`wait_time` instructs the `Wait` state to loop every 60 seconds\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. \(Optional\) In the **Execution Details** section, choose **Info** to view the **Execution Status** and the **Started** and **Closed** timestamps\.

1. To view the changing status of your AWS Batch job and the looping results of your execution, choose **Output**\.  
![\[State machine execution.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-job-status-poller-state-machine-execution-output.png)![\[State machine execution.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)