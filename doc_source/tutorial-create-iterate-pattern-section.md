# Iterating a Loop Using Lambda<a name="tutorial-create-iterate-pattern-section"></a>

In this tutorial, you implement a design pattern that uses a state machine and an AWS Lambda function to iterate a loop a specific number of times\. 

This implementation helps you break up large tasks, or long\-running executions, into smaller chunks\. You can use a similar implementation to periodically end and restart a long\-running execution\. This can help you avoid exceeding service limits for AWS Step Functions, AWS Lambda, or other AWS services\.

Before you begin, go through the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial to familiarize yourself with using Lambda with Step Functions\.

## Step 1: Create a Lambda Function to Iterate a Count<a name="create-iterate-pattern-step-1"></a>

Your Lambda function receives input values for `count`, `index`, and `step`\. Then it returns these values with an updated `index` and a Boolean named `continue`\. The Lambda function sets `continue` to `true` if the `index` is less than `count`\.

Your state machine then implements a `Choice` state that executes some application logic if `continue` is `true`, or exits if it is `false`\.

### To create the Lambda function<a name="create-iterate-pattern-create-lambda-function"></a>

**Important**  
Ensure that your Lambda function is under the same AWS account as your state machine\.

1. Sign in to the [Lambda console](https://console.aws.amazon.com/lambda/home), and then choose **Create function**\.

1. In the **Create function** section, choose **Author from scratch**\.

1. In the **Author from scratch** section, configure your Lambda function, as follows:

   1. For **Name**, type `Iterate`\.

   1. For **Runtime**, select **Node\.js 6\.10**\.

   1. For **Role**, select **Choose an existing role**\.

   1. For **Existing role**, select the Lambda role that you created in the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial\.
**Note**  
If the IAM role that you created doesn't appear in the list, the role might still need a few minutes to propagate to Lambda\.

   1. Choose **Create function**\.

      When your Lambda function is created, make a note of its Amazon Resource Name \(ARN\) in the upper\-right corner of the page\. For example:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:Iterator
      ```

1. Copy the following code for the Lambda function into the **Configuration** section of the ***Iterator*** page\.

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

## Step 2: Test the Lambda Function<a name="create-iterate-pattern-step-3"></a>

Test your Lambda function to see it in operation\.

### To test your Lambda function<a name="create-iterate-pattern-test-lambda-function"></a>

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

   These values will come from your state machine on each iteration until the index is not less than the count\. For this test, the index has already incremented to 5\. The results should increment the `index` to 6 and set `continue` to `true`\.

1. Choose **Create**\.

1. On the ***Iterator*** page, be sure **TestIterator** is listed, and then choose **Test**\.

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
If you set `index` to 9 for this test, the `index` will increment to 10, and `continue` will be `false`\. 

## Step 3: Create a State Machine<a name="create-iterate-pattern-step-4"></a>

### To create the state machine<a name="create-iterate-pattern-create-state-machine"></a>

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home), and then choose **Create a state machine**\.

1. On the **Create a state machine** page, choose **Author from scratch**\. For **Give a name to your state machine**, enter `IterateCount`\.
**Note**  
State machine names must be 1—80 characters in length, must be unique for your account and region, and must not contain any of the following:  
Whitespace
Whitespace characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Create or enter an IAM role\.

   + To create a new IAM role for Step Functions, choose **Create a role for me**, and then choose **I acknowledge that Step Functions will create an IAM role which allows access to my Lambda functions\.**

   + If you have [previously created an IAM role for Step Functions](procedure-create-iam-role.md), choose **I will provide an IAM role ARN** and enter your existing **IAM role ARN**\.
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. In the **Code** pane, add the following state machine definition using the Amazon Resource Name of [the Lambda function that you created earlier](#create-iterate-pattern-create-lambda-function)\.

   ```
   {
       "Comment": "Iterator State Machine Example",
       "StartAt": "ConfigureCount",
       "States": {
           
           "ConfigureCount": {
               "Type": "Pass",
               "Result": {
                   "count": 10,
                   "index": -1,
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

   This state machine references the `Iterator` Lambda function you created earlier\. Be sure to update the Amazon Resource Name in the previous `Iterator` state\. For more information about the Amazon States Language, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.  
![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-workflow.png)![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Choose **Create State Machine**\.

1. Choose **OK**\.

   The state machine is created and an acknowledgement page is displayed\.

## Step 5: Start a New Execution<a name="create-iterate-pattern-step-5"></a>

After you create your state machine, you can start an execution\.

### To start a new execution<a name="create-iterate-pattern-start-execution"></a>

1. On the **IterateCount** page, choose **New execution**\.

   The **New execution** page is displayed\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter your execution id here** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.  
![\[State machine execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-running.png)![\[State machine execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[State machine execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

   The execution increments in steps, tracking the count using your Lambda function\. On each iteration, it performs your example work\. 

1. \(Optional\) In the **Execution Details** section, choose the **Info** tab to view the **Execution Status** and the **Started** and **Closed** time stamps\.

1. Once the count reaches the number configured in the `ConfigureCount` state in your state machine, the execution quits iterating and ends\.  
![\[State machine execution complete\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-done.png)![\[State machine execution complete\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[State machine execution complete\]](http://docs.aws.amazon.com/step-functions/latest/dg/)