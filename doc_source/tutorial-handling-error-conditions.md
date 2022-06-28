# Handling Error Conditions Using a Step Functions State Machine<a name="tutorial-handling-error-conditions"></a>

In this tutorial, you create an AWS Step Functions state machine with a `Catch` field\. The `Catch` field uses an AWS Lambda function to respond with conditional logic based on error message type\. This is a technique called *function error handling*\. 

For more information, see [Function Error Handling](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-mode-exceptions.html#nodejs-prog-model-custom-exceptions) in the *AWS Lambda Developer Guide*\.

**Note**  
You can also create state machines that `Retry` on timeouts or those that use `Catch` to transition to a specific state when an error or timeout occurs\. For examples of these error handling techniques, see [Examples Using Retry and Using Catch](concepts-error-handling.md#error-handling-examples)\.

**Topics**
+ [Step 1: Create a Lambda Function That Fails](#using-state-machine-error-conditions-step-2)
+ [Step 2: Test the Lambda Function](#using-state-machine-error-conditions-step-3)
+ [Step 3: Create a State Machine with a Catch Field](#using-state-machine-error-conditions-step-4)
+ [Step 4: Start a New Execution](#using-state-machine-error-conditions-step-5)

## Step 1: Create a Lambda Function That Fails<a name="using-state-machine-error-conditions-step-2"></a>

Use a Lambda function to simulate an error condition\.

**Important**  
Ensure that your Lambda function is under the same AWS account and AWS Region as your state machine\.

### <a name="using-state-machine-error-conditions-simulate"></a>

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

   Choose **Create function**\.

1. Choose **Use a blueprint**, enter `step-functions` into the filter, and then choose the **step\-functions\-error** blueprint\.

1. Choose **Configure**\.

1. In the **Basic information** section, configure your Lambda function:

   1. For **Name**, enter `FailFunction`\.

   1. For **Role**, choose **Create a new role with basic Lambda permissions**\.

1. The following code is displayed in the **Lambda function code** pane\.

   ```
   exports.handler = async (event, context) => {
       function CustomError(message) {
           this.name = 'CustomError';
           this.message = message;
       }
       CustomError.prototype = new Error();
   
       throw new CustomError('This is a custom error!');
   };
   ```

   The `context` object returns the error message `This is a custom error!`\.

1. Choose **Create function**\.

   After your Lambda function is created, copy the function's Amazon Resource Name \(ARN\) displayed in the upper\-right corner of the page\. To copy the ARN, click ![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-sm-using-lambda-copy-icon.png)![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)\. The following is an example ARN:

   ```
   arn:aws:lambda:us-east-1:123456789012:function:FailFunction
   ```

1. Choose **Deploy**\.

## Step 2: Test the Lambda Function<a name="using-state-machine-error-conditions-step-3"></a>

Test your Lambda function to see it in operation\.

### <a name="to-test-your-lam-function"></a>

1. On the ***FailFunction*** page, choose **Test**\.

1. In the **Configure test event** dialog box, enter `FailFunction` for **Event name**, and then choose **Create**\.

1. On the ***FailFunction*** page, choose **Test** to test your Lambda function\.

   The results of the test \(the simulated error\) are displayed in a new tab **Execution results**\.

## Step 3: Create a State Machine with a Catch Field<a name="using-state-machine-error-conditions-step-4"></a>

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine that uses a `Task` state with a `Catch` field\. Add a reference to your Lambda function in the `Task` state\. The Lambda function is invoked and fails during execution\. Step Functions retries the function twice using exponential backoff between retries\.

### <a name="using-state-machine-error-conditions-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Create state machine**\.

1. On the **Choose authoring method** page, choose **Write your workflow in code**\. 

1. For **Type**, retain the default selection, that is, **Standard**\.

1. In the **Definition** pane, paste the following code, but replace the ARN of [the Lambda function that you created earlier](#using-state-machine-error-conditions-simulate) in the `Resource` field\.

   ```
   {
      "Comment": "A Catch example of the Amazon States Language using an AWS Lambda function",
      "StartAt": "CreateAccount",
      "States": {
         "CreateAccount": {
            "Type": "Task",
            "Resource": "arn:aws:lambda:us-east-1:123456789012:function:FailFunction",
            "Catch": [ {
               "ErrorEquals": ["CustomError"],
               "Next": "CustomErrorFallback"
            }, {
               "ErrorEquals": ["States.TaskFailed"],
               "Next": "ReservedTypeFallback"
            }, {
               "ErrorEquals": ["States.ALL"],
               "Next": "CatchAllFallback"
            } ],
            "End": true
         },
         "CustomErrorFallback": {
            "Type": "Pass",
            "Result": "This is a fallback from a custom Lambda function exception",
            "End": true
         },
         "ReservedTypeFallback": {
            "Type": "Pass",
            "Result": "This is a fallback from a reserved error code",
            "End": true
         },
         "CatchAllFallback": {
            "Type": "Pass",
            "Result": "This is a fallback from any error code",
            "End": true
         }
      }
   }
   ```

   This is a description of your state machine using the Amazon States Language\. It defines a single `Task` state named `CreateAccount`\. For more information, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

   For more information about the syntax of the `Retry` field, see [Examples using Retry and using Catch](concepts-error-handling.md#error-handling-examples)\.
**Note**  
Unhandled errors in Lambda are reported as `Lambda.Unknown` in the error output\. These include out\-of\-memory errors and function timeouts\. You can match on `Lambda.Unknown`, `States.ALL`, or `States.TaskFailed` to handle these errors\. When Lambda hits the maximum number of invocations, the error is `Lambda.TooManyRequestsException`\. For more information about Lambda function errors, see [Error handling and automatic retries](https://docs.aws.amazon.com/lambda/latest/dg/invocation-retries.html) in the *AWS Lambda Developer Guide*\. 

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Choose **Next**\.

1. Enter a **Name** for your state machine, such as `Catchfailure`\.

1. In **Permissions**, choose **Create new role**\.

1. Choose **Create state machine**\.

## Step 4: Start a New Execution<a name="using-state-machine-error-conditions-step-5"></a>

After you create your state machine, you can start an execution\.

### <a name="using-state-machine-error-conditions-start-executino"></a>

1. On the ***Catchfailure*** page, choose **Start execution**\.

   The **Start execution** dialog box is displayed\.

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. Go to the **Execution output** tab to view the output of your workflow\.  
![\[Execution output\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-retry-state-machine-execution-output.png)

1. To view your custom error message, select `CreateAccount` in the **Graph inspector** pane and choose the **Step output** tab\.  
![\[Error output\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-retry-state-machine-error-output.png)
**Note**  
You can preserve the state input with the error by using `ResultPath`\. See [Use ResultPath to Include Both Error and Input in a `Catch`](input-output-resultpath.md#input-output-resultpath-catch)\.