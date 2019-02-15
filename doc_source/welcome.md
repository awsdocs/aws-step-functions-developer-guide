# What Is AWS Step Functions?<a name="welcome"></a>

AWS Step Functions is a web service that enables you to coordinate the components of distributed applications and microservices using visual workflows\. You build applications from individual components that each perform a discrete function, or *task*, allowing you to scale and change applications quickly\. Step Functions provides a reliable way to coordinate components and step through the functions of your application\. Step Functions provides a graphical console to visualize the components of your application as a series of steps\. It automatically triggers and tracks each step, and retries when there are errors, so your application executes in order and as expected, every time\. Step Functions logs the state of each step, so when things do go wrong, you can diagnose and debug problems quickly\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/Dh7h3lkpeP4?rel=0&amp;controls=0&amp;showinfo=0/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/Dh7h3lkpeP4?rel=0&amp;controls=0&amp;showinfo=0)

Step Functions manages the operations and underlying infrastructure for you to ensure your application is available at any scale\.

You can run your tasks on the AWS Cloud, on your own servers, or on any system that has access to AWS\. Step Functions can be accessed and used with the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/), the AWS SDKs, or an HTTP API\. This guide shows you how to develop, test, and troubleshoot your own state machine using these methods\.

## Overview of Step Functions<a name="overview"></a>

Here are some of the key features of AWS Step Functions:
+ Step Functions is based on the concepts of [tasks](concepts-tasks.md) and [state machines](concepts-states.md)\.
+ You define state machines using the JSON\-based [Amazon States Language](concepts-amazon-states-language.md)\.
+ The [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) displays a graphical view of your state machine's structure, which provides you with a way to visually check your state machine's logic and monitor executions\.

## Supported Regions<a name="supported-regions"></a>

Currently, Step Functions is supported only in the following regions:
+ US East \(Ohio\)
+ US East \(N\. Virginia\)
+ US West \(Oregon\)
+ US West \(N\. California\)
+ Asia Pacific \(Mumbai\)
+ China \(Ningxia\)
+ China \(Beijing\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ EU \(Frankfurt\)
+ EU \(Ireland\)
+ EU \(London\)
+ EU \(Paris\)
+ EU \(Stockholm\)
+ Canada \(Central\)
+ South America \(São Paulo\)
+ AWS GovCloud \(US\-West\)

### About Amazon Web Services<a name="about-amazon-web-services"></a>

 Amazon Web Services \(AWS\) is a collection of digital infrastructure services that developers can leverage when developing their applications\. The services include computing, storage, database, and application synchronization \(messaging and queuing\)\. AWS uses a pay\-as\-you\-go service model: you are charged only for the services that you—or your applications—use\. For new AWS users, a free usage tier is available\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the Free Tier, see [Use the AWS Free Tier](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html)\. To obtain an AWS account, visit the [AWS home page](https://aws.amazon.com/) and choose **Create a Free Account**\.