# Getting started with AWS Step Functions<a name="getting-started"></a>

In this tutorial, you learn the basics of working with Step Functions\. You sign in to the Step Functions console, where you create a state machine that uses two [`Pass` states](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-states.html)\. You then start a new execution and review the execution details\. You then change one of the `Pass` state’s result and view the changes\. Finally, you perform a clean\-up step, where you delete your state machine\. At the end of this tutorial, you'll know how to create, test, debug, and delete a state machine\. 

Make sure to complete the [prerequisites for this tutorial](https://docs.aws.amazon.com/step-functions/latest/dg/setting-up-prereq.html)\.

**Topics**
+ [Step 1: Create a state machine](#create-state-machine)
+ [Step 2: Start a new execution](#start-new-execution)
+ [Step 3: Update a state machine](#update-state-machine)
+ [Step 4: Clean up](#clean-up)
+ [Next Steps](#next-steps)

## Step 1: Create a state machine<a name="create-state-machine"></a>

A state machine is a graphical representation of your workflow that you can use to examine the individual steps that define it\.

For more information about state machine structure and common states, see the following:
+ [State machine structure](amazon-states-language-state-machine-structure.md)
+ [Common states](amazon-states-language-common-fields.md)

**To create a state machine from a predefined ***Hello world*** template**

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home?#/statemachines/create)\.

1. On the **Define a state machine** page, choose **Start with a template**, and then choose **Hello world**\.

1. Under **Type**, choose **Standard**\.

   Step Functions has two workflow types: *Standard* and *Express*\. These workflows determine how Step Functions performs tasks, integrates with AWS services, and manages pricing\. After you create a state machine, you can't change its workflow type\.

   For a side\-by\-side comparison of both workflows, under **Type**, choose **Help me decide**\.

1. Under **Definition**, review the state machine's workflow\.

   The state machine has two panes: a *code pane* and a *visual workflow pane*\. The code pane is where you define the workflow for your application\. The visual workflow pane is where Step Functions shows a graph of your workflow as a series of steps\.

   In the code pane, Step Functions populates the workflow using the [Amazon States Language](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-amazon-states-language.html) and starting with a comment that describes the state machine\. In the code, two `Pass` states are defined in JSON: one state is named `Hello`, and the other state is named `World`\.

   ```
   {
     "Comment": "A Hello World example of the Amazon States Language using Pass states",
     "StartAt": "Hello",
     "States": {
       "Hello": {
         "Type": "Pass",
         "Result": "Hello",
         "Next": "World"
       },
       "World": {
         "Type": "Pass",
         "Result": "World",
         "End": true
       }
     }
   }
   ```

   In the visual workflow pane, Step Functions renders a graph of the workflow as a series of steps\.  
![\[Visual Workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-visual-pane-render.png)

   If the graph doesn't appear in the visual workflow pane, choose the refresh icon in the top left corner\.

1. Choose **Next**\.

1. Under **Permissions**, select **Create a new IAM role**\.

   When you create a state machine, you select an IAM role that defines which resources the state machine has permission to access during its execution\. Choose from the following options:
   + **Create a new IAM role** – Select this option when you want Step Functions to create a new IAM role for you based on the definition of your state machine and its configuration details\.
   + **Choose an existing role** – Select this option if youpreviously created an IAM role for Step Functions and your state machine has the correct permissions\.
   + **Enter a role ARN** – Select this option if you know the ARN details for the IAM role that you want to use for Step Functions\.

1. Choose **Create state machine**\.

## Step 2: Start a new execution<a name="start-new-execution"></a>

State machine executions are instances where you run your workflow to perform tasks\.

**To start a new execution**

1. On the **Helloworld** page, choose **Start execution**\.

1. **Optional \-** On the **New execution** page, in the **exection ID** field, you can enter a name of your choice\.

   Make sure that the execution name doesn’t contain any non\-ASCII characters\. If you don’t specify your own execution ID, Step Functions generates a unique execution ID for you\.

1. On the **New execution** page, choose **Start execution**\.

   After you choose **Start execution**, the Step Functions console directs you to a page that's titled with your execution ID\. On this page, you can review the results of your new execution\. Under **Execution details**, you can see your execution ARN and a status to indicate whether your execution succeeded\. You can also see the timestamps for when your execution started and ended\.

1. To view the results of your execution, choose **Output**\.

   The output is `World`\.

## Step 3: Update a state machine<a name="update-state-machine"></a>

Change a `Pass` state's result, and update your state machine for future exceptions\. Then view your changes in the visual workflow pane\. An exception is an event that disrupts a step in your workflow\. 

When you update a state machine, your updates are *eventually consistent*\. After a few seconds or minutes, all newly started executions will reflect your state machine's updated definition and `roleARN`\. All currently running executions will run to completion under the previous defintion and `roleARN` before updating\.

**To change a `Pass` state's result**

1. On the page titled with your execution ID, choose **Edit state machine**\.

1. On the **Edit Helloworld** page, in the code pane, update the second `Result` to `World has been updated!` 

   ```
   {
     "Comment": "A Hello World example of the Amazon States Language using Pass states",
     "StartAt": "Hello",
     "States": {
       "Hello": {
         "Type": "Pass",
         "Result": "Hello",
         "Next": "World"
       },
       "World": {
         "Type": "Pass",
         "Result": "World has been updated!",
         "End": true
       }
     }
   }
   ```

1. Choose **Save**, and then choose **Start execution**\.

   After you choose **Save**, the following message appears:

   *"The changes to your state machine may affect which resources it needs to access\. To ensure your state machine has the right permissions, you might need to edit the current IAM role, create a new one, or select a different role\."*

   This message is standard\. Choose **Save anyway**\.

1. On the **New execution** page, choose **Start Execution** again\.

1. On the next page, in the visual workflow pane, examine the individual steps that define your workflow\.

1. **Optional \-** To export the graph of your workflow to an SVG or PNG file, choose **Export**\.

1. To view the results of your execution, in the visual workflow pane, choose **World**, and then, under **Step details**, choose **Output**\.

   The output is `World has been updated!`

## Step 4: Clean up<a name="clean-up"></a>

In some cases, you might need to delete your state machine and execution role\.

If you're done with this tutorial, delete your state machine and the execution role that Step Functions created for you\.

**To delete your state machine**

1. From the navigation menu, choose **State machines**\.

1. On the **State machines** page, under **State machines**, select **Helloworld**, and then choose **Delete**\.

   After you choose **Delete**, the following message appears:

   *“You are about to delete your state machine\. Do you want to proceed?”*

   This message is standard\. Choose **Delete state machine**\.

   A green status bar appears at the top of your screen\. The green status bar tells you that your state machine is marked for deletion\. Your state machine will be removed when all of its executions stop running\.

**To delete your execution role**

1. Open the [Roles page](https://console.aws.amazon.com/iam/home?#/roles) for IAM\.

1. Choose the IAM role that Step Functions created for you: ***StepFunctions\-Helloworld\-role\-EXAMPLE***\.

1. Choose **Delete role**\.

1. Choose **Yes, delete**\.

   Now that you completed this tutorial, you know how to create, test, debug, and delete a state machine\.

## Next Steps<a name="next-steps"></a>

For more tutorials about working with Step Functions, try the following:
+ [Create a Lambda State Machine](tutorial-creating-lambda-state-machine.md)
+ [Create a Lambda State Machine Using AWS CloudFormation](tutorial-lambda-state-machine-cloudformation.md)
+ [Create an Activity State Machine](tutorial-creating-activity-state-machine.md)
+ [Handle Error Conditions Using a State Machine](tutorial-handling-error-conditions.md)
+ [Start a State Machine Using Amazon CloudWatch Events](tutorial-cloudwatch-events-target.md)
+ [Create a Step Functions API Using Amazon API Gateway](tutorial-api-gateway.md)