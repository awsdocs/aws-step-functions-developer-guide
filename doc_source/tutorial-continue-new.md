# Continue as a New Execution<a name="tutorial-continue-new"></a>

This tutorial shows you how to create a state machine with a Lambda function that can start a new execution, continuing your ongoing work in that new execution\.

AWS Step Functions is designed to run workflows that have a finite duration and number of steps\. Executions are limited to a duration of one year, and a maximum of 25,000 events \(see [Limits](limits.md)\)\. 

However, you can create a state machine that uses an AWS Lambda function to start a new execution, before allowing the current execution to terminate\. This enables you to have a state machine that can break large jobs into smaller workflows, or to have a state machine that runs indefinitely\.

This tutorial builds on the concept of using an external Lambda function to modify your workflow, which was demonstrated in the [Iterating a Loop Using Lambda](tutorial-create-iterate-pattern-section.md) tutorial\. You use the same Lambda function \(`Iterator`\) to iterate a loop for a specific number of times\. In addition, you create another Lambda function to start a new execution of your workflow, and to decrement a count each time it starts a new execution\. By setting the number of executions in the input, this state machine ends and restarts an execution a specified number of times\.

![\[\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-continue-state-machine.png)

The state machine you'll create implements the following states\.


| State | Purpose | 
| --- | --- | 
| ConfigureCount |  A `[Pass](amazon-states-language-pass-state.md)` state that configures the `count`, `index`, and `step` values that the `Iterator` Lambda function uses to step through iterations of work\.  | 
|  `Iterator`  |  A `[Task](amazon-states-language-task-state.md)` state that references the `Iterator` Lambda function\.  | 
| IsCountReached | A [Choice](amazon-states-language-choice-state.md) state that uses a Boolean value from the Iterator function to decide if the state machine should continue the example work, or move to the ShouldRestart choice state\. | 
| ExampleWork | In this example, ExampleWork is a Pass state that represents the Task state that would perform work in an actual implementation\. | 
| ShouldRestart | A [Choice](amazon-states-language-choice-state.md) state that uses the executionCount value to decide if it should end one execution and start another, or simply end\.  | 
| Restart | A [Task](amazon-states-language-task-state.md) state that uses a Lambda function to start a new execution of your state machine\. Like the Iterator function, this function also decrements a count\. It passes that value to the input of the new execution\.  | 

## Prerequisites<a name="tutorial-continue-new-prereq"></a>

Before you begin, go through the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial to ensure you have created an initial IAM role, and that you are familiar with using Lambda and Step Functions together\.

**Topics**
+ [Prerequisites](#tutorial-continue-new-prereq)
+ [Step 1: Create an Iterate Lambda Function to Iterate a Count](#tutorial-continue-new-step-1)
+ [Step 2: Create a Restart Lambda Function to Start a New Step Functions Execution](#tutorial-continue-new-step-3)
+ [Step 3: Create a State Machine](#tutorial-continue-new-step-4)
+ [Step 4: Update the IAM Policy](#tutorial-continue-new-step-2)
+ [Step 5: Run an Execution](#tutorial-continue-new-step-5)

## Step 1: Create an Iterate Lambda Function to Iterate a Count<a name="tutorial-continue-new-step-1"></a>

**Note**  
If you have completed the [Iterating a Loop Using Lambda](tutorial-create-iterate-pattern-section.md) tutorial, you can skip this step and use that Lambda function\.

This section and the [Iterating a Loop Using Lambda](tutorial-create-iterate-pattern-section.md) tutorial show how you can use a Lambda function to track a count so that you can track the number of iterations of a loop in your state machine\. 

 The following Lambda function receives input values for `count`, `index`, and `step`\. It returns these values with an updated `index` and a Boolean named `continue`\. The Lambda function sets `continue` to `true` if the `index` is less than `count`\.

Your state machine then implements a `Choice` state that executes some application logic if `continue` is `true`, or moves on to `ShouldRestart` if `continue` is `false`\.

### To create the Iterate Lambda function<a name="tutorial-continue-new-create-lambda-function"></a>

1. Open the [Lambda console](https://console.aws.amazon.com/lambda/home), and then choose **Create function**\.

1. In the **Create function** section, choose **Author from scratch**\.

1. In the **Author with code snippets** section, configure your Lambda function, as follows:

   1. For **Name**, enter `Iterator`\.

   1. For **Runtime**, choose **Node\.js 10\.15**\.

   1. For **Role**, select **Choose an existing role**\.

   1. For **Existing role**, choose the Lambda role that you created in the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial\.
**Note**  
If the IAM role that you created doesn't appear in the list, the role might still need a few minutes to propagate to Lambda\.

   1. Choose **Create function**\.

      When your Lambda function is created, make a note of its Amazon Resource Name \(ARN\) in the upper\-right corner of the page, for example:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:Iterator
      ```

1. Copy the following code for the Lambda function into the **Configuration** section of the ***Iterator*** page in the Lambda console\.

   ```
   exports.iterator = function iterator (event, context, callback) {
     let index = event.iterator.index
     let step = event.iterator.step
     let count = event.iterator.count
    
     index += step
    
     callback(null, {
       index,
       step,
       count,
       continue: index < count
     })
   }
   ```

   This code accepts input values for `count`, `index`, and `step`\. It increments the `index` by the value of `step` and returns these values, and the Boolean value of `continue`\. The value of `continue` is `true` if `index` is less than `count`\.

1. Choose **Save**\.

### Test the Iterate Lambda Function<a name="tutorial-continue-new-step-1-test"></a>

To see your `Iterate` function working, run it with numeric values\. You can provide input values for your Lambda function that mimic an iteration to see what output you get with specific input values\. 

#### To test your Lambda function<a name="tutorial-continue-new-test-lambda-function"></a>

1. In the **Configure test event** dialog box, choose **Create new test event**, and then type `TestIterator` for **Event name**\.

1. Replace the example data with the following\.

   ```
   {
     "Comment": "Test my Iterator function",
     "iterator": {
       "count": 10,
       "index": 5,
       "step": 1
     }
   }
   ```

   These values mimic what would come from your state machine during an iteration\. The Lambda function increments the index and returns `continue` as `true`\. When the index is not less than the `count`, it returns `continue` as `false`\. For this test, the index has already incremented to `5`\. The results should increment the `index` to `6` and set `continue` to `true`\.

1. Choose **Create**\.

1. On the ***Iterator*** page in your Lambda console, be sure **TestIterator** is listed, and then choose **Test**\.

   The results of the test are displayed at the top of the page\. Choose **Details** and review the result\.

   ```
   {
     "index": 6,
     "step": 1,
     "count": 10,
     "continue": true
   }
   ```
**Note**  
If you set `index` to `9` for this test, the `index` increments to `10`, and `continue` is `false`\. 

## Step 2: Create a Restart Lambda Function to Start a New Step Functions Execution<a name="tutorial-continue-new-step-3"></a>

1. Open the [Lambda console](https://console.aws.amazon.com/lambda/home), and then choose **Create function**\.

1. In the **Author with code snippets** section, configure your Lambda function, as follows:

   1. For **Name**, enter `Restart`\.

   1. For **Runtime**, choose **Node\.js 10\.15**\.

   1. For **Role**, select **Choose an existing role**\.

   1. Under **Existing role**, choose the role that includes the IAM policy you created previously\.

   1. Choose **Create function**\.

      When your Lambda function is created, make a note of its Amazon Resource Name \(ARN\) in the upper\-right corner of the page, for example:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:Restart
      ```

1. Copy the following code for the Lambda function into the **Configuration** section of the ***Restart*** page in the Lambda console\.

   The following code decrements a count of the number of executions, and starts a new execution of your state machine, including the decremented value\.

   ```
   var aws = require('aws-sdk');
   var sfn = new aws.StepFunctions();
   
   exports.restart = function(event, context, callback) {
   
     let StateMachineArn = event.restart.StateMachineArn;
     event.restart.executionCount -= 1;
     event = JSON.stringify(event);
   
     let params = {
         input: event,
         stateMachineArn: StateMachineArn
     };
   
     sfn.startExecution(params, function(err, data) {
         if (err) callback(err);
         else callback(null,event);
     });
   
   }
   ```

1. Choose **Save**\.

## Step 3: Create a State Machine<a name="tutorial-continue-new-step-4"></a>

Now that you've created your two Lambda functions, create a state machine\. In this state machine, the `ShouldRestart` and `Restart` states are how you break your work across multiple executions\.

**Example ShouldRestart Choice state**  
This excerpt of your state machine shows the `ShouldRestart` `[Choice](amazon-states-language-choice-state.md)` state\. This state determines whether you should restart the execution\.  

```
"ShouldRestart": {
"Type": "Choice",
"Choices": [
  {
    "Variable": "$.restart.executionCount",
    "NumericGreaterThan": 1,
    "Next": "Restart"
  }
],
```

The `$.restart.executionCount` value is included in the input of the initial execution\. It's decremented by one each time the `Restart` function is called, and then placed into the input for each subsequent execution\.

**Example Restart Task state**  
This excerpt of your state machine shows the `Restart` `[Task](amazon-states-language-task-state.md)` state\. This state uses the Lambda function you created earlier to restart the execution, and to decrement the count to track the remaining number of executions to start\.  

```
"Restart": {
  "Type": "Task",
  "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Restart",
  "Next": "Done"
},
```

1. On the Step Functions console, choose **Create a state machine**\.

1. Select **Author with code snippets**, and enter `ContinueAsNew` as your state machine name\.

1. Paste the following into the `Code` pane\.  
**Example ContinueAsNew state machine**  

   ```
   {
       "Comment": "Continue-as-new State Machine Example",
       "StartAt": "ConfigureCount",
       "States": {
           "ConfigureCount": {
               "Type": "Pass",
               "Result": {
                   "count": 100,
                   "index": -1,
                   "step": 1
               },
               "ResultPath": "$.iterator",
               "Next": "Iterator"
           },
           "Iterator": {
               "Type": "Task",
               "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Iterator",
               "ResultPath": "$.iterator",
               "Next": "IsCountReached"
           },
           "IsCountReached": {
               "Type": "Choice",
               "Choices": [
                   {
                       "Variable": "$.iterator.continue",
                       "BooleanEquals": true,
                       "Next": "ExampleWork"
                   }
               ],
               "Default": "ShouldRestart"
           },
           "ExampleWork": {
               "Comment": "Your application logic, to run a specific number of times",
               "Type": "Pass",
               "Result": {
                 "success": true
               },
               "ResultPath": "$.result",
               "Next": "Iterator"
           },
           "ShouldRestart": {
             "Type": "Choice",
             "Choices": [
               {
                 "Variable": "$.restart.executionCount",
                 "NumericGreaterThan": 0,
                 "Next": "Restart"
               }
             ],
             "Default": "Done"
           },
           "Restart": {
             "Type": "Task",
             "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Restart",
             "Next": "Done"
           },
           "Done": {
               "Type": "Pass",
               "End": true 
           }
       }
   }
   ```

1. Update the `Resource` string in the `Restart` and `Iterator` states to reference the respective Lambda functions you created earlier\.

1. Choose **Next**\.

1. Create or enter an IAM role:
   + To create an IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the list, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. Choose **Create state machine**\.

**Note**  
Save the Amazon Resource Name \(ARN\) of this state machine\. 

## Step 4: Update the IAM Policy<a name="tutorial-continue-new-step-2"></a>

To ensure your Lambda function has permissions to start a new Step Functions execution, attach an inline policy to the IAM role you use for your `Restart` Lambda function\. For more information, see [Embedding Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#embed-inline-policy-console) in the *IAM User Guide*\.

```
{
 "Version": "2012-10-17",
 "Statement": [
     {
         "Sid": "VisualEditor0",
         "Effect": "Allow",
         "Action": [
             "states:StartExecution"
         ],
         "Resource": "*"
     }
 ]
}
```

**Note**  
You can update the `"Resource": "*"` line in the previous example to reference the ARN of your `ContinueAsNew` state machine\. This restricts the policy so that it can only start an execution of that specific state machine\.

## Step 5: Run an Execution<a name="tutorial-continue-new-step-5"></a>

To start an execution, provide input that includes the ARN of the state machine and an `executionCount` for how many times it should start a new execution\.

1.  On the **ContinueAsNew** page, choose **New execution**\.

1. In the **Input** section, on the **New execution** page, enter `Test1` for the execution name\. Then enter the following in the **Input**\.

   ```
   {
     "restart": {
       "StateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:ContinueAsNew",
       "executionCount": 4
     }
   }
   ```

1. Update the `StateMachineArn` field with the ARN for your `ContinueAsNew` state machine\.

1. Choose **Start Execution**\.

The **Visual Workflow** graph displays the first of the four executions\. Before it completes, it will pass through the `Restart` state and start a new execution\.

![\[First execution of four.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/execution-test1.png)

With this execution complete, you can look at the next execution that's running\. Select the **ContinueAsNew** link at the top to see the list of executions\. You should see both the recently closed execution, and an ongoing execution that the `Restart` Lambda function kicked off\.

![\[One execution complete, the next one running.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/execution-test1-running.png)

When all the executions are complete, you should see four successful executions in the list\. The first execution that was started displays the name you chose, and subsequent executions have a generated name\.

![\[All executions complete.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/execution-test1-complete.png)