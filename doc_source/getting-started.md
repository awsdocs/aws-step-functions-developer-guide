# Getting Started<a name="getting-started"></a>

This tutorial introduces you to the basics of working with AWS Step Functions\. To get started, you create a simple, independently running state machine using a `Pass` state\. The `Pass` state represents a *no\-op* \(an instruction with no operation\)\.

**Topics**
+ [Create a state machine](#create-state-machine)
+ [Start a new execution](#start-new-execution)
+ [Update a state machine](#update-state-machine)
+ [Next Steps](#next-steps)

Step Functions offers various predefined state machines as *templates*\. Create your first state machine using the **Hello World** template\.

## Create a state machine<a name="create-state-machine"></a>

1. Sign in to the AWS Management Console and open the Step Functions console at [https://console\.aws\.amazon\.com/states/home?\#/statemachines/create](https://console.aws.amazon.com/states/home?#/statemachines/create)\.

1. On the **Define state machine** page select **Templates** and choose **Hello world**\. Review the **State machine definition** and the visual workflow\.  
![\[Hello world.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-hello-world.png)

   Step Functions fills in the name of the state machine automatically\. It also populates the **Code** pane with the Amazon States Language description of the state machine\.

   This JSON text defines a `Pass` state named `HelloWorld`\. For more information, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

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

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.  
![\[Visual Workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/hello-state-machine-preview.png)

1. Choose **Next**\.

1. Create or enter an IAM role:
   + To create an IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the list, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. Select **Create state machine**\.

## Start a new execution<a name="start-new-execution"></a>

After you create your state machine, you can start an execution\.

1. On the ***Helloworld*** page, choose **New execution** or, if you have started an execution before, choose **Start execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. On the **New execution** page, choose **Start execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. \(Optional\) In the **Execution Details** section, choose the **Info** tab to view the **Execution Status** and the **Started** and **Closed** timestamps\.

1. To view the results of your execution, choose the **Output** tab\.  
![\[State machine results.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-output.png)

## Update a state machine<a name="update-state-machine"></a>

You can update your state machine for future executions\.

**Note**  
State machine updates in Step Functions are *eventually consistent*\. All executions within a few seconds will use the updated definition and `roleArn`\. Executions started immediately after updating a state machine can use the previous state machine definition and `roleArn`\.

1. On the ***Helloworld*** page, choose **Edit**\.

1. In the **Code** pane on the **Edit** page, edit the Amazon States Language description of the state machine\. Update the `Result` to read `Hello World has been updated!`

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

1. \(Optional\) Select a new AWS Identity and Access Management \(IAM\) role from the **IAM role for executions** list\.
**Note**  
You can also choose **Create new role** to create an IAM role\. For more information, see [Creating IAM Roles for AWS Step Functions](procedure-create-iam-role.md)\.

1. Choose **Save**, and then choose **Start execution**\.

1. On the **New execution** page, choose **Start Execution**\.

1. To view the results of your execution, select the **HelloWorld** state in the **Visual workflow**, and expand the **Output** section under **Step details**\.  
![\[Execution output\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-state-machine-update-output.png)
**Note**  
The output text matches your newly updated state machine\.

## Next Steps<a name="next-steps"></a>

Now that you've created a simple state machine using a `Pass` state, try the following:
+ [Create a Lambda State Machine](tutorial-creating-lambda-state-machine.md)
+ [Create a Lambda State Machine Using AWS CloudFormation](tutorial-lambda-state-machine-cloudformation.md)
+ [Create an Activity State Machine](tutorial-creating-activity-state-machine.md)
+ [Handle Error Conditions Using a State Machine](tutorial-handling-error-conditions.md)
+ [Start a State Machine Using Amazon CloudWatch Events](tutorial-cloudwatch-events-target.md)
+ [Create a Step Functions API Using Amazon API Gateway](tutorial-api-gateway.md)