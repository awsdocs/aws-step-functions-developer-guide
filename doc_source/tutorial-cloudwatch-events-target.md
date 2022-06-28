# Periodically Start a State Machine Execution Using EventBridge<a name="tutorial-cloudwatch-events-target"></a>

You can execute an AWS Step Functions state machine in response to an event pattern or on a schedule using Amazon EventBridge\. This tutorial shows you how to set a state machine as a target for an EventBridge rule that starts the periodic execution of a state machine based on the schedule you define\.

**Topics**
+ [Step 1: Create a State Machine](#tutorial-cloudwatch-events-target-step-1)
+ [Step 2: Create an EventBridge Rule](#tutorial-cloudwatch-events-target-step-2)
+ [Example of Execution Input](#sfn-sm-periodic-exec-example)

## Step 1: Create a State Machine<a name="tutorial-cloudwatch-events-target-step-1"></a>

Before you can set an EventBridge target, you must create a state machine\.
+ To create a basic state machine, use the [Getting Started](getting-started.md) tutorial\.
+ If you already have a state machine, proceed to the next step\.

## Step 2: Create an EventBridge Rule<a name="tutorial-cloudwatch-events-target-step-2"></a>

After you create your state machine, you can create your EventBridge rule\.

### <a name="tutorial-cloudwatch-events-target-create-rule"></a>

1. Navigate to the [Amazon EventBridge console](https://console.aws.amazon.com/events/), and choose **Create rule**\.

   Alternatively, on the [Amazon EventBridge console](https://console.aws.amazon.com/events/), choose **Rules**, and then choose **Create rules**\.

1. On the **Step 1 Define rule detail** page, enter a name and description for the rule\.
**Note**  
A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to match events that come from your account, choose **default**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\.

1. For **Rule type**, choose **Schedule**\.

1. Choose **Next**\.

1. On the **Step 2 Define schedule** page, for **Schedule pattern**, do one of the following:
   + To use a cron expression to define the schedule, choose **A fine\-grained schedule that runs at a specific time, such as 8:00 a\.m\. PST on the first Monday of every month\.** and enter the cron expression\.
   + To use a rate expression to define the schedule, choose **A schedule that runs at a regular rate, such as every 10 minutes\.** and enter the rate expression\.

1. Choose **Next**\.

1. On the **Step 3 Select target\(s\)** page, for **Target types**, choose **AWS service**\.

1. For **Select a target**, choose **Step Functions state machine**\.

1. For **State machine**, choose the state machine that you want to run periodically\.

1. For **Execution role**, do one of the following:
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created earlier, choose **Use existing role** and choose the existing role from the drop\-down list\.

1. Choose **Next**\.

1. \(Optional\) On the **Step 4 Configure tags** page, enter one or more tags for the rule\.

1. Choose **Next**\.

1. On the **Step 5 Review and create** page, review the details of the rule and choose **Create rule**\.

## Example of Execution Input<a name="sfn-sm-periodic-exec-example"></a>

The following example shows a typical input to the state machine execution\.

```
{
  "version": "0",
  "id": "6c540ad4-0671-9974-6511-756fb789fced",
  "detail-type": "Scheduled Event",
  "source": "aws.events",
  "account": "123456789012",
  "time": "2022-03-26T00:01:02Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:sm:::rule/my-state-machine"
  ],
  "detail": {}
}
```