# Task Timer<a name="task-timer-sample"></a>

This sample project creates an task timer\. It implements an AWS Step Functions state machine that uses Lambda function to implement a Wait state, that then that sends an Amazon Simple Notification Service Service notification\. A Wait state is a state type that waits for a trigger to perform a single unit of work\.

This sample project creates the state machine, a Lambda function, an Amazon SNS topic, and configures the related IAM permissions\. For more information on the resources that are created with the **Task Timer** sample project, see:

+ [AWS CloudFormation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)

+ [Amazon Simple Notification Service Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/)

+ [AWS Lambda Developer Guide](http://docs.aws.amazon.com/lambda/latest/dg/)

+ [IAM Getting Started Guide](http://docs.aws.amazon.com/IAM/latest/GettingStartedGuide/)

To create the **Task Timer** state machine and provision all resources:

1. Log in to the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/), and choose **Create a state machine**\.

1. Select **Sample Projects** and choose **Task Timer**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Task timer visual workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-task-timer-preview.png)![\[Task timer visual workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Task timer visual workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)
**Note**  
The **Code** section in this state machine references the AWS resources that will be created for this sample project\.

1. Choose **Create Sample Project**\.

   The **Create Project Resources** window is displayed, listing the resources that will be created\. For this sample project the resources include:

   + A SendToSNS Lambda function

   + A TaskTimerTopic Amazon SNS topic
**Note**  
It can take up to 10 minutes as these resources and related IAM permissions are created\. While the **Create Project Resources** window displays **Creating resources**, you can open the **Stack ID:** link to see which resources are being provisioned\.

   Once complete, the **New execution** window is displayed, with example input similar to this:

   ```
   {
     "topic": "arn:aws:sns:us-east-2:123456789012:StepFunctionsSample-TaskTimer-517b8680-e0ad-07cf-feee-65aa5fc63ac0-SNSTopic-96RHT77RAKTS",
     "message": "HelloWorld",
     "timer_seconds": 10
   }
   ```

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. \(Optional\) In the **Execution Details** section, choose **Info** to view the **Execution Status** and the **Started** and **Closed** timestamps\.

1. To view the changing status of your AWS Batch job and the looping results of your execution, choose **Output**\.  
![\[Task timer execution output.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-task-timer-state-machine-execution-output.png)![\[Task timer execution output.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Task timer execution output.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)