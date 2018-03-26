# Getting Started<a name="getting-started"></a>

This tutorial introduces you to the basics of working with AWS Step Functions\. You'll create a simple, independently running state machine using a `Pass` state\. The `Pass` state represents a *no\-op* \(an instruction with no operation\)\.

**Topics**
+ [Step 1: Creating a State Machine](#create-state-machine-step-1)
+ [Step 2: Starting a New Execution](#create-state-machine-step-2)
+ [Update a State Machine](#update-state-machine-step-3)
+ [Next Steps](#next-steps)

## Step 1: Creating a State Machine<a name="create-state-machine-step-1"></a>

Step Functions offers various predefined state machines as *templates*\. Create your first state machine using the **Hello World** template\.

### To create the state machine<a name="create-state-machine"></a>

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home), and then choose **Get Started**\.

1. On the **Create a state machine** page, select **Templates** and then choose **Hello world**\.  
![\[Hello world.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-hello-world.png)

   Step Functions fills in the name of the state machine automatically\. It also populates the **Code** pane with the Amazon States Language description of the state machine\.

   ```
   {
     "Comment": "A Hello World example of the Amazon States Language using a Pass state",
     "StartAt": "HelloWorld",
     "States": {
       "HelloWorld": {
         "Type": "Pass",
         "Result": "Hello World!",
         "End": true
       }
     }
   }
   ```

   This JSON text defines a `Pass` state named `HelloWorld`\. For more information, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.  
![\[Visual Workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/hello-state-machine-preview.png)

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Create or enter an IAM role\.
   + To create a new IAM role for Step Functions, choose **Create a role for me**, and then choose **I acknowledge that Step Functions will create an IAM role which allows access to my Lambda functions\.**
   + If you have [previously created an IAM role for Step Functions](procedure-create-iam-role.md), choose **I will provide an IAM role ARN** and enter your existing **IAM role ARN**\.
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. Choose **Create State Machine**\.

## Step 2: Starting a New Execution<a name="create-state-machine-step-2"></a>

After you create your state machine, you can start an execution\.

### To start a new execution<a name="start-new-execution"></a>

1. On the ***Helloworld*** page, choose **New execution**\.

   The **New execution** window is displayed\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter your execution id here** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. \(Optional\) In the **Execution Details** section, choose the **Info** tab to view the **Execution Status** and the **Started** and **Closed** timestamps\.

1. To view the results of your execution, choose the **Output** tab\.  
![\[State machine results.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-state-machine-execution-output.png)

## Step 3: \(Optional\) Update a State Machine<a name="update-state-machine-step-3"></a>

You can update your state machine for future executions\.

**Note**  
State machine updates in Step Functions are *eventually consistent*\. All executions within a few seconds will use the updated definition and `roleArn`\. Executions started immediately after updating a state machine may use the previous state machine definition and `roleArn`\.

### To update a state machine<a name="update-state-machine"></a>

1. On the ***Helloworld*** page, choose **Edit state machine**\.

   The **Edit** page is displayed\.

1. In the **Code** pane, edit the Amazon States Language description of the state machine\. Update the `Result` to read `Hello World has been updated!`

   ```
   {
     "Comment": "A Hello World example of the Amazon States Language using a Pass state",
     "StartAt": "HelloWorld",
     "States": {
       "HelloWorld": {
         "Type": "Pass",
         "Result": "Hello World has been updated!",
         "End": true
       }
     }
   }
   ```

1. \(Optional\) Select a new IAM role from the **IAM role for executions** list\.
**Note**  
You can also select **Create new role** to create a new IAM role\. For more information, see [Creating IAM Roles for AWS Step Functions](procedure-create-iam-role.md)\.

1. Choose **Update and start execution**\.

   The **New execution** window displays a message that the state machine was updated successfully\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-state-machine-updated.png)

1. Choose **Start Execution**\.

1. To view the results of your execution, choose the **Output** tab in the **Execution Details** section\.  
![\[Start new execution.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-state-machine-update-output.png)
**Note**  
The output text matches your newly updated state machine\.

## Next Steps<a name="next-steps"></a>

Now that you've created a simple state machine using a `Pass` state, try the following:
+ [Create a Lambda state machine](tutorial-creating-lambda-state-machine.md)
+ [Create a Lambda state machine using AWS CloudFormation](tutorial-lambda-state-machine-cloudformation.md)
+ [Create an activity state machine](tutorial-creating-activity-state-machine.md)
+ [Handle error conditions using a state machine](tutorial-handling-error-conditions.md)
+ [Start a state machine using Amazon CloudWatch Events](tutorial-cloudwatch-events-target.md)
+ [Create a Step Functions API using Amazon API Gateway](tutorial-api-gateway.md)