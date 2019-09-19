# Iterating a Loop Using Lambda<a name="tutorial-create-iterate-pattern-section"></a>

In this tutorial, you implement a design pattern that uses a state machine and an AWS Lambda function to iterate a loop a specific number of times\. 

Use this design pattern any time you need to keep track of the number of loops in a state machine\. This implementation can help you break up large tasks or long\-running executions into smaller chunks, or to end an execution after a specific number of events\. You can use a similar implementation to periodically end and restart a long\-running execution to avoid exceeding service limits for AWS Step Functions, AWS Lambda, or other AWS services\.

Before you begin, go through the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial to ensure you have created the necessary IAM role, and are familiar with using Lambda and Step Functions together\.

**Topics**
+ [Step 1: Create a Lambda Function to Iterate a Count](#create-iterate-pattern-step-1)
+ [Step 2: Test the Lambda Function](#create-iterate-pattern-step-2)
+ [Step 3: Create a State Machine](#create-iterate-pattern-step-3)
+ [Step 4: Start a New Execution](#create-iterate-pattern-step-4)

## Step 1: Create a Lambda Function to Iterate a Count<a name="create-iterate-pattern-step-1"></a>

By using a Lambda function you can track the number of iterations of a loop in your state machine\. The following Lambda function receives input values for `count`, `index`, and `step`\. It returns these values with an updated `index` and a Boolean value named `continue`\. The Lambda function sets `continue` to `true` if the `index` is less than `count`\.

Your state machine then implements a `Choice` state that executes some application logic if `continue` is `true`, or exits if it is `false`\.

### To create the Lambda function<a name="create-iterate-pattern-create-lambda-function"></a>

1. Sign in to the [Lambda console](https://console.aws.amazon.com/lambda/home), and then choose **Create function**\.

1. In the **Create function** section, choose **Author from scratch**\.

1. In the **Basic information** section, configure your Lambda function, as follows:

   1. For **Function name**, enter `Iterator`\.

   1. For **Runtime**, choose **Node\.js 6\.10**\.

   1. For **Role**, select **Use an existing role**\.

   1. For **Existing role**, choose the Lambda role that you created in the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial\.
**Note**  
If the IAM role that you created doesn't appear in the list, the role might still need a few minutes to propagate to Lambda\.

   1. Choose **Create function**\.

      When your Lambda function is created, make a note of its Amazon Resource Name \(ARN\) in the upper\-right corner of the page\. For example:

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

   This code accepts input values for `count`, `index`, and `step`\. It increments the `index` by the value of `step` and returns these values, and the Boolean `continue`\. The value of `continue` is `true` if `index` is less than `count`\.

1. Choose **Save**\.

## Step 2: Test the Lambda Function<a name="create-iterate-pattern-step-2"></a>

Run your Lambda function with numeric values to see it in operation\. You can provide input values for your Lambda function that mimic an iteration, to see what output you get with specific input values\. 

### To test your Lambda function<a name="create-iterate-pattern-test-lambda-function"></a>

1. In the **Configure test event** dialog box, choose **Create new test event**, and then enter `TestIterator` for **Event name**\.

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

   These values mimic what would come from your state machine during an iteration\. The Lambda function will increment the index and return `continue` as `true`\. When the index isn't less than the `count`, it returns `continue` as `false`\. For this test, the index has already incremented to `5`\. The results should increment the `index` to `6` and set `continue` to `true`\.

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

## Step 3: Create a State Machine<a name="create-iterate-pattern-step-3"></a>

### To create the state machine<a name="create-iterate-pattern-create-state-machine"></a>

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home), and then choose **Create a state machine**\.
**Important**  
Ensure that your state machine is under the same AWS account and Region as the Lambda function you created earlier\.

1. On the **Create a state machine** page, choose **Author with code snippets**\. For **Give a name to your state machine**, enter `IterateCount`\.
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

   Select **Next**\.

1. Create or enter an IAM role:
   + To create an IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the list, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. The following code describes a state machine with the following states\.
   + `ConfigureCount` – Sets the default values for `count`, `index`, and `step`\. 

     ```
     "ConfigureCount": {
         "Type": "Pass",
         "Result": {
             "count": 10,
             "index": 0,
             "step": 1
         },
         "ResultPath": "$.iterator",
         "Next": "Iterator"
     },
     ```
   + `Iterator` – References the Lambda function you created earlier, passing in the values configured in `ConfigureCount`\.

     ```
     "Iterator": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Iterate",
         "ResultPath": "$.iterator",
         "Next": "IsCountReached"
     },
     ```
   + `IsCountReached` – A choice state that either runs your sample work again or goes to `Done`, based on a Boolean value returned from your `Iterator` Lambda function\.

     ```
     "IsCountReached": {
         "Type": "Choice",
         "Choices": [
             {
                 "Variable": "$.iterator.continue",
                 "BooleanEquals": true,
                 "Next": "ExampleWork"
             }
         ],
         "Default": "Done"
     },
     ```
   + `ExampleWork` – A stub for the work you want to accomplish in your execution\. In this example, it's a `pass` state\. In an actual implementation, this would be a `task` state\. See [Task](amazon-states-language-task-state.md)\.
   + `Done` – The end state of your execution\.

   In the **Code** pane, add the following state machine definition using the Amazon Resource Name \(ARN\) of [the Lambda function that you created earlier](#create-iterate-pattern-create-lambda-function)\.

   ```
   {
       "Comment": "Iterator State Machine Example",
       "StartAt": "ConfigureCount",
       "States": {
           
           "ConfigureCount": {
               "Type": "Pass",
               "Result": {
                   "count": 10,
                   "index": 0,
                   "step": 1
               },
               "ResultPath": "$.iterator",
               "Next": "Iterator"
           },
           "Iterator": {
               "Type": "Task",
               "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Iterate",
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
               "Default": "Done"
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
           "Done": {
               "Type": "Pass",
               "End": true
             
           }
       }
   }
   ```

   Be sure to update the ARN in the `Iterator` state above, so that it references the Lambda function you created earlier\. For more information about the Amazon States Language, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.

   This graph shows the logic expressed in the previous state machine code\.   
![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-workflow.png)

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Choose **Next**\.

1. Create or enter an IAM role:
   + To create an IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the list, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. Choose **Create state machine**\.

## Step 4: Start a New Execution<a name="create-iterate-pattern-step-4"></a>

After you create your state machine, you can start an execution\.

### To start a new execution<a name="create-iterate-pattern-start-execution"></a>

1. On the **IterateCount** page, choose **New execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, showing your running execution\.  
![\[State machine execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-running.png)

   The execution increments in steps, tracking the count using your Lambda function\. On each iteration, it performs the example work referenced in the `ExampleWork` state in your state machine\. 

1. \(Optional\) In the **Execution Details** section, choose the **Info** tab to view the **Execution Status** and the **Started** and **Closed** timestamps\.

1. When the count reaches the number specified in the `ConfigureCount` state in your state machine, the execution quits iterating and ends\.  
![\[State machine execution complete\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-done.png)