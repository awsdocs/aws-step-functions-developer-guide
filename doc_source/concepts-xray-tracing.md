# AWS X\-Ray and Step Functions<a name="concepts-xray-tracing"></a>

You can use [AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) to visualize the components of your state machine, identify performance bottlenecks, and troubleshoot requests that resulted in an error\. Your state machine sends trace data to X\-Ray, and X\-Ray processes the data to generate a service map and searchable trace summaries\.

With X\-Ray enabled for your state machine, you can trace requests as they are executed in Step Functions, in all AWS Regions where X\-Ray is available\. This gives you a detailed overview of an entire Step Functions request\. Step Functions will send traces to X\-Ray for state machine executions, even when a trace ID is not passed by an upstream service\. You can use an X\-Ray service map to view the latency of a request, including any AWS services that are integrated with X\-Ray\. You can also configure sampling rules to tell X\-Ray which requests to record, and at what sampling rates, according to criteria that you specify\.

When X\-Ray is not enabled for your state machine, and an upstream service does not pass a trace ID, Step Functions will not send traces to X\-Ray for state machine executions\. However, if a trace ID is passed by an upstream service, Step Functions will then send traces to X\-Ray for state machine executions\.

You can use AWS X\-Ray with Step Functions in regions where both are supported\. See the [Step Functions](https://docs.aws.amazon.com/general/latest/gr/step-functions.html) and [X\-Ray](https://docs.aws.amazon.com/general/latest/gr/xray.html) endpoints and quotas pages for information on region support for X\-Ray and Step Functions\. 

**X\-Ray and Step Functions Combined Quotas**  
You can add data to a trace for up to seven days, and query trace data going back thirty days, the length of time that X\-Ray stores trace data\. Your traces will be subject to X\-Ray quotas\. In addition to other quotas, X\-Ray provides a minimum guaranteed trace size of 100KB for Step Functions state machines\. If more than 100KB of trace data is provided to X\-Ray, this may result in a frozen trace\. See the service quotas section of the [X\-Ray endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/xray.html#limits_xray) page for more information on other quotas for X\-Ray\.

**Topics**
+ [Setup and configuration](#xray-concept-create)
+ [Concepts](#xray-concepts)
+ [Service integrations](#xray-concept-integrations)
+ [Viewing the X\-Ray console](#xray-concept-tracing-details)
+ [Viewing X\-Ray tracing information for Step Functions](#concepts-xray-tracing-events)
+ [Traces](#concepts-xray-traces)
+ [Service map](#concepts-xray-tracing-overview)
+ [Segments and subsegments](#concepts-xray-tracing-segments)
+ [Analytics](#concepts-xray-tracing-analytics)
+ [Configuration](#concepts-xray-tracing-config)
+ [What if there is no data in the trace map or service map?](#concepts-xray-troubleshooting)

## Setup and configuration<a name="xray-concept-create"></a>

### Enable X\-Ray tracing when creating a state machine<a name="xray-concept-create-new"></a>

You can enable X\-Ray tracing when creating a new state machine by selecting **Enable X\-Ray tracing** on the **Specify details** page\.

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Create state machine**\.

1. On the **Choose authoring method** page, choose an appropriate option to create your state machine\. If you choose **Run a sample project**, you cannot enable X\-Ray tracing during the state machine creation, and you will need to enable X\-Ray tracing after your state machine has been created\. For more information about enabling X\-Ray in an existing state machine, see [Enable X\-Ray in an existing state machine](#xray-concept-enable-existing)\.

   Choose **Next**\.

1. On the **Specify details** page, configure your state machine\.

1. Choose **Enable X\-Ray tracing**\.   
![\[X-Ray enable\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-enable.png)

    Your Step Functions state machine will now send traces to X\-Ray for state machine executions\.
**Note**  
If you choose to use an existing IAM role, you should ensure that X\-Ray writes are allowed\.

### Enable X\-Ray in an existing state machine<a name="xray-concept-enable-existing"></a>

To enable X\-Ray in an existing state machine:

1. In the [Step Functions console](https://console.aws.amazon.com/states/home), select the state machine for which you want to enable tracing\.

1. Choose **Edit**\.

1. Choose **Enable X\-Ray tracing**\.  
![\[X-Ray enable\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-enable.png)

    You will see a notification telling you that you that you may need to make additional changes\. 
**Note**  
When you enable X\-Ray for an existing state machine, you must ensure that you have an IAM policy that grants sufficient permissions for X\-Ray to perform traces\. You can either add one manually, or generate one\. For more information, see the IAM policy section for [X\-Ray](xray-iam.md)\. 

1. \(Optional\) Auto\-generate a new role for your state machine to include X\-Ray permissions\.

1. Choose **Save**\. 

### Configure X\-Ray tracing for Step Functions<a name="xray-concept-configure"></a>

When you first run a state machine with X\-Ray tracing enabled, it will use the default configuration values for X\-Ray tracing\. AWS X\-Ray does not collect data for every request that is sent to an application\. Instead, it collects data for a statistically significant number of requests\. The default is to record the first request each second, and five percent of any additional requests\. One request per second is the *reservoir*\. This ensures that at least one trace is recorded each second as long as the service is serving requests\. Five percent is the *rate* at which additional requests beyond the reservoir size are sampled\.

To avoid incurring service charges when you are getting started, the default sampling rate is conservative\. You can configure X\-Ray to modify the default sampling rule and configure additional rules that apply sampling based on properties of the service or request\.

For example, you might want to disable sampling and trace all requests for calls that modify state or handle user accounts or transactions\. For high\-volume read\-only calls, like background polling, health checks, or connection maintenance, you can sample at a low rate and still get enough data to observe issues that occur\.

To configure a sampling rule for your state machine:

1. Go to the [X\-Ray console](https://console.aws.amazon.com/xray/home)\.

1. Choose **Sampling**\.

1. To create a rule, choose **Create sampling rule**\.

   To edit a rule, choose a rule's name\.

   To delete a rule, choose a rule and use the **Actions** menu to delete it\.

Some parts of existing sampling rules, such as the name and priority, cannot be changed\. Instead, add or clone an existing rule, make the changes you want, then use the new rule\.

For detailed information on X\-Ray sampling rules and how to configure the various parameters, see [Configuring sampling rules in the X\-Ray console](https://docs.aws.amazon.com/xray/latest/devguide/xray-console-sampling.html)\. 

## Concepts<a name="xray-concepts"></a>

### The X\-Ray console<a name="xray-concepts-console"></a>

The AWS X\-Ray console enables you to view service maps and traces for requests that your applications serve\. You can access the console to view detailed information collected by X\-Ray when it's enabled for your state machine\.

See [Viewing the X\-Ray console](#xray-concept-tracing-details) for information on how to access the X\-Ray console for your state machine executions\.

For detailed information about the X\-Ray console, see the [X\-Ray console documentation](https://docs.aws.amazon.com/xray/latest/devguide/xray-console.html)\. 

### Segments, subsegments, and traces<a name="xray-concepts-traces"></a>

A **segment** records information about a request to your state machine\. It contains information such as the work that your state machine performs, and may also contain **subsegments** with information about downstream calls\.

A **trace** collects all the segments generated by a single request\.

### Sampling<a name="xray-concepts-sampling"></a>

To ensure efficient tracing and provide a representative sample of the requests that your application serves, X\-Ray applies a **sampling** algorithm to determine which requests get traced\. This can be changed by editing the sampling rules\.

### Metrics<a name="xray-concepts-metrics"></a>

For your state machine, X\-Ray will meter invocation time, state transition time, the overall execution time of Step Functions, and variances in this execution time\. This information can be accessed through the X\-Ray console\.

### Analytics<a name="xray-concepts-analyzing"></a>

The AWS X\-Ray Analytics console is an interactive tool for interpreting trace data\. You can refine the active dataset with increasingly granular filters by clicking the graphs and the panels of metrics and fields that are associated with the current trace set\. This lets you analyze how your state machine is performing, and quickly locate and identify performance issues\. 

For detailed information about X\-Ray analytics, see [Interacting with the AWS X\-Ray Analytics console](https://docs.aws.amazon.com/xray/latest/devguide/xray-console-analytics.html)

## Step Functions service integrations and X\-Ray<a name="xray-concept-integrations"></a>

 Some of the AWS services that integrate with Step Functions provide integration with AWS X\-Ray by adding a tracing header to requests, running the X\-Ray daemon, or making sampling decisions and uploading trace data to X\-Ray\. Others must be instrumented using the AWS X\-Ray SDK\. A few do not yet support X\-Ray integration\. X\-Ray integration is necessary to provide complete trace data when using a service integration with Step Functions  

### Native X\-Ray support<a name="xray-concept-integrations-native"></a>

Service integrations with native X\-Ray support include:
+ [Amazon Simple Notification Service](https://docs.aws.amazon.com/xray/latest/devguide/xray-services-sns.html)
+ [Amazon Simple Queue Service](https://docs.aws.amazon.com/xray/latest/devguide/xray-services-sqs.html)
+ [AWS Lambda](https://docs.aws.amazon.com/xray/latest/devguide/xray-services-lambda.html)
+ AWS Step Functions

### Instrumentation required<a name="xray-concept-integrations-instrument"></a>

Service integrations that require [X\-Ray instrumentation](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html):
+ Amazon Elastic Container Service
+ AWS Batch
+ AWS Fargate

### Client\-side trace only<a name="xray-concept-integrations-unsupported"></a>

Other service integrations do not support X\-Ray traces\. However, client side traces can still be collected:
+ Amazon DynamoDB
+ Amazon EMR
+ Amazon SageMaker
+ AWS CodeBuild
+ AWS Glue

## Viewing the X\-Ray console<a name="xray-concept-tracing-details"></a>

X\-Ray receives data from services as segments\. X\-Ray groups segments that have a common request into traces\. X\-Ray processes the traces to generate a service graph that provides a visual representation of your application\. 

After you start your state machine's execution, you can view its X\-Ray traces by choosing the **X\-Ray trace map** link in the **Execution details** section\.

![\[X-Ray traces\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-choose.png)

After you have enabled X\-Ray for your state machine, you can view tracing information for its executions in the X\-Ray console\. 

## Viewing X\-Ray tracing information for Step Functions<a name="concepts-xray-tracing-events"></a>

The following steps illustrate what kind of information you can see in the console after you enable X\-Ray and run an execution\. X\-Ray traces for the [Callback Pattern Example \(Amazon SQS, Amazon SNS, Lambda\) ](callback-task-sample-sqs.md) sample project are shown\. 

## Traces<a name="concepts-xray-traces"></a>

After the an execution has finished, you can navigate to the X\-Ray console, where you will see the X\-Ray **Traces** page\. This displays an overview of the service map as well as trace and segment information for your state machine\. 

![\[X-Ray traces\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-overview.png)

## Service map<a name="concepts-xray-tracing-overview"></a>

The service map in the X\-Ray console helps you to identify services where errors are occurring, where there are connections with high latency, or see traces for requests that were unsuccessful\. 

![\[X-Ray traces\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-servicemap.png)

On the trace map, you can choose a service node to view requests for that node, or an edge between two nodes to view requests that traveled that connection\. Here, the `WaitForCallBack` node has been selected, and you can view additional information about its execution and response status\.

![\[X-Ray traces\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-servicemap-detail.png)

You can see how the X\-Ray service map correlates to the state machine\. There is a service map node for each service integration that is called by Step Functions, provided it supports X\-Ray\. 

![\[X-Ray traces\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-callback-example.png)

## Segments and subsegments<a name="concepts-xray-tracing-segments"></a>

A **trace** is a collection of **segments** generated by a single request\. Each segment provides the resource's name, details about the request, and details about the work done\. On the **Traces** page, you can see the segments and, if expanded, its corresponding subsegments\. You can choose a segment or subsegment to view detailed information about it\.

Choose each of the tabs to see how information for segments and subsegments is displayed\.

------
#### [ Overview of Segments ]

An overview of segments and subsegments for this state machine\. There is a different segment for each node on the service map\.

![\[Segments\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-segments.png)

------
#### [ View segment detail ]

Choosing a segment provides the resource's name, details about the request, and details about the work done\. 

![\[Segment detail\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-segments-detail.png)

------
#### [ View subsegment detail ]

A segment can break down the data about the work done into subsegments\. Choosing a subsegments lets you view more granular timing information and details\. A subsegment can contain additional details about a call to an AWS service, an external HTTP API, or an SQL database\.

![\[Subsegment detail\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-subsegments-detail.png)

------

## Analytics<a name="concepts-xray-tracing-analytics"></a>



The AWS X\-Ray **Analytics** console is an interactive tool for interpreting trace data\. You can use this to more easily understand how your state machine is performing\. The console enables you to explore, analyze, and visualize traces through interactive response time and time\-series graphs\. This can help you quickly locate performance and latency issues\.

 You can refine the active dataset with increasingly granular filters by clicking the graphs and the panels of metrics and fields that are associated with the current trace set\.

![\[Analytics\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-analytics.png)

## Configuration<a name="concepts-xray-tracing-config"></a>

You can configure sampling and encryption options from the X\-Ray console\.

------
#### [ Sampling ]

Choose **Sampling** to view details about the sampling rate and configuration\. You can change the sampling rules to control the amount of data that you record, and modify sampling behavior to suit your specific requirements\.

![\[Sampling\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-sampling.png)

------
#### [ Encryption ]

Choose **Encryption** to modify the encryption settings\. You can use the default setting, where X\-Ray encrypts traces and date at rest, or, if needed, you can choose a customer master key\. Standard [AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/) charges apply in the latter case\.

![\[Encryption settings\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/xray-tracing-encryption.png)

------

## What if there is no data in the trace map or service map?<a name="concepts-xray-troubleshooting"></a>

If you have enabled X\-Ray, but can't see any data in the X\-Ray console, check that:
+ Your IAM roles are set up correctly to allow writing to X\-Ray\.
+ Sampling rules allow sampling of data\.
+ Since there can be a short delay before newly created or modified IAM roles are applied, check the trace or service maps again after a few minutes\.
+ If you see **Data Not Found** in the X\-Ray Traces panel, check your [ IAM account settings](https://console.aws.amazon.com/iam/home?#/account_settings) and ensure that AWS Security Token Service is enabled for the intended region\. For more information, see [the IAM user guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html#sts-regions-activate-deactivate)\.