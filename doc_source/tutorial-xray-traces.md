# View X\-Ray traces in Step Functions<a name="tutorial-xray-traces"></a>

In this tutorial, you will learn how to use X\-Ray to trace errors that occur when running a state machine\. You can use [AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) to visualize the components of your state machine, identify performance bottlenecks, and troubleshoot requests that resulted in an error\. In this tutorial, you will create several Lambda functions that randomly produce errors, which you can then trace and analyze using X\-Ray\.

The [Creating a Step Functions State Machine That Uses Lambda](tutorial-creating-lambda-state-machine.md) tutorial walks you though creating a state machine that calls a Lambda function\. If you have completed that tutorial, skip to [Step 2](#create-xray-lambda-state-machine-step-4) and use the AWS Identity and Access Management \(IAM\) role that you previously created\.

**Topics**
+ [Step 1: Create an IAM Role for Lambda](#create-xray-lambda-state-machine-step-1)
+ [Step 2: Create a Lambda Function](#create-xray-lambda-state-machine-step-2)
+ [Step 3: Create two more Lambda functions](#create-xray-lambda-state-machine-step-3)
+ [Step 4: Create a State Machine](#create-xray-lambda-state-machine-step-4)
+ [Step 5: Start a New Execution](#create-xray-lambda-state-machine-step-5)

## Step 1: Create an IAM Role for Lambda<a name="create-xray-lambda-state-machine-step-1"></a>

Both AWS Lambda and AWS Step Functions can execute code and access AWS resources \(for example, data stored in Amazon S3 buckets\)\. To maintain security, you must grant Lambda and Step Functions access to these resources\.

Lambda requires you to assign an AWS Identity and Access Management \(IAM\) role when you create a Lambda function, in the same way Step Functions requires you to assign an IAM role when you create a state machine\.

### <a name="create-xray-lambda-state-machine-to-create-a-role-for-use-with-lambda"></a>

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

## Step 2: Create a Lambda Function<a name="create-xray-lambda-state-machine-step-2"></a>

Your Lambda function will randomly throw errors or time out, producing example data to view in X\-Ray\.

### <a name="create-xray-lambda-state-machine-create-lambda-function"></a>

**Important**  
Ensure that your Lambda function is under the same AWS account and AWS Region as your state machine\.

1. Open the [Lambda console](https://console.aws.amazon.com/lambda/home) and choose **Create function**\.

1. In the **Create function** section, choose **Author from scratch**\.

1. In the **Basic information** section, configure your Lambda function:

   1. For **Function name**, enter `TestFunction1`\.

   1. For **Runtime**, choose **Node\.js 12\.x**\.

   1. For **Role**, select **Choose an existing role**\.

   1. For **Existing role**, select [the Lambda role that you created earlier](#create-xray-lambda-state-machine-to-create-a-role-for-use-with-lambda)\.
**Note**  
If the IAM role that you created doesn't appear in the list, the role might still need a few minutes to propagate to Lambda\.

   1. Choose **Create function**\.

      When your Lambda function is created, note its Amazon Resource Name \(ARN\) in the upper\-right corner of the page\. For example:

      ```
      arn:aws:lambda:us-east-1:123456789012:function:TestFunction1
      ```

1. Copy the following code for the Lambda function into the **Function code** section of the ***TestFunction1*** page\.

   ```
   function getRandomSeconds(max) {
       return Math.floor(Math.random() * Math.floor(max)) * 1000;
   }
   function sleep(ms) {
       return new Promise(resolve => setTimeout(resolve, ms));
   }
   exports.handler = async (event, context) => {
       if(getRandomSeconds(4) === 0) {
           throw new Error("Something went wrong!");
       }   
       let wait_time = getRandomSeconds(5);
       await sleep(wait_time);
       return { 'response': true }
   };
   ```

   This code creates randomly timed failures, which will be used to generate example errors in your state machine that can be viewed and analyzed using X\-Ray traces\.

1. Choose **Save**\.

## Step 3: Create two more Lambda functions<a name="create-xray-lambda-state-machine-step-3"></a>

Create two more Lambda functions\.

### <a name="create-xray-lambda-state-machine-test-lambda-function"></a>

1. Repeat Step 2 to create two more Lambda functions\. For the next function, in **Function name**, enter `TestFunction2`\. For the last function, in **Function name**, enter `TestFunction3`\.

1. In the Lambda console, check that you now have three Lambda functions, `TestFunction1`, `TestFunction2`, and `TestFunction3`\.

## Step 4: Create a State Machine<a name="create-xray-lambda-state-machine-step-4"></a>

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine with three `Task` states\. Each `Task` state will a reference one of your three Lambda functions\. 

### <a name="create-xray-lambda-state-machine-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Create a state machine**\.

1. On the **Define state machine** page, choose **Author with code snippets**\. For **Type**, choose **Standard**\. 
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In the **State machine definition** pane, replace the example with the following state machine definition\. For each `Task` task state, use the ARN of [the corresponding Lambda function that you created earlier](#create-xray-lambda-state-machine-create-lambda-function), as shown in the following example\.

   ```
   {
     "StartAt": "CallTestFunction1",
     "States": {
       "CallTestFunction1": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:test-function1",
         "Catch": [
           {
             "ErrorEquals": [
               "States.TaskFailed"
             ],
             "Next": "AfterTaskFailed"
           }
         ],
         "Next": "CallTestFunction2"
       },
       "CallTestFunction2": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:test-function2",
         "Catch": [
           {
             "ErrorEquals": [
               "States.TaskFailed"
             ],
             "Next": "AfterTaskFailed"
           }
         ],
         "Next": "CallTestFunction3"
       },
       "CallTestFunction3": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:test-function3",
         "TimeoutSeconds": 5,
         "Catch": [
           {
             "ErrorEquals": [
               "States.Timeout"
             ],
             "Next": "AfterTimeout"
           },
           {
             "ErrorEquals": [
               "States.TaskFailed"
             ],
             "Next": "AfterTaskFailed"
           }
         ],
         "Next": "Succeed"
       },
       "Succeed": {
         "Type": "Succeed"
       },
       "AfterTimeout": {
         "Type": "Fail"
       },
       "AfterTaskFailed": {
         "Type": "Fail"
       }
     }
   }
   ```

   This is a description of your state machine using the Amazon States Language\. It defines three `Task` states named `CallTestFunction1`, `CallTestFunction2` and `CallTestFunction3`\. Each calls one of your three Lambda functions\. For more information, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

   Choose **Next**\.

1. Enter a **Name**, for example, `TraceFunctions`\.

1. Create or enter an IAM role:
   + To create an IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the list, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. In the **Tracing** pane, ensure that **Enable X\-Ray** tracing is selected\. This will let you view the X\-Ray traces of your state machine\.

1. Select **Create state machine**\.

## Step 5: Start a New Execution<a name="create-xray-lambda-state-machine-step-5"></a>

After you create your state machine, start an execution\.

### <a name="create-xray-lambda-state-machine-start-execution"></a>

1. On the ***TraceFunctions*** page, choose **Start execution**\.

   The **New execution** page is displayed\.

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\. Run several \(at least three\) executions\.

1. After the executions have finished, follow the **X\-Ray trace map** link\. You can view the trace while an execution is still running, but you may want to see the execution results before viewing the X\-Ray trace map\.  
![\[X-Ray enable\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-xray-link.png)

1. View the service map to identify where errors are occurring, connections with high latency, or traces for requests that were unsuccessful\. In this example, you can see how much traffic each function is receiving\. `TestFunction2` was called more often than `TestFunction3`, and `TestFunction1` was called more than twice as often as `TestFunction2`\.

   The service map indicates the health of each node by coloring it based on the ratio of successful calls to errors and faults:
   +  **Green** for successful calls 
   +  **Red** for server faults \(500 series errors\) 
   +  **Yellow** for client errors \(400 series errors\) 
   +  **Purple** for throttling errors \(429 Too Many Requests\)   
![\[X-Ray enable\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-xray-service.png)

   You can also choose a service node to view requests for that node, or an edge between two nodes to view requests that traveled that connection\. 

1. View the X\-Ray trace map to see what has happened for each execution\. The Timeline view shows a hierarchy of segments and subsegments\. The first entry in the list is the segment, which represents all data recorded by the service for a single request\. Below the segment are subsegments\. This example shows subsegments recorded by the Lambda functions\.  
![\[X-Ray enable\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-xray-trace.png)

For more information on understanding X\-Ray traces and using X\-Ray with Step Functions, see the [AWS X\-Ray and Step Functions](concepts-xray-tracing.md) 