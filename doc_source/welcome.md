# What is AWS Step Functions?<a name="welcome"></a>

Step Functions is a serverless orchestration service that lets you combine [AWS Lambda](https://aws.amazon.com/lambda/) functions and other AWS services to build business\-critical applications\. Through Step Functions' graphical console, you see your application’s workflow as a series of event\-driven steps\.

Step Functions is based on state machines and tasks\. A state machine is a workflow\. A task is a state in a workflow that represents a single unit of work that another AWS service performs\. Each step in a workflow is a state\.

With Step Functions' built\-in controls, you examine the state of each step in your workflow to make sure that your application runs in order and as expected\. Depending on your use case, you can have Step Functions call AWS services, such as Lambda, to perform tasks\. You can create workflows that process and publish machine learning models\. You can have Step Functions control AWS services, such as [AWS Glue](https://aws.amazon.com/glue/), to create extract, transform, and load \(ETL\) workflows\. You also can create long\-running, automated workflows for applications that require human interaction\. 

**Topics**
+ [AWS SDK and Optimized integrations](#optimized-integrations)
+ [Standard and Express workflows](#welcome-workflows)
+ [Use cases](#application)
+ [Service integrations](#welcome-integrations)
+ [Supported regions](#supported-regions)
+ [Is this your first time using Step Functions?](#welcome-overview)

## AWS SDK and Optimized integrations<a name="optimized-integrations"></a>

To call other AWS services, you can use Step Functions's AWS SDK integrations, or you can use one of Step Functions's Optimized integrations\.
+ The [AWS SDK integrations](supported-services-awssdk.md) let you call any of the over two hundred AWS services directly from your state machine, giving you access to over nine thousand API actions\.
+ [Step Functions's Optimized integrations](connect-supported-services.md) have been customized to simplify usage in your state machines\.

## Standard and Express workflows<a name="welcome-workflows"></a>

Step Functions has two workflow types\. Standard workflows have exactly\-once workflow execution and can run for up to one year\. Express workflows have at\-least\-once workflow execution and can run for up to five minutes\. Executions are instances where you run your workflow to perform tasks\. Standard workflows are ideal for long\-running, auditable workflows, as they show execution history and visual debugging\. Express workflows are ideal for high\-event\-rate workloads, such as streaming data processing and IoT data ingestion\.

### Standard workflows<a name="welcome-standard"></a>
+ 2,000 per second execution rate
+ 4,000 per second state transition rate
+ Priced per state transition
+ Shows execution history and visual debugging
+ Supports all service integrations and patterns

### Express workflows<a name="welcome-express"></a>
+ 100,000 per second execution rate
+ Nearly unlimited state transition rate
+ Priced per number and duration of executions
+ Sends execution history to [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)
+ Supports all service integrations and most patterns

For more information about Standard and Express workflows, including Step Functions pricing, see the following:
+ [Standard vs\. Express Workflows](concepts-standard-vs-express.md)
+ [AWS Step Functions pricing](https://aws.amazon.com/step-functions/pricing/)

## Use cases<a name="application"></a>

Step Functions manages your application's components and logic, so you can write less code and focus on building and updating your application quickly\. This section describes typical use cases for working with Step Functions\.

### Use case \#1: Function orchestration<a name="welcome-function"></a>

![\[Chaining\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/chaining.png)

You create a workflow that runs a group of Lambda functions \(steps\) in a specific order\. One Lambda function's output passes to the next Lambda function's input\. The last step in your workflow gives a result\. With Step Functions, you can see how each step in your workflow interacts with one other, so you can make sure that each step performs its intended function\.

For a tutorial that shows you how to create a state machine with a group of functions, see the following:
+ [Getting started with AWS Step Functions](getting-started.md)

### Use case \#2: Branching<a name="welcome-branching"></a>

![\[Branching\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/branching.png)

A customer requests a credit limit increase\. Using a `Choice` state, you can have Step Functions make decisions based on the `Choice` state’s input\. If the request is more than your customer’s pre\-approved credit limit, you can have Step Functions send your customer's request to a manager for sign\-off\. If the request is less than your customer’s pre\-approved credit limit, you can have Step Functions approve the request automatically\.

### Use case \#3: Error handling<a name="welcome-error"></a>

![\[Retry/Catch\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/retry.png)

**`Retry`**

In this use case, a customer requests a username\. The first time, your customer’s request is unsuccessful\. Using a `Retry` statement, you can have Step Functions try your customer's request again\. The second time, your customer’s request is successful\.

**`Catch`**

In a similar use case, a customer requests an unavailable username\. Using a `Catch` statement, you have Step Functions suggest an available username\. If your customer takes the available username, you can have Step Functions go to the next step in your workflow, which is to send a confirmation email\. If your customer doesn’t take the available username, you have Step Functions go to a different step in your workflow, which is to start the sign\-up process over\.

For more detailed examples of `Retry` and `Catch` statements, see the following:
+ [Error handling in Step Functions](concepts-error-handling.md)

### Use case \#4: Human in the loop<a name="welcome-human"></a>

![\[Chaining\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/human-in-the-loop.png)

Using a banking app, one of your customers sends money to a friend\. Your customer waits for a confirmation email\. With [a callback and a task token](connect-to-resource.md#connect-wait-token), you have Step Functions tell Lambda to send your customer’s money and report back when your customer’s friend receives it\. After Lambda reports back that your customer’s friend received the money, you can have Step Functions go to the next step in your workflow, which is to send your customer a confirmation email\.

To see a sample project that shows a callback with a task token, see the following:
+ [Callback Pattern Example \(Amazon SQS, Amazon SNS, Lambda\) ](callback-task-sample-sqs.md) 

### Use case \#5: Parallel processing<a name="welcome-parallel-processing"></a>

![\[Parallelism\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/parallelism.png)

A customer converts a video file into five different display resolutions, so viewers can watch the video on multiple devices\. Using a `Parallel` state, Step Functions inputs the video file, so Lambda can process it into the five display resolutions at the same time\.

### Use case \#6: Dynamic parallelism<a name="welcome-dyn-parallelism"></a>

![\[Chaining\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/dynamic-parallelism.png)

A customer orders three items, and you need to prepare each item for delivery\. You check each item's availability, gather each item, and then package each item for delivery\. Using a `Map` state, Step Functions has Lambda process each of your customer's items in parallel\. Once all of your customer's items are packaged for delivery, Step Functions goes to the next step in your workflow, which is to send your customer a confirmation email with tracking information\.

To see a sample project that shows dynamic parallelism using a `Map` state, see the following:
+ [Dynamically process data with a Map state](sample-map-state.md)

## Service integrations<a name="welcome-integrations"></a>

 Step Functions integrates with multiple AWS services\. To combine Step Functions with these services, use the following service integration patterns:

**[Request a response \(default\)](connect-to-resource.md#connect-default)**
+ Call a service, and let Step Functions progress to the next state after it gets an HTTP response\.



**[Run a job \(\.sync\)](connect-to-resource.md#connect-sync)**
+ Call a service, and have Step Functions wait for a job to complete\.



**[Wait for a callback with a task token \(\.waitForTaskToken\)](connect-to-resource.md#connect-wait-token)**
+ Call a service with a task token, and have Step Functions wait until the task token returns with a callback\.

The table below shows the available service integrations and service integration patterns for Step Functions\.

 Standard Workflows and Express Workflows support the same integrations but do not support the same integration patterns\. Express Workflows do not support Run a Job \(\.sync\) or Wait for Callback \(\.waitForTaskToken\)\. Optimized integrations pattern support is different for each integration\. For more information, see [Standard vs\. Express Workflows](concepts-standard-vs-express.md)\.

------
#### [ Standard Workflows ]


**Supported service integrations**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)

------
#### [ Express Workflows ]


**Supported service integrations**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)

------

## Supported regions<a name="supported-regions"></a>

Most AWS regions support Step Functions\. For a complete list of AWS regions where Step Functions is available, see the [AWS Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

## Is this your first time using Step Functions?<a name="welcome-overview"></a>

If this is your first time using Step Functions, the following topics help you understand different parts of working with Step Functions, including how Step Functions combines with other AWS services:
+ [Tutorials for Step Functions](tutorials.md)
+ [Sample projects for Step Functions](create-sample-projects.md)
+ [AWS Step Functions Data Science SDK for Python](concepts-python-sdk.md)
