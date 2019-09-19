# Handling Error Conditions Using a State Machine<a name="tutorial-handling-error-conditions"></a>

In this tutorial, you create an AWS Step Functions state machine with a `Catch` field, which uses an AWS Lambda function to respond with conditional logic based on error message type\. This is a technique called *function error handling*\. 

For more information, see [Function Error Handling](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-mode-exceptions.html#nodejs-prog-model-custom-exceptions) in the *AWS Lambda Developer Guide*\.

**Note**  
You can also create state machines that `Retry` on timeouts or those that use `Catch` to transition to a specific state when an error or timeout occurs\. For examples of these error handling techniques, see [Examples Using Retry and Using Catch](concepts-error-handling.md#error-handling-examples)\.

**Topics**
+ [Step 1: Create an IAM Role for Lambda](#using-state-machine-error-conditions-step-1)
+ [Step 2: Create a Lambda Function That Fails](#using-state-machine-error-conditions-step-2)
+ [Step 3: Test the Lambda Function](#using-state-machine-error-conditions-step-3)
+ [Step 4: Create a State Machine with a Catch Field](#using-state-machine-error-conditions-step-4)
+ [Step 5: Start a New Execution](#using-state-machine-error-conditions-step-5)

## Step 1: Create an IAM Role for Lambda<a name="using-state-machine-error-conditions-step-1"></a>

Both AWS Lambda and AWS Step Functions can execute code and access AWS resources \(for example, data stored in Amazon S3 buckets\)\. To maintain security, you must grant Lambda and Step Functions access to these resources\.

Lambda requires you to assign an AWS Identity and Access Management \(IAM\) role when you create a Lambda function, in the same way Step Functions requires you to assign an IAM role when you create a state machine\.

### <a name="tutorial-handling-error-conditions-to-create-a-role-for-use-with-lambda"></a>

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/home) and choose **Roles**, **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS service**, select **Lambda** from the list, and then choose **Next: Permissions**\.
**Note**  
The role is automatically provided with a trust relationship that allows Lambda to use the role\.

1. On the **Attach permissions policy** page, choose **Next: Review**\.

1. On the **Review** page, enter `MyLambdaRole` for **Role Name**, and then choose **Create role**\.

The IAM role appears in the list of roles\.

## Step 2: Create a Lambda Function That Fails<a name="using-state-machine-error-conditions-step-2"></a>

Use a Lambda function to simulate an error condition\.

**Important**  
Ensure that your Lambda function is under the same AWS account and AWS Region as your state machine\.

### <a name="using-state-machine-error-conditions-simulate"></a>

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

   Choose **Create a function**\.

1. In the **Blueprints** section, enter `step-functions` into the filter, and then choose the **step\-functions\-error** blueprint\.

1. In the **Basic information** section, configure your Lambda function:

   1. For **Name**, enter `FailFunction`\.

   1. For **Role**, select **Choose an existing role**\.

   1. For **Existing role**, choose [the Lambda role that you created earlier](#tutorial-handling-error-conditions-to-create-a-role-for-use-with-lambda)\.
**Note**  
If the IAM role that you created doesn't appear in the list, the role might still need a few minutes to propagate to Lambda\.

1. The following code is displayed in the **Lambda function code** pane\.

   ```
   'use strict';
   
   exports.handler = (event, context, callback) => {
       function CustomError(message) {
           this.name = 'CustomError';
           this.message = message;
       }
       CustomError.prototype = new Error();
   
       const error = new CustomError('This is a custom error!');
       callback(error);
   };
   ```

   The `context` object returns the error message `This is a custom error!`\.

1. Choose **Create function**\.

   When your Lambda function is created, note its Amazon Resource Name \(ARN\) in the upper\-right corner of the page\. For example:

   ```
   arn:aws:lambda:us-east-1:123456789012:function:FailFunction
   ```

## Step 3: Test the Lambda Function<a name="using-state-machine-error-conditions-step-3"></a>

Test your Lambda function to see it in operation\.

### <a name="to-test-your-lam-function"></a>

1. On the ***FailFunction*** page, choose **Test**\.

1. In the **Configure test event** dialog box, enter `FailFunction` for **Event name**, and then choose **Create**\.

1. On the ***FailFunction*** page, **Test** your Lambda function\.

   The results of the test \(the simulated error\) are displayed at the bottom of the page\.

## Step 4: Create a State Machine with a Catch Field<a name="using-state-machine-error-conditions-step-4"></a>

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine that uses a `Task` state with a `Catch` field\. Add a reference to your Lambda function in the `Task` state\. The Lambda function is invoked and fails during execution\. Step Functions retries the function twice using exponential backoff between retries\.

### <a name="using-state-machine-error-conditions-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Create state machine**\.

1. On the **Create a state machine** page, choose **Templates**, and then choose **Catch failure**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-catch-failure.png)

1. **Name your state machine**, for example, `Catchfailure`\.
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In the **Code** pane, add the ARN of [the Lambda function that you created earlier](#using-state-machine-error-conditions-simulate) to the `Resource` field, for example:

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

   For more information about the syntax of the `Retry` field, see [Examples Using Retry and Using Catch](concepts-error-handling.md#error-handling-examples)\.
**Note**  
Unhandled errors in Lambda are reported as `Lambda.Unknown` in the error output\. These include out\-of\-memory errors, function timeouts, and hitting the concurrent Lambda invoke limit\. You can match on `Lambda.Unknown`, `States.ALL`, or `States.TaskFailed` to handle these errors\. For more information about Lambda `Handled` and `Unhandled` errors, see `FunctionError` in the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_ResponseSyntax)\. 

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-catch-failure-preview.png)

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Choose **Next**\.

1. Create or enter an IAM role:
   + To create an IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the list, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. Choose **Create state machine**\.

## Step 5: Start a New Execution<a name="using-state-machine-error-conditions-step-5"></a>

After you create your state machine, you can start an execution\.

### <a name="using-state-machine-error-conditions-start-executino"></a>

1. On the ***CatchStateMachine*** page, choose **New execution**\.

   The **New execution** page is displayed\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. In the **Execution Details** section, expand the **Output** section to view the output of your workflow\.  
![\[Execution output\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-retry-state-machine-execution-output.png)

1. To view your custom error message, select `CreateAccount` in the **Visual workflow** and expand the **Output** section\.  
![\[Error output\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-retry-state-machine-error-output.png)
**Note**  
You can preserve the state input with the error by using `ResultPath`\. See [Use ResultPath to Include Both Error and Input in a `Catch`](input-output-resultpath.md#input-output-resultpath-catch)