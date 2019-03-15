# Periodically Start a State Machine Execution Using CloudWatch Events<a name="tutorial-cloudwatch-events-target"></a>

You can execute a Step Functions state machine in response to an event pattern or on a schedule using Amazon CloudWatch Events\. This tutorial shows you how to set a state machine as a target for a CloudWatch Events rule that starts the execution of a state machine every five minutes\.

For more information about setting a Step Functions state machine as a target using the `PutTarget` Amazon CloudWatch Events API action, see [Add a Step Functions state machine as a target](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_PutTargets.html#API_PutTargets_Example_8)\.

**Topics**
+ [Step 1: Create a State Machine](#tutorial-cloudwatch-events-target-step-1)
+ [Step 2: Create a CloudWatch Events Rule](#tutorial-cloudwatch-events-target-step-2)

## Step 1: Create a State Machine<a name="tutorial-cloudwatch-events-target-step-1"></a>

Before you can set a CloudWatch Events target, you must create a state machine\.
+ To create a basic state machine, use the [Getting Started](getting-started.md) tutorial\.
+ If you already have a state machine, proceed to the next step\.

## Step 2: Create a CloudWatch Events Rule<a name="tutorial-cloudwatch-events-target-step-2"></a>

After you create your state machine, you can create your CloudWatch Events rule\.

### <a name="tutorial-cloudwatch-events-target-create-rule"></a>

1. Navigate to the [CloudWatch Events console](https://console.aws.amazon.com/cloudwatch/), choose **Events**, and then choose **Create Rule**\.

   The **Step 1: Create rule** page is displayed\.

1. In the **Event source** section, choose **Schedule**, and then enter `5` for **Fixed rate of**\.  
![\[Event source options\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-events-target-create-rule-event-source.png)

1. In the **Targets** section, choose **Add target**, and then from the list choose **Step Functions state machine**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-events-target-create-rule-targets.png)

1. CloudWatch Events can create the IAM role needed for your event to run:
   + To create an IAM role automatically, select **Create a new role for this specific resource**\.
   + To use an IAM role that you created previously, choose **Use existing role**\.

1. Choose **Configure details**\.

   The **Step 2: Configure rule details** page is displayed\.

1. Enter a **Name** for your rule \(for example, `statemachine-event`\), choose **Enabled** for **State**, and then choose **Create rule**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-events-target-create-rule-finish.png)

   The rule is created and the **Rules** page is displayed, listing all your CloudWatch Events rules\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-events-target-create-rule-list.png)

   A new execution of your state machine starts every five minutes\.