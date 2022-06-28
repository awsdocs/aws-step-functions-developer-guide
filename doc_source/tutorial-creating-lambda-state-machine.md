# Creating a Step Functions State Machine That Uses Lambda<a name="tutorial-creating-lambda-state-machine"></a>

In this tutorial, you will create a single\-step workflow using AWS Step Functions to invoke an AWS Lambda function\.

**Note**  
In Step Functions, a workflow is called a *state machine*, which is a series of event\-driven steps\. Each step in a workflow is called a *state*\. A `Task` state represents a unit of work that another AWS service, such as AWS Lambda, performs\. A `Task` state can call any AWS service or API\. For more information, see:  
[What is AWS Step Functions?](welcome.md)
[Call other AWS services](connect-to-services.md)

Lambda is well\-suited for `Task` states, because Lambda functions are *serverless* and easy to write\. You can write code in the AWS Management Console or your favorite editor\. AWS handles the details of providing a computing environment for your function and running it\.

**Topics**
+ [Step 1: Create a Lambda Function](#create-lambda-state-machine-step-2)
+ [Step 2: Test the Lambda Function](#create-lambda-state-machine-step-3)
+ [Step 3: Create a State Machine](#create-lambda-state-machine-step-4)
+ [Step 4: Start a New Execution](#create-lambda-state-machine-step-5)

## Step 1: Create a Lambda Function<a name="create-lambda-state-machine-step-2"></a>

Your Lambda function receives event data and returns a greeting message\.

### <a name="create-lambda-state-machine-create-lambda-function"></a>

**Important**  
Ensure that your Lambda function is under the same AWS account and AWS Region as your state machine\.

1. Open the [Lambda console](https://console.aws.amazon.com/lambda/home) and choose **Create function**\.

1. On the **Create function** page, choose **Author from scratch**\.

1. In the **Basic information** section, configure your Lambda function:

   1. For **Function name**, enter `HelloFunction`\.

   1. For **Runtime**, choose **Node\.js 14\.x**\.

   1. In **Change default execution role**, choose **Create a new role with basic Lambda permissions**\.

   1. Choose **Create function**\.

   1. After your Lambda function is created, copy the function's Amazon Resource Name \(ARN\) displayed in the upper\-right corner of the page\. To copy the ARN, click ![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-sm-using-lambda-copy-icon.png)![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[copy Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)\. The following is an example ARN:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:HelloFunction
      ```

1. Copy the following code for the Lambda function into the **Code source** section of the ***HelloFunction*** page\.

   ```
   exports.handler = (event, context, callback) => {
       callback(null, "Hello from " + event.who + "!");
   };
   ```

   This code assembles a greeting using the `who` field of the input data, which is provided by the `event` object passed into your function\. You add input data for this function later, when you [start a new execution](#create-lambda-state-machine-start-execution)\. The `callback` method returns the assembled greeting from your function\.

1. Choose **Deploy**\.

## Step 2: Test the Lambda Function<a name="create-lambda-state-machine-step-3"></a>

Test your Lambda function to see it in operation\.

### <a name="create-lambda-state-machine-test-lambda-function"></a>

1. Choose **Test**\.

1. In the **Configure test event** dialog box, enter `HelloEvent` in the **Event name** box\.

1. Replace the example data with the following\.

   ```
   {
       "who": "AWS Step Functions"
   }
   ```

   The `"who"` entry corresponds to the `event.who` field in your Lambda function, completing the greeting\. You will input the same input data when you run your state machine\.

1. Choose **Create**\.

1. On the ***HelloFunction*** page, choose **Test** to test your Lambda function using the new data\.

   The results of the test are displayed in the **Execution results** tab\. 

1. Choose the **Execution results** tab to see the output\.

## Step 3: Create a State Machine<a name="create-lambda-state-machine-step-4"></a>

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine that invokes [the Lambda function that you created earlier in Step 1](#create-lambda-state-machine-create-lambda-function)\.

### <a name="create-lambda-state-machine-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Create a state machine**\.
**Important**  
Ensure that your state machine is under the same AWS account and Region as the Lambda function you created earlier\.

1. On the **Choose authoring method** page, choose ** Design your workflow visually**\.

1. For **Type**, retain the default selection, that is, **Standard**\.

1. Choose **Next**\. This will open Workflow Studio\.

1. From the States browser on the left, choose the **Actions** panel\.

   1. Drag and drop the **AWS Lambda Invoke** API into the empty state labelled **Drag first state here**\.  
![\[States browser\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-states-browser.png)

1. In the ** Inspector** panel on the right, configure the Lambda function and its name:

   1. Choose **Configuration**, and then edit the **State name**, if required\.

   1. In the **API Parameters** section, choose [the Lambda function that you created earlier](#create-lambda-state-machine-create-lambda-function) in the **Function name** dropdown list\.

   1. Retain the default selection in the **Payload** dropdown list\.

1. Choose **Next**\.

1. On the **Review generated code** page, review the state machine's Amazon States Language \(ASL\) definition, which is automatically generated based on your selections in the **Actions** and ** Inspector** panel\.

1. Choose **Next**\.

1. Enter a **Name for your state machine**, for example, `LambdaStateMachine`\.
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In **Execution role**, under the **Permissions** section, choose **Create new role**\.

1. Choose **Create state machine**\.

## Step 4: Start a New Execution<a name="create-lambda-state-machine-step-5"></a>

After you create your state machine, you start an execution\.

### <a name="create-lambda-state-machine-start-execution"></a>

1. On the ***LambdaStateMachine*** page, choose **Start execution**\.

   The **Start execution** dialog box is displayed\.

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In the execution input area, replace the example data with the following\.

   ```
   {
       "who" : "AWS Step Functions"
   }
   ```

    `"who"` is the key name that your Lambda function uses to get the name of the person to greet\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. To view the results of your execution, choose the **Execution output** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-console-state-machine-execution-output.png)

**Note**  
You can also pass payloads while invoking Lambda from a state machine\. For more information and examples about invoking Lambda by passing payload in the `Parameters` field, see [Invoke Lambda with Step Functions](connect-lambda.md)\.