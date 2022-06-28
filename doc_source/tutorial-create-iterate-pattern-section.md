# Iterating a Loop Using Lambda<a name="tutorial-create-iterate-pattern-section"></a>

In this tutorial, you implement a design pattern that uses a state machine and an AWS Lambda function to iterate a loop a specific number of times\. 

Use this design pattern any time you need to keep track of the number of loops in a state machine\. This implementation can help you break up large tasks or long\-running executions into smaller chunks, or to end an execution after a specific number of events\. You can use a similar implementation to periodically end and restart a long\-running execution to avoid exceeding service quotas for AWS Step Functions, AWS Lambda, or other AWS services\.

Before you begin, go through the [Creating a Step Functions State Machine That Uses Lambda](tutorial-creating-lambda-state-machine.md) tutorial to ensure you are familiar with using Lambda and Step Functions together\.

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

1. On the **Create function** page, choose **Author from scratch**\.

1. In the **Basic information** section, configure your Lambda function, as follows:

   1. For **Function name**, enter `Iterator`\.

   1. For **Runtime**, choose **Node\.js 14\.x**\.

   1. In **Change default execution role**, choose **Create a new role with basic Lambda permissions**\.

   1. Choose **Create function**\.

   1. After your Lambda function is created, copy the function's Amazon Resource Name \(ARN\) displayed in the upper\-right corner of the page\. To copy the ARN, click ![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-sm-using-lambda-copy-icon.png)![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)\. The following is an example ARN:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:Iterator
      ```

1. Copy the following code for the Lambda function into the **Code source** section of the ***Iterator*** page\.

   ```
   exports.handler = function iterator (event, context, callback) {
     let index = event.iterator.index
     let step = event.iterator.step
     let count = event.iterator.count
    
     index = index + step
    
     callback(null, {
       index,
       step,
       count,
       continue: index < count
     })
   }
   ```

   This code accepts input values for `count`, `index`, and `step`\. It increments the `index` by the value of `step` and returns these values, and the Boolean `continue`\. The value of `continue` is `true` if `index` is less than `count`\.

1. Choose **Deploy**\.

## Step 2: Test the Lambda Function<a name="create-iterate-pattern-step-2"></a>

Run your Lambda function with numeric values to see it in operation\. You can provide input values for your Lambda function that mimic an iteration, to see what output you get with specific input values\. 

### To test your Lambda function<a name="create-iterate-pattern-test-lambda-function"></a>

1. Choose **Test**\.

1. In the **Configure test event** dialog box, enter `TestIterator` in the **Event name** box\.

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

1. On the ***Iterator*** page, choose **Test** to test your Lambda function\.

   The results of the test are displayed in the **Execution results** tab\. 

1. Choose the **Execution results** tab to see the output\.

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

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine with the following states to invoke [the Lambda function that you created earlier in Step 1](#create-iterate-pattern-create-lambda-function)\.
+ `ConfigureCount` – Sets the default values for `count`, `index`, and `step`\.
+ `Iterator` – References the Lambda function you created earlier, passing in the values configured in `ConfigureCount`\.
+ `IsCountReached` – A choice state that either runs your sample work again or goes to `Done`, based on a Boolean value returned from your `Iterator` Lambda function\.
+ `ExampleWork` – A stub for the work you want to accomplish in your execution\. In this example, it's a `Pass` state\. In an actual implementation, this would be a `Task` state\.
+ `Done` – The end state of your execution\.

### <a name="create-iterate-pattern-create-state-machine"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home), and then choose **Create a state machine**\.
**Important**  
Ensure that your state machine is under the same AWS account and Region as the Lambda function you created earlier\.

1. On the **Choose authoring method** page, choose **Write your workflow in code**\.

1. For **Type**, retain the default selection, that is, **Standard**\.

1. In the **Definition** pane, paste the following code, but replace the ARN of [the Lambda function that you created earlier](#create-iterate-pattern-create-lambda-function) in the `Resource` field\.

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

   Be sure to update the ARN in the `Iterator` state above, so that it references [the Lambda function that you created earlier](#create-iterate-pattern-create-lambda-function)\.

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.

   This graph shows the logic expressed in the previous state machine code\.   
![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-workflow.png)

   For more information about the Amazon States Language, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Choose **Next**\.

1. Enter a **Name** for your state machine, such as `IterateCount`\.
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In **Permissions**, choose **Create new role**\.

1. Choose **Create state machine**\.

## Step 4: Start a New Execution<a name="create-iterate-pattern-step-4"></a>

After you create your state machine, you can start an execution\.

### <a name="create-iterate-pattern-start-execution"></a>

1. On the **IterateCount** page, choose **Start execution**\.

   The **Start execution** dialog box is displayed\.

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, showing your running execution\.  
![\[State machine execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-running.png)

   The execution increments in steps, tracking the count using your Lambda function\. On each iteration, it performs the example work referenced in the `ExampleWork` state in your state machine\. 

1. \(Optional\) In the **Details** tab, view the **Execution Status** and the timestamps for **Started** and **End Time** of the execution\.

   When the count reaches the number specified in the `ConfigureCount` state in your state machine, the execution quits iterating and ends\.  
![\[State machine execution complete\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-done.png)