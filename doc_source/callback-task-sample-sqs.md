# Callback Pattern Example<a name="callback-task-sample-sqs"></a>

This sample project demonstrates how to have AWS Step Functions pause during a task, and wait for an external process to return a task token that was generated when the task started\. 

When this sample project is deployed and an execution is started, the following steps occur\. 

1. Step Functions passes a message that includes a task token to an Amazon SQS queue\.

1. Step Functions then pauses, waiting for that token to be returned\.

1. The Amazon Simple Queue Service queue triggers an AWS Lambda function that calls [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html) with that same task token\.

1. When the task token is received, the workflow continues\.

1. The `"Notify Success"` task publishes an Amazon SNS message that the callback was received\.

To learn how to implement the callback pattern in Step Functions see [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token)\.

**To create the state machine and provision all resources**

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Callback Pattern Example**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Callback example workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-callback-example.png)

1. Choose **Create Sample Project**\.

   The **Create Project Resources** page is displayed, listing the resources that will be created\. For this sample project the resources include:
   + An Amazon SQS message queue\.
   + An AWS Lambda function, that calls the Step Functions API action [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html)\.
   + An Amazon SNS topic to notify success or failure when the workflow can continues\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Create Project Resources** page displays **Creating resources**, you can open the **Stack ID:** link to see which resources are being provisioned\.

   When complete, the **New execution** page is displayed\.

1. \(Optional\) Enter an execution name, and sample input\.  
![\[Start callback execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-callback-execution.png)

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.  
![\[Callback execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-callback-success.png)

1. To review how Step Functions progressed through the workflow and received a callback from Amazon SQS, review the entries in the **Execution event history**\.