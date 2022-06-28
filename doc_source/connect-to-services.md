# Call other AWS services<a name="connect-to-services"></a>

AWS Step Functions integrates with AWS services, letting you call each service's API actions from your workflow\. You can use Step Functions' [AWS SDK integrations](supported-services-awssdk.md) to call any of the over two hundred AWS services directly from your state machine, giving you access to over nine thousand API actions\. Or you can use [Step Functions' Optimized integrations](connect-supported-services.md), each of which has been customized to provide special functionality for your workflow\. Some API actions are available in both types of integration\. In this case, it's recommended that you use the Optimized integration\. 

You coordinate these services directly from a `Task` state in the Amazon States Language\. For example, using Step Functions, you can call other services to: 
+ Invoke an AWS Lambda function\.
+ Run an AWS Batch job and then perform different actions based on the results\.
+ Insert or get an item from Amazon DynamoDB\.
+ Run an Amazon Elastic Container Service \(Amazon ECS\) task and wait for it to complete\.
+ Publish to a topic in Amazon Simple Notification Service \(Amazon SNS\)\.
+ Send a message in Amazon Simple Queue Service \(Amazon SQS\)\.
+ Manage a job for AWS Glue or Amazon SageMaker\.
+ Build workflows for executing Amazon EMR jobs\.
+ Launch an AWS Step Functions workflow execution\.

## Optimized integrations<a name="connect-to-services-optimized"></a>

 Optimized integrations have been customized by Step Functions to provide special functionality for a workflow context\. For example, [Lambda `Invoke`](connect-lambda.md) converts its API output from an escaped JSON to a JSON object\. [AWS Batch`SubmitJob`](connect-batch.md) lets you pause execution until the job is complete\. The first set of optimized integrations was released in 2018, and there are now over fifty APIs\. 

## AWS SDK integrations<a name="connect-to-services-awssdk"></a>

 AWS SDK integrations work exactly like a standard API call using the AWS SDK\. They provide the ability to call over nine thousand APIs across the more than two hundred AWS services directly from your state machine definition\. AWS SDK integrations were released in 2021\. 

## Integration pattern support<a name="connect-to-services-integration-patterns"></a>

 Standard Workflows and Express Workflows support the same integrations but do not support the same integration patterns\. Express Workflows do not support Run a Job \(\.sync\) or Wait for Callback \(\.waitForTaskToken\)\. Optimized integrations pattern support is different for each integration\. For more information, see [Standard vs\. Express Workflows](concepts-standard-vs-express.md)\.

------
#### [ Standard Workflows ]


**Supported service integrations**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/connect-to-services.html)

------
#### [ Express Workflows ]


**Supported service integrations**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/connect-to-services.html)

------