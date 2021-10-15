# Tutorial: Learn to use the AWS Step Functions Workflow Studio<a name="tutorial-workflow-studio-using"></a>

Learn the basics of working with Workflow Studio for Step Functions\. From the Step Functions console you will navigate to Workflow Studio for Step Functions, where you'll create a state machine with two Pass states\. Use the drag and drop experience visual to search for, select and configure the Pass states\. You'll then view the Amazon States Language JSON code, exit Workflow Studio and run a state machine\.

From the Step Functions console you will start a new execution and review the execution details\. You'll change the result of one of the Pass state and view the changes\. Finally, you perform a clean\-up step, and delete your state machine\. After you complete this tutorial, you'll know how to use Workflow Studio to create and configure a workflow, and how to update, execute and delete your state machine\.

Make sure to complete the [prerequisites for this tutorial](sfn-prerequisites.md)\.

**Topics**
+ [Create a state machine](#tutorial-workflow-studio-using-create)
+ [Start a new execution](#tutorial-workflow-studio-using-start)
+ [Update your state machine](#tutorial-workflow-studio-using-update)
+ [Clean up](#tutorial-workflow-studio-using-cleanup)

## Create a state machine<a name="tutorial-workflow-studio-using-create"></a>

In Workflow Studio, a state machine is a graphical representation of your workflow\. You can use Workflow Studio to define, configure and examine the individual steps of your workflow\.

**To create a state machine**

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

1. Choose **Create state machine**\.

1. On the **Choose authoring method** page, choose **Design your workflow visually**\. 

1. Under **Type**, choose **Standard**\.

   Step Functions has two workflow types: *Standard* and *Express*\. These workflows determine how Step Functions performs tasks, integrates with AWS services, and manages pricing\. After you create a state machine, you can't change its workflow type\.

   For a side\-by\-side comparison of both workflows, under **Type**, choose **Help me decide**\.

1. Choose Next\. This will open Workflow Studio\.  
![\[Workflow Studio components\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_main_01-all.png)

1. Select the **Flow** panel, then drag a `Pass` state to the empty state labelled **Drag first state here**\.

1. Drag the `Choice` state below the `Pass` state\. Change its name to `Hello World example`\.

1. Drag a second `Pass` state to one branch of the `Choice` state, and a `Fail` state to the other branch of the `Choice` state\.

1. Configure the `Choice` rules to use the boolean variable `$.IsHelloWorldExample`\. If `$.IsHelloWorldExample` is `False`, the workflow will enter the Fail state\. Otherwise the workflow will continue its execution flow in the other branch\.

1. Configure the default for the `Choice` state to be `Yes`, and the cause to be `Not Hello World` for the `No` `Fail` state\.

1. Add a `Wait` state after the `Yes` pass state, and name it `Wait 3 sec`\. Configure the wait time to be three seconds\.

1. After the `Wait 3 sec` state, add a `Parallel` state called `Parallel`\. Add two more `Pass` states in its two branches\. Name the first `Pass` state `Hello` and the second `Pass` state `World`\.

1. Finally, drag another `Pass` state and name it `Hello World`\. This points to the end of the workflow\. The completed workflow will look like this:  
![\[Step Functions Workflow Studio completed workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_tutorial_result_01.png)

1. Under **Definition**, review the state machine's workflow\.

1. In the code panel, Studio populates the workflow using the [Amazon States Language](concepts-amazon-states-language.md) and starting with a comment that describes the workflow\. In the code for this workflow, two `Pass` states are defined\. One `Pass` state is named Hello, and the other `Pass` state is named World\. 

   ```
   {
     "Comment": "A Hello World example of the Amazon States Language using Pass states",
     "StartAt": "Pass",
     "States": {
       "Pass": {
         "Type": "Pass",
         "Next": "IsHelloWorldExample",
         "Comment": "A Pass state passes its input to its output, without performing work. Pass states are useful when constructing and debugging state machines."
       },
       "IsHelloWorldExample": {
         "Type": "Choice",
         "Comment": "A Choice state adds branching logic to a state machine. Choice rules can implement 16 different comparison operators, and can be combined using And, Or, and Not\"",
         "Choices": [
           {
             "Variable": "$.IsHelloWorldExample",
             "BooleanEquals": false,
             "Next": "No"
           },
           {
             "Variable": "$.IsHelloWorldExample",
             "BooleanEquals": true,
             "Next": "Yes"
           }
         ],
         "Default": "Yes"
       },
       "No": {
         "Type": "Fail",
         "Cause": "Not Hello World"
       },
       "Yes": {
         "Type": "Pass",
         "Next": "Wait 3 sec"
       },
       "Wait 3 sec": {
         "Type": "Wait",
         "Seconds": 3,
         "Next": "Parallel"
       },
       "Parallel": {
         "Type": "Parallel",
         "End": true,
         "Branches": [
           {
             "StartAt": "Hello",
             "States": {
               "Hello": {
                 "Type": "Pass",
                 "End": true
               }
             }
           },
           {
             "StartAt": "World",
             "States": {
               "World": {
                 "Type": "Pass",
                 "End": true
               }
             }
           }
         ]
       }
     }
   }
   ```

1. Choose **Next**\.

1. \(Optional\) Review your state machine's Amazon States Language definition\. You can make additional changes if you want\.

1. Choose **Save**\. You can see the workflow definition\.

1. Choose **Next**\. Give your workflow the name `HelloWorld`\.

1. Under **Permissions**, select **Create a new IAM role**\.

   When you create a state machine, you select an IAM role that defines which resources the state machine has permission to access during its execution\. Choose from the following options:
   + **Create a new IAM role** – Select this option when you want Step Functions to create a new IAM role for you based on the definition of your state machine and its configuration details\.
   + **Choose an existing role** – Select this option if you previously created an IAM role for Step Functions and your state machine has the correct permissions\.
   + **Enter a role ARN** – Select this option if you know the ARN details for the IAM role that you want to use for Step Functions\.

1. Choose **Create state machine**\.

## Start a new execution<a name="tutorial-workflow-studio-using-start"></a>

State machine executions are instances where you run your workflow to perform tasks\.

**To start a new execution:**

1. On the **Helloworld** page, choose **Start execution**\.

1. **Optional \-** On the **New execution** page, in the **exection ID** field, you can enter a name of your choice\.

   Make sure that the execution name doesn’t contain any non\-ASCII characters\. If you don’t specify your own execution ID, Step Functions generates a unique execution ID for you\.

1. **Optional \- ** In the **Input** panel, you can enter input values for your execution in JSON format\. `IsHelloWorldExample` determines which state machine flow will be executed\. For now, use the following value: 

   ```
   {
      "IsHelloWorldExample": true
   }
   ```

1. On the **New execution** page, choose **Start execution**\.

   After you choose **Start execution**, the Step Functions console directs you to a page that's titled with your execution ID\. On this page, you can review the results of your new execution\. Under **Execution details**, you can see your execution ARN and a status to indicate whether your execution succeeded\. You can also see the timestamps for when your execution started and ended\.

1. To view the results of your execution, choose **Output**\.

   The output is:\.

   ```
   {
      "IsHelloWorldExample": true
   }
   {
      "IsHelloWorldExample": true
   }
   ```

## Update your state machine<a name="tutorial-workflow-studio-using-update"></a>

Change a `Pass` state's result, and update your state machine for future exceptions\. Then view your changes in the visual workflow panel\. An exception is an event that disrupts a step in your workflow\. 

When you update a state machine, your updates are *eventually consistent*\. After a few seconds or minutes, all newly started executions will reflect your state machine's updated definition and `roleARN`\. All currently running executions will run to completion under the previous definition and `roleARN` before updating\.

**To change a `Pass` state's result**

1. On the page titled with your execution ID, choose **Edit state machine**\.

1. On the **Edit Helloworld** page, in the code panel, update the second `Result` to `World has been updated!` 

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

1. On the next page, in the visual workflow panel, examine the individual steps that define your workflow\.

1. **Optional \-** To export the graph of your workflow to an SVG or PNG file, choose **Export**\.

1. To view the results of your execution, in the visual workflow panel, choose **World**, and then, under **Step details**, choose **Output**\.

   The output is `World has been updated!`

## Clean up<a name="tutorial-workflow-studio-using-cleanup"></a>

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

After completing this tutorial, you now know how to use Workflow Studio to create, edit and delete a workflow\.