# AWS Service Integrations<a name="concepts-service-integrations"></a>

AWS Step Functions integrates with some AWS services so that you can call API actions, and coordinate executions directly from the Amazon States Language in Step Functions\. You can directly call and pass parameters to the API of those services\. You coordinate these services directly from a task state in the Amazon States Language\. For example, using Step Functions, you can call other services to: 
+ Invoke an AWS Lambda function\.
+ Run an AWS Batch job and then perform different actions based on the results\.
+ Insert or get an item from Amazon DynamoDB\.
+ Run an Amazon Elastic Container Service \(Amazon ECS\) task and wait for it to complete\.
+ Publish to a topic in Amazon Simple Notification Service \(Amazon SNS\)\.
+ Send a message in Amazon Simple Queue Service \(Amazon SQS\)\.
+ Manage a job for AWS Glue or Amazon SageMaker\.
+ Launch an AWS Step Functions workflow execution\.


**Supported Service Integrations**  

| Service | [Request Response](connect-to-resource.md#connect-default) | [Run a Job \(`.sync`\)](connect-to-resource.md#connect-sync) | [Wait for Callback \(`.waitForTaskToken`\)](connect-to-resource.md#connect-wait-token) | 
| --- | --- | --- | --- | 
| [AWS Lambda](connect-lambda.md) | ✓ |  | ✓ | 
| [AWS Batch](connect-batch.md) | ✓ | ✓ | ✓ | 
| [Amazon DynamoDB](connect-ddb.md) | ✓ |  |  | 
| [Amazon ECS/Fargate](connect-ecs.md) | ✓ | ✓ | ✓ | 
| [Amazon Simple Notification Service](connect-sns.md) | ✓ |  | ✓ | 
| [Amazon Simple Queue Service](connect-sqs.md) | ✓ |  | ✓ | 
| [AWS Glue](connect-glue.md) | ✓ | ✓ |  | 
| [Amazon SageMaker](connect-sagemaker.md) | ✓ | ✓ |  | 
| [AWS Step Functions](connect-stepfunctions.md) | ✓ | ✓ | ✓ | 

**Topics**
+ [Service Integration Patterns](connect-to-resource.md)
+ [Pass Parameters to a Service API](connect-parameters.md)
+ [Code Snippets](concepts-code-snippets.md)
+ [Supported AWS Services](connect-supported-services.md)