# Creating an Activity State Machine<a name="tutorial-creating-activity-state-machine"></a>

You can coordinate task code in your state machine\. This tutorial introduces you to creating an activity\-based state machine using Java and AWS Step Functions\.

To complete this tutorial you'll need the following:
+ The [SDK for Java](https://aws.amazon.com/sdk-for-java/)\. The example activity in this tutorial is a Java application that uses the AWS SDK for Java to communicate with AWS\.
+ AWS credentials in the environment or in the standard AWS configuration file\. For more information, see [Set up Your AWS credentials](http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/set-up-creds.html) in the *AWS SDK for Java Developer Guide*\.

**Topics**
+ [Step 1: Creating a New Activity](#create-activity-state-machine-step-1)
+ [Step 2: Creating a State Machine](#create-activity-state-machine-step-2)
+ [Step 3: Implementing a Worker](#create-activity-state-machine-step-3)
+ [Step 4: Starting an Execution](#create-activity-state-machine-step-4)
+ [Step 5: Running and Stopping the Worker](#create-activity-state-machine-step-5)

## Step 1: Creating a New Activity<a name="create-activity-state-machine-step-1"></a>

You must make Step Functions aware of the *activity* whose *worker* \(a program\) you want to create\. Step Functions responds with an ARN that establishes an identity for the activity\. Use this identity to coordinate the information passed between your state machine and worker\.

**Important**  
Ensure that your activity task is under the same AWS account as your state machine\.

### To create the new activity task<a name="create-activity-state-machine-new-activity"></a>

1. In the [Step Functions console](https://console.aws.amazon.com/states/home), choose **Activities** in the left navigation panel\.

1. Choose **Create activity**\.

1. Type an **Activity Name**\. For example `get-greeting`, and choose **Create Activity**\.

1. When your activity task is created, note its Amazon Resource Name \(ARN\), for example:

   ```
   arn:aws:states:us-east-1:123456789012:activity:get-greeting
   ```

## Step 2: Creating a State Machine<a name="create-activity-state-machine-step-2"></a>

Create a state machine that will determine when your activity is invoked and when your worker should perform its primary work, collect its results, and return them\.

### To create the state machine<a name="create-activity-state-machine-create"></a>

1. In the [Step Functions console](https://console.aws.amazon.com/states/home), choose **State machines** in the left navigation panel\.

1. On the **State machines** page, choose **Create state machine**, select **Author from scratch**, and enter a name under **Details** \(for example `ActivityStateMachine)`\.
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and region, and must not contain any of the following:  
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

1. Under **State machine definition**, enter the following code, and include the ARN of [the activity task that you created earlier](#create-activity-state-machine-new-activity) in the `Resource` field, for example:

   ```
   {
     "Comment": "An example using a Task state.",
     "StartAt": "getGreeting",
     "Version": "1.0",
     "TimeoutSeconds": 300,
     "States":
     {
       "getGreeting": {
         "Type": "Task",
         "Resource": "arn:aws:states:us-east-1:123456789012:activity:get-greeting",
         "End": true
       }
     }
   }
   ```

   This is a description of your state machine using the Amazon States Language\. It defines a single `Task` state named `getGreeting`\. For more information, see [State Machine Structure](amazon-states-language-state-machine-structure.md)\.

1. Use the graph in the **Visual Workflow** pane to check that your Amazon States Language code describes your state machine correctly\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-state-machine-custom-preview.png)

   If you don't see the graph, choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\.

1. Choose **Create State Machine**\.

   The state machine is created and an acknowledgement page is displayed\.

## Step 3: Implementing a Worker<a name="create-activity-state-machine-step-3"></a>

Create a *worker*, a program which is responsible for the following:
+ Polling Step Functions for activities using the `GetActivityTask` API action\.
+ Performing the work of the activity using your code, \(for example, the `getGreeting()` method in the code below\)\.
+ Returning the results using the `SendTaskSuccess`, `SendTaskFailure`, and `SendTaskHeartbeat` API actions\.

**Note**  
For a more complete example of an activity worker, see [Example Activity Worker in Ruby](example-ruby-activity-worker.md)\. This example provides an implementation based on best practices, that can be used as a reference for your activity worker\. The code implements a consumer\-producer pattern with a configurable number of threads for pollers and activity workers\. 

### To implement the worker<a name="create-activity-state-machine-implement-worker"></a>

1. Create a new file named `GreeterActivities.java`\.

1. Add the following code to it:

   ```
   import com.amazonaws.ClientConfiguration;
   import com.amazonaws.auth.EnvironmentVariableCredentialsProvider;
   import com.amazonaws.regions.Regions;
   import com.amazonaws.services.stepfunctions.AWSStepFunctions;
   import com.amazonaws.services.stepfunctions.AWSStepFunctionsClientBuilder;
   import com.amazonaws.services.stepfunctions.model.GetActivityTaskRequest;
   import com.amazonaws.services.stepfunctions.model.GetActivityTaskResult;
   import com.amazonaws.services.stepfunctions.model.SendTaskFailureRequest;
   import com.amazonaws.services.stepfunctions.model.SendTaskSuccessRequest;
   import com.amazonaws.util.json.Jackson;
   import com.fasterxml.jackson.databind.JsonNode;
   import java.util.concurrent.TimeUnit;
   
   
   public class GreeterActivities {
   
       public String getGreeting(String who) throws Exception {
           return "{\"Hello\": \"" + who + "\"}";
       }
   
       public static void main(final String[] args) throws Exception {
           GreeterActivities greeterActivities = new GreeterActivities();
           ClientConfiguration clientConfiguration = new ClientConfiguration();
           clientConfiguration.setSocketTimeout((int)TimeUnit.SECONDS.toMillis(70));
   
           AWSStepFunctions client = AWSStepFunctionsClientBuilder.standard()
                   .withRegion(Regions.US_EAST_1)
                   .withCredentials(new EnvironmentVariableCredentialsProvider())
                   .withClientConfiguration(clientConfiguration)
                   .build();
   
           while (true) {
               GetActivityTaskResult getActivityTaskResult =
                       client.getActivityTask(
                               new GetActivityTaskRequest().withActivityArn(ACTIVITY_ARN));
   
               if (getActivityTaskResult.getTaskToken() != null) {
                   try {
                       JsonNode json = Jackson.jsonNodeOf(getActivityTaskResult.getInput());
                       String greetingResult =
                               greeterActivities.getGreeting(json.get("who").textValue());
                       client.sendTaskSuccess(
                               new SendTaskSuccessRequest().withOutput(
                                       greetingResult).withTaskToken(getActivityTaskResult.getTaskToken()));
                   } catch (Exception e) {
                       client.sendTaskFailure(new SendTaskFailureRequest().withTaskToken(
                               getActivityTaskResult.getTaskToken()));
                   }
               } else {
                   Thread.sleep(1000);
               }
           }
       }
   }
   ```
**Note**  
The `EnvironmentVariableCredentialsProvider` class in this example assumes that the `AWS_ACCESS_KEY_ID` \(or `AWS_ACCESS_KEY`\) and `AWS_SECRET_KEY` \(or `AWS_SECRET_ACCESS_KEY`\) environment variables are set\. For more information about providing the required credentials to the factory, see [AWSCredentialsProvider](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/auth/AWSCredentialsProvider.html) in the *AWS SDK for Java API Reference* and [Set up AWS Credentials and Region for Development](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html) in the *AWS SDK for Java Developer Guide*\.  
To give Step Functions sufficient time to process the request, `setSocketTimeout` is set to 70 seconds\.

1. In the parameter list of the `GetActivityTaskRequest().withActivityArn()` constructor, replace the `ACTIVITY_ARN` value with the ARN of [the activity task that you created earlier](#create-activity-state-machine-new-activity)\.

## Step 4: Starting an Execution<a name="create-activity-state-machine-step-4"></a>

When you start the execution of the state machine, your worker polls Step Functions for activities, performs its work \(using the input that you provide\), and returns its results\.

### To start the execution<a name="create-activity-state-machine-start-execution"></a>

1. On the ***ActivityStateMachine*** page, choose **Start execution**\.

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

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. In the **Execution Details** section, choose **Info** to view the **Execution Status** and the **Started** and **Closed** timestamps\.

1. In the **Execution Details** section, expand the **Output** section to view the output of your workflow\.

## Step 5: Running and Stopping the Worker<a name="create-activity-state-machine-step-5"></a>

To have the worker poll your state machine for activities, you must run the worker\.

**Note**  
After the execution completes, you should stop your worker\. If you don't stop the worker, it will continue to run and poll for activities\. When the execution is stopped, your worker has no source of tasks and generates a `SocketTimeoutException` during each poll\.

### To run and stop the worker<a name="create-activity-state-machine-run-stop-worker"></a>

1. On the command line, navigate to the directory in which you created `GreeterActivities.java`\.

1. To use the AWS SDK, add the full path of the `lib` and `third-party` directories to the dependencies of your build file and to your Java `CLASSPATH`\. For more information, see [Downloading and Extracting the SDK](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-install.html#download-and-extract-sdk) in the *AWS SDK for Java Developer Guide*\.

1. Compile the file:

   ```
   $ javac GreeterActivities.java
   ```

1. Run the file:

   ```
   $ java GreeterActivities
   ```

1. In the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/), navigate to the **Execution Details** page\.

1. When the execution completes, choose **Output** to see the results of your execution\.

1. Stop the worker\.