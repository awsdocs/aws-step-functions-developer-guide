# Tutorial: Learn to use the AWS Step Functions Workflow Studio<a name="tutorial-workflow-studio-using"></a>

In this tutorial, you will learn the basics of working with Workflow Studio for AWS Step Functions\. In Workflow Studio, you'll create a state machine containing multiple states, including `Pass`, `Choice`, `Fail`, `Wait`, and `Parallel`\. You'll use the drag and drop feature to search for, select, and configure these states\. Then, you'll view the auto\-generated Amazon States Language JSON code for your workflow, exit Workflow Studio, run the state machine, and review the execution details\.

In this tutorial, you'll also learn how to update the state machine and view the changes in the execution output\. Finally, you'll perform a clean\-up step and delete your state machine\.

After you complete this tutorial, you'll know how to use Workflow Studio to create and configure a workflow\. You'll also know how to update, execute, and delete your state machine\.

**Note**  
Before you start, make sure to complete the [prerequisites for this tutorial](sfn-prerequisites.md)\.

**Topics**
+ [Step 1: Navigate to Workflow Studio](#tutorial-open-wf-studio)
+ [Step 2: Create a state machine](#tutorial-workflow-studio-using-create)
+ [Step 3: Review the auto\-generated Amazon States Language definition](#tutorial-wf-studio-review-asl-def)
+ [Step 4: Start a new execution](#tutorial-workflow-studio-using-start)
+ [Step 5: Update your state machine](#tutorial-workflow-studio-using-update)
+ [Step 6: Clean up](#tutorial-workflow-studio-using-cleanup)

## Step 1: Navigate to Workflow Studio<a name="tutorial-open-wf-studio"></a>

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

1. Choose **Create state machine**\.

1. On the **Choose authoring method** page, choose **Design your workflow visually**\. 

1. For **Type**, keep the default selection of **Standard**\.

   Step Functions has two workflow types: *Standard* and *Express*\. These types determine how Step Functions performs tasks, integrates with AWS services, and manages pricing\. Once you create a state machine, you can't change its workflow type\.

   For a side\-by\-side comparison of both workflows, under **Type**, choose **Help me decide**\.

1. Choose **Next**\. This will open Workflow Studio\.  
![\[Workflow Studio interface. The Actions panel on the left is selected.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_main_01-all.png)

## Step 2: Create a state machine<a name="tutorial-workflow-studio-using-create"></a>

In Workflow Studio, a state machine is a graphical representation of your workflow\. You can use Workflow Studio to define, configure, and examine the individual steps of your workflow\.

**To create a state machine**

1. In Workflow Studio, from the states browser on the left, choose the **Flow** panel\. Then, drag a **`Pass`** state to the empty state labelled **Drag first state here**\.

1. Drag a **`Choice`** state from the **Flow** panel and drop it below the **`Pass`** state\. 

1. For **State name**, replace the default name\. For this tutorial, use the name `IsHelloWorldExample`\.

1. Drag another **`Pass`** state and drop it to one branch of the **`Choice`** state\. Then, drag a **`Fail`** state to the other branch of the **`Choice`** state\.

1. Choose the **`Pass (1)`** state, and rename it to **Yes**\. Rename the **`Fail`** state as **No**\.

1. Specify the **`Choice`** state's branching logic using the boolean variable `IsHelloWorldExample`\.

   If `IsHelloWorldExample` is `False`, the workflow will enter the **`No`** state\. Otherwise, the workflow will continue its execution flow in the **`Yes`** state's branch\.

   To define the branching logic, do the following:

   1. Choose the **`Choice`** state on the canvas, and then under **Choice Rules** choose ![\[edit Choice Rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-learn-using-wf-studio-edit-choice-rule-icon.png)![\[edit Choice Rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[edit Choice Rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Rule \#1** box to define the first choice rule\.

   1. Choose **Add conditions**\.

   1. In the **Conditions for rule \#1** dialog box, enter **$\.IsHelloWorldExample** under **Variable**\.

   1. Choose **is equal to** under **Operator**\.

   1. Choose **Boolean constant** under **Value**, and then choose **true** from the dropdown list\.

   1. Choose **Save conditions**\.

   1. Make sure the **Then next state is:** dropdown list has **Yes** selected\.

   1. Choose **Add new choice rule**, then choose **Add conditions**\. In the **Rule \#2** box, define the second choice rule when the `IsHelloWorldExample` variable's value is false by repeating substeps 6\.c through 6\.e\. For step 6\.e, choose **false** instead of **true**\.

   1. In the **Rule \#2** box, choose `No` from the **Then next state is:** dropdown list\.

   1. In the **Default rule** box, click ![\[edit Choice Rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-learn-using-wf-studio-edit-choice-rule-icon.png)![\[edit Choice Rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[edit Choice Rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/) to define the default choice rule, and then choose **Yes** from the dropdown list\.

1. Add a **`Wait`** state after the **`Yes Pass`** state, and name it **Wait 3 sec**\. Then, configure the wait time to be three seconds by doing the following steps:

   1. Under **Options**, keep the default selection of **Wait for a fixed interval**\.

   1. Under **Seconds**, make sure **Enter seconds** is selected, and then enter **3** in the box\.

1. After the **`Wait 3 sec`** state, add a **`Parallel`** state\. Add two more **`Pass`** states in its two branches\. Name the first **`Pass`** state **Hello**\. Name the second **`Pass`** state **World**\.

   The completed workflow will look like this:  
![\[The completed workflow for the IsHelloWorldExample state machine.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_tutorial_result_01.png)

## Step 3: Review the auto\-generated Amazon States Language definition<a name="tutorial-wf-studio-review-asl-def"></a>

As you drag and drop states from the **Flow** panel onto the canvas, Workflow Studio automatically composes the [Amazon States Language](concepts-amazon-states-language.md) definition of your workflow in real\-time\. You can edit this definition as required\.

1. \(Optional\) Choose **Definition** on the **Inspector** panel and view the state machine's workflow\.

   The following example code shows the auto\-generated Amazon States Language definition for the `IsHelloWorldExample` state machine\. The `Choice` state that you added in Workflow Studio is used to determine the execution flow based on [the branching logic you defined in Step 2](#tutorial-workflow-studio-using-create)\.

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

1. On the **Review generated code** page, review your state machine's Amazon States Language definition\. If needed, you can make additional changes under **Definition**\.

1. Choose **Next**\.

1. Enter a name for your workflow\. For example, enter ***HelloWorld***\.

1. Under **Permissions**, choose **Create a new IAM role**\.

   When you create a state machine, you select an IAM role that defines which resources the state machine has permission to access during its execution\. Choose one of the following options:
   + **Create a new IAM role** – Select this option when you want Step Functions to create a new IAM role for you based on the definition of your state machine and its configuration details\.
   + **Choose an existing role** – Select this option if you previously created an IAM role for Step Functions and your state machine has the correct permissions\.
   + **Enter a role ARN** – Select this option if you know the ARN for the IAM role that you want to use for Step Functions\.

1. Choose **Create state machine**\.

## Step 4: Start a new execution<a name="tutorial-workflow-studio-using-start"></a>

State machine executions are instances where you run your workflow to perform tasks\.

1. On the **HelloWorld** page, choose **Start execution**\.

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In the execution **Input** area, enter input values for your execution in JSON format\. Based on your input, the `IsHelloWorldExample` variable determines which state machine flow will be executed\. For now, use the following input value:

   ```
   {
      "IsHelloWorldExample": true
   }
   ```
**Note**  
While specifying an execution input is optional, in this tutorial, it is mandatory to specify an execution input similar to the above example input\. This input value is referenced in the `Choice` state when you run the state machine\.

1. Choose **Start execution**\.

1. The Step Functions console directs you to a page that's titled with your execution ID\. On this page, you can review the results of your new execution\. Under **Details**, you can see your execution ARN and a status which indicates if your execution succeeded\. You can also see the timestamps for when your execution started and ended\. To view the results of your execution, choose **Execution output**\. For this tutorial, if you entered an input value of `"IsHelloWorldExample": true`, you should see the following output:

   ```
   {
      "IsHelloWorldExample": true
   },
   {
      "IsHelloWorldExample": true
   }
   ```

## Step 5: Update your state machine<a name="tutorial-workflow-studio-using-update"></a>

When you update a state machine, your updates are *eventually consistent*\. After a short amount of time, all newly started executions will reflect your state machine's updated definition and `roleARN`\. All currently running executions will run to completion under the previous definition and `roleARN` before they update\.

In this step, you'll update your state machine by adding a `Result` field in the `Pass` state named `World`\.

1. On the page titled with your execution ID, choose **Edit state machine**\.

1. On the **Edit `HelloWorld`** page, in the **Definition** area, choose **Workflow Studio**\.

1. Choose the `Pass` state named **World** on the canvas, and then choose **Output**\.

1. In the **Result** box, enter **"World has been updated\!"**\.

1. Choose **Apply and exit** to save the change and return to the **Edit `HelloWorld`** page\.

1. \(Optional\) In the **Definition** area, view the updated Amazon States Language definition of your workflow\.

   ```
   {
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
                 "Result": "World has been updated!",
                 "End": true
               }
             }
           }
         ]
       }
   ```

1. Choose **Save**, and then choose **Start execution**\.

1. The Step Functions console displays the following message:

   *The changes to your state machine may affect which resources it needs to access\. To ensure your state machine has the right permissions, you might need to edit the current IAM role, create a new one, or select a different role\.*

   This message is standard\. Choose **Save anyway**\.

1. Choose **Start execution**\. In the **Start execution** dialog box, identify your execution, and then enter the following input value in the **Input** area:

   ```
   {
      "IsHelloWorldExample": true
   }
   ```

1. Choose **Start Execution** again\.

1. \(Optional\) In the visual workflow panel, choose each step to view its details, input, and output under the **Details**, **Step input** and **Step output** tabs, respectively\. For example, in the visual workflow panel, choose the **World** step, and then choose **Step output**\. The output is **World has been updated\!**

1. \(Optional\) To export the graph of your workflow to an SVG or PNG file, choose **Export**\.

## Step 6: Clean up<a name="tutorial-workflow-studio-using-cleanup"></a>

**To delete your state machine**

1. From the navigation menu, choose **State machines**\.

1. On the **State machines** page, select **`HelloWorld`**, and then choose **Delete**\.

1. The Step Functions console prompts you with the following message:

   *You are about to delete your state machine\. Do you want to proceed?*

   This message is standard\. Choose **Delete state machine**\.

   If deletion is successful, a green status bar appears at the top of your screen\. The green status bar tells you that your state machine is marked for deletion\. Your state machine will be removed when all of its executions stop running\.

**To delete your execution role**

1. Open the [Roles page](https://console.aws.amazon.com/iam/home?#/roles) for IAM\.

1. Choose the IAM role that Step Functions created for you: ***StepFunctions\-HelloWorld\-role\-EXAMPLE***\.

1. Choose **Delete role**\.

1. Choose **Yes, delete**\.