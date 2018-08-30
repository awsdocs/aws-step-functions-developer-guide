# Creating a Lambda State Machine<a name="tutorial-creating-lambda-state-machine"></a>

In this tutorial you'll create an AWS Step Functions state machine that uses a AWS Lambda function to implement a `Task` state\. A `Task` state is a simple state that performs a single unit of work\.

Lambda is well\-suited for implementing `Task` states, because Lambda functions are *stateless* \(they have a predictable input\-output relationship\), easy to write, and don't require deploying code to a server instance\. You can write code in the AWS Management Console or your favorite editor, and AWS handles the details of providing a computing environment for your function and running it\.

**Topics**
+ [Step 1: Creating an IAM Role for Lambda](#create-lambda-state-machine-step-1)
+ [Step 2: Creating a Lambda Function](#create-lambda-state-machine-step-2)
+ [Step 3: Testing the Lambda Function](#create-lambda-state-machine-step-3)
+ [Step 4: Creating a State Machine](#create-lambda-state-machine-step-4)
+ [Step 5: Starting a New Execution](#create-lambda-state-machine-step-5)

## Step 1: Creating an IAM Role for Lambda<a name="create-lambda-state-machine-step-1"></a>

Both Lambda and Step Functions can execute code and access AWS resources \(for example, data stored in Amazon S3 buckets\)\. To maintain security, you must grant Lambda and Step Functions access to these resources\.

Lambda requires you to assign an IAM role when you create a Lambda function in the same way Step Functions requires you to assign an IAM role when you create a state machine\.

### To create a role for Lambda<a name="create-lambda-state-machine-to-create-a-role-for-use-with-lambda"></a>

You can use the IAM console to create a service\-linked role\.

**To create a role \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\. Then choose **Create role**\.

1. Choose the **AWS Service** role type, and then choose **Lambda**\.

1. Choose the **Lambda** use case\. Use cases are defined by the service to include the trust policy required by the service\. Then choose **Next: Permissions**\.

1. Choose one or more permissions policies to attach to the role\. Select the box next to the policy that assigns the permissions that you want the role to have, and then choose **Next: Review**\.

1. Enter a **Role name**\.

1. \(Optional\) For **Role description**, edit the description for the new service\-linked role\.

1. Review the role and then choose **Create role**\.

## Step 2: Creating a Lambda Function<a name="create-lambda-state-machine-step-2"></a>

Your Lambda function receives input \(a name\) and returns a greeting that includes the input value\.

### To create the Lambda function<a name="create-lambda-state-machine-create-lambda-function"></a>

**Important**  
Ensure that your Lambda function is under the same AWS account and region as your state machine\.

1. Log in to the [Lambda console](https://console.aws.amazon.com/lambda/home) and choose **Create a function**\.

1. In the **Blueprints** section, choose **Author from scratch**\.

1. In the **Basic information** section, configure your Lambda function:

   1. For **Name**, type `HelloFunction`\.

   1. For **Role**, select **Choose an existing role**\.

   1. For **Existing role**, select [the Lambda role that you created earlier](#create-lambda-state-machine-to-create-a-role-for-use-with-lambda)\.
**Note**  
If the IAM role that you created doesn't appear in the list, the role might still need a few minutes to propagate to Lambda\.

   1. Choose **Create function**\.

      When your Lambda function is created, note its Amazon Resource Name \(ARN\) in the upper\-right corner of the page\. For example:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:HelloFunction
      ```

1. Copy the following code for the Lambda function into the **Function code** section of the ***HelloFunction*** page:

   ```
   exports.handler = (event, context, callback) => {
       callback(null, "Hello, " + event.who + "!");
   };
   ```

   This code assembles a greeting using the `who` field of the input data, which is provided by the `event` object passed into your function\. You will add input data for this function later, when you [start a new execution](#create-lambda-state-machine-start-execution)\. The `callback` method returns the assembled greeting from your function\.

1. Choose **Save**\.

## Step 3: Testing the Lambda Function<a name="create-lambda-state-machine-step-3"></a>

Test your Lambda function to see it in operation\.

### To test your Lambda function<a name="create-lambda-state-machine-test-lambda-function"></a>

1. On the **Select a test event** drop\-down, choose **Configure test event** and type `HelloFunction` for **Event name**\.

1. Replace the example data with the following:

   ```
   {
       "who": "AWS Step Functions"
   }
   ```

   The `"who"` entry corresponds to the `event.who` field in your Lambda function, completing the greeting\. You will use the same input data when running the function as a Step Functions task\.

1. Choose **Create**\.

1. On the ***HelloFunction*** page, **Test** your Lambda function using the new data\.

   The results of the test are displayed at the top of the page\. Expand **Details** to see the output\.

## Step 4: Creating a State Machine<a name="create-lambda-state-machine-step-4"></a>

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine with a `Task` state\. Add a reference to your Lambda function in the `Task` state\. The Lambda function is invoked when an execution of the state machine reaches the `Task` state\.

### To create the state machine<a name="create-lambda-state-machine-create"></a>

1. Log in to the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Create a state machine**\.

1. On the **Create a state machine** page, select **Author from scratch** and enter a **Name for your state machine**, for example `LambdaStateMachine`\.
**Note**  
State machine names must be 1–80 characters in length, must be unique for your account and region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Create or enter an IAM role\.
   + To create a new IAM role for Step Functions, choose **Create a role for me**, and then choose **I acknowledge that Step Functions will create an IAM role which allows access to my Lambda functions\.**
   + If you have [previously created an IAM role for Step Functions](procedure-create-iam-role.md), choose **I will provide an IAM role ARN** and enter your existing **IAM role ARN**\.
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. In the **State machine definition** pane, add the following state machine definition using the ARN of [the Lambda function that you created earlier](#create-lambda-state-machine-create-lambda-function), for example:

   ```
   {
     "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
     "StartAt": "HelloWorld",
     "States": {
       "HelloWorld": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
         "End": true
       }
     }
   }
   ```

   This is a description of your state machine using the Amazon States Language\. It defines a single `Task` state named `HelloWorld`\. For more information, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.
**Note**  
You can also set up a `Retry` for `Task` states\. As a best practice, ensure production code can handle Lambda service exceptions \(`Lambda.ServiceException` and `Lambda.SdkclientException`\)\. For more information see:   
[Handle Lambda Service Exceptions](bp-lambda-serviceexception.md)\. 
[Retrying After an Error](concepts-error-handling.md#error-handling-retrying-after-an-error)\. 

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/hello-state-machine-preview.png)

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Choose **Create State Machine**\.

## Step 5: Starting a New Execution<a name="create-lambda-state-machine-step-5"></a>

After you create your state machine, you can start an execution\.

### To start a new execution<a name="create-lambda-state-machine-start-execution"></a>

1. On the ***LambdaStateMachine*** page, choose **Start execution**\.

   The **New execution** page is displayed\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In the execution input area, replace the example data with the following:

   ```
   {
       "who" : "AWS Step Functions"
   }
   ```

    `"who"` is the key name that your Lambda function uses to get the name of the person to greet\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. To view the results of your execution, expand the **Output** section under **Execution details**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-state-machine-execution-output.png)
