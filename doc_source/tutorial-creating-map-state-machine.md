# Use a Map State to Call Lambda Multiple Times<a name="tutorial-creating-map-state-machine"></a>

In this tutorial, you will learn how to use a `Map` state to call a AWS Lambda function multiple times, based on the state machine input\.

The [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial walks you though creating a state machine that calls a Lambda function\. If you have completed that tutorial, skip to [Step 4](#create-map-lambda-state-machine-step-4) and use the IAM role and Lambda function that you previously created\.

**Topics**
+ [Step 1: Create an IAM Role for Lambda](#create-map-lambda-state-machine-step-1)
+ [Step 2: Create a Lambda Function](#create-map-lambda-state-machine-step-2)
+ [Step 3: Test the Lambda Function](#create-map-lambda-state-machine-step-3)
+ [Step 4: Create a State Machine](#create-map-lambda-state-machine-step-4)
+ [Step 5: Start a New Execution](#create-map-lambda-state-machine-step-5)

## Step 1: Create an IAM Role for Lambda<a name="create-map-lambda-state-machine-step-1"></a>

Both AWS Lambda and AWS Step Functions can execute code and access AWS resources \(for example, data stored in Amazon S3 buckets\)\. To maintain security, you must grant Lambda and Step Functions access to these resources\.

Lambda requires you to assign an AWS Identity and Access Management \(IAM\) role when you create a Lambda function, in the same way Step Functions requires you to assign an IAM role when you create a state machine\.

### <a name="create-map-lambda-state-machine-to-create-a-role-for-use-with-lambda"></a>

You use the IAM console to create a service\-linked role\.

**To create a role \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\. Then choose **Create role**\.

1. Choose the **AWS Service** role type, and then choose **Lambda**\.

1. Choose the **Lambda** use case\. Use cases are defined by the service to include the trust policy required by the service\. Then choose **Next: Permissions**\.

1. Choose one or more permissions policies to attach to the role \(for example, `AWSLambdaBasicExecutionRole`\)\. See [AWS Lambda Permissions Model](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html)\.

    Select the box next to the policy that assigns the permissions that you want the role to have, and then choose **Next: Review**\.

1. Enter a **Role name**\.

1. \(Optional\) For **Role description**, edit the description for the new service\-linked role\.

1. Review the role, and then choose **Create role**\.

## Step 2: Create a Lambda Function<a name="create-map-lambda-state-machine-step-2"></a>

Your Lambda function receives input \(a name\) and returns a greeting that includes the input value\.

### <a name="create-map-lambda-state-machine-create-lambda-function"></a>

**Important**  
Ensure that your Lambda function is under the same AWS account and AWS Region as your state machine\.

1. Open the [Lambda console](https://console.aws.amazon.com/lambda/home) and choose **Create a function**\.

1. In the **Blueprints** section, choose **Author from scratch**\.

1. In the **Basic information** section, configure your Lambda function:

   1. For **Function name**, enter `HelloFunction`\.

   1. For **Runtime**, choose **Node\.js 6\.10**\.

   1. For **Role**, select **Choose an existing role**\.

   1. For **Existing role**, select [the Lambda role that you created earlier](#create-map-lambda-state-machine-to-create-a-role-for-use-with-lambda)\.
**Note**  
If the IAM role that you created doesn't appear in the list, the role might still need a few minutes to propagate to Lambda\.

   1. Choose **Create function**\.

      When your Lambda function is created, note its Amazon Resource Name \(ARN\) in the upper\-right corner of the page\. For example:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:HelloFunction
      ```

1. Copy the following code for the Lambda function into the **Function code** section of the ***HelloFunction*** page\.

   ```
   exports.handler = (event, context, callback) => {
       callback(null, "Hello, " + event.who + "!");
   };
   ```

   This code assembles a greeting using the `who` field of the input data, which is provided by the `event` object passed into your function\. You add input data for this function later, when you [start a new execution](#create-map-lambda-state-machine-start-execution)\. The `callback` method returns the assembled greeting from your function\.

1. Choose **Save**\.

## Step 3: Test the Lambda Function<a name="create-map-lambda-state-machine-step-3"></a>

Test your Lambda function to see it in operation\.

### <a name="create-map-lambda-state-machine-test-lambda-function"></a>

1. For **Select a test event**, choose **Configure test event**\. For **Event name**, enter `HelloFunction`\.

1. Replace the example data with the following\.

   ```
   {
       "who": "AWS Step Functions"
   }
   ```

   The `"who"` entry corresponds to the `event.who` field in your Lambda function, completing the greeting\. You will use the same input data when running the function as a Step Functions task\.

1. Choose **Create**\.

1. On the ***HelloFunction*** page, **Test** your Lambda function using the new data\.

   The results of the test are displayed at the top of the page\. Expand **Details** to see the output\.

## Step 4: Create a State Machine<a name="create-map-lambda-state-machine-step-4"></a>

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine with a `Map` state\. Add a `Task`state with a reference to your Lambda\. The Lambda function is invoked for each iteration of the `Map` state, based on the state machine input\.

### <a name="create-map-lambda-state-machine-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Create a state machine**\.

1. On the **Define state machine** page, choose **Author with code snippets**\. Enter a **Name for your state machine**, for example, `MapLambda`\.
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In the **State machine definition** pane, add the following state machine definition using the ARN of [the Lambda function that you created earlier](#create-map-lambda-state-machine-create-lambda-function), for example:

   ```
   {
     "StartAt": "ExampleMapState",
     "States": {
       "ExampleMapState": {
         "Type": "Map",
         "Iterator": {
            "StartAt": "CallLambda",
            "States": {
              "CallLambda": {
                "Type": "Task",
                "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
                "End": true
              }
            }
         },
         "End": true
       }
     }
   }
   ```

   This is a description of your state machine using the Amazon States Language\. It defines a `Map` state named `ExampleMapState` that includes a `Task` state \(`CallLambda`\) that calls your Lambda function\. For more information, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.
**Note**  
You can also set up a `Retry` for `Task` states\. As a best practice, ensure production code can handle Lambda service exceptions \(`Lambda.ServiceException` and `Lambda.SdkclientException`\)\. For more information, see:   
[Handle Lambda Service Exceptions](bp-lambda-serviceexception.md)\. 
[Retrying after an Error](concepts-error-handling.md#error-handling-retrying-after-an-error)\. 

   Choose **Next**\.

1. Create or enter an IAM role:
   + To create an IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the list, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. Select **Next**\.

## Step 5: Start a New Execution<a name="create-map-lambda-state-machine-step-5"></a>

After you create your state machine, you start an execution\.

### <a name="create-map-lambda-state-machine-start-execution"></a>

1. On the ***LambdaStateMachine*** page, choose **Start execution**\.

   The **New execution** page is displayed\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In the execution input area, replace the example data with the following\.

   ```
   [
     {
       "who": "bob"
     },
     {
       "who": "meg"
     },
     {
       "who": "joe"
     }
   ]
   ```

   Your `Map` state will iterate and run the `CallLambda` task state for each of these items in the input\. `"who"` is the key name that your Lambda function uses to get the name of the person to greet\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. To view the results of your execution, expand the **Output** section under **Execution details**\. 

   ```
   [
     "Hello, bob!",
     "Hello, meg!",
     "Hello, joe!"
   ]
   ```

   The output for each of the Lambda function executions are combined into the output for the state machine\.

For more information on using `Map` states, see the following\.
+ [](amazon-states-language-map-state.md)
+ [Map State Example](amazon-states-language-map-state.md#map-state-examples)
+ [Map State Input and Output Processing](amazon-states-language-map-state.md#amazon-states-language-map-state-output)
+ [ItemsPath](input-output-itemspath.md)