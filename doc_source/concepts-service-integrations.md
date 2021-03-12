# Service Integrations with AWS Step Functions<a name="concepts-service-integrations"></a>

AWS Step Functions integrates with some AWS services so that you can call API actions, and coordinate executions directly from the Amazon States Language in Step Functions\. You can directly call and pass parameters to the APIs of those services\. 

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

Standard Workflows and Express Workflows support the same set of service integrations but do not support the same integration patterns\. Express Workflows do not support Run a Job \(\.sync\) or Wait for Callback \(\.waitForTaskToken\)\. For more information, see [Standard vs\. Express Workflows](concepts-standard-vs-express.md)\.

------
#### [ Standard Workflows ]


**Supported Service Integrations**  

| Service | [Request Response](connect-to-resource.md#connect-default) | [Run a Job \(`.sync`\)](connect-to-resource.md#connect-sync) | [Wait for Callback \(`.waitForTaskToken`\)](connect-to-resource.md#connect-wait-token) | 
| --- | --- | --- | --- | 
| [Lambda](connect-lambda.md) | ✓ |  | ✓ | 
| [AWS Batch](connect-batch.md) | ✓ | ✓ |  | 
| [DynamoDB](connect-ddb.md) | ✓ |  |  | 
| [Amazon ECS/AWS Fargate](connect-ecs.md) | ✓ | ✓ | ✓ | 
| [Amazon SNS](connect-sns.md) | ✓ |  | ✓ | 
| [Amazon SQS](connect-sqs.md) | ✓ |  | ✓ | 
| [AWS Glue](connect-glue.md) | ✓ | ✓ |  | 
| [SageMaker](connect-sagemaker.md) | ✓ | ✓ |  | 
| [Amazon EMR](connect-emr.md) | ✓ | ✓ |  | 
| [CodeBuild](connect-codebuild.md) | ✓ | ✓ |  | 
| [Athena](connect-athena.md) | ✓ | ✓ |  | 
| [Amazon EKS](connect-eks.md) | ✓ | ✓ |  | 
| [API Gateway](connect-api-gateway.md) | ✓ |  | ✓ | 
| [AWS Glue DataBrew](connect-databrew.md) | ✓ | ✓ |  | 
| [AWS Step Functions](connect-stepfunctions.md) | ✓ | ✓ | ✓ | 

------
#### [ Express Workflows ]


**Supported Service Integrations**  

| Service | [Request Response](connect-to-resource.md#connect-default) | [Run a Job \(`.sync`\)](connect-to-resource.md#connect-sync) | [Wait for Callback \(`.waitForTaskToken`\)](connect-to-resource.md#connect-wait-token) | 
| --- | --- | --- | --- | 
| [Lambda](connect-lambda.md) | ✓ |  |  | 
| [AWS Batch](connect-batch.md) | ✓ |  |  | 
| [DynamoDB](connect-ddb.md) | ✓ |  |  | 
| [Amazon ECS/AWS Fargate](connect-ecs.md) | ✓ |  |  | 
| [Amazon SNS](connect-sns.md) | ✓ |  |  | 
| [Amazon SQS](connect-sqs.md) | ✓ |  |  | 
| [AWS Glue](connect-glue.md) | ✓ |  |  | 
| [SageMaker](connect-sagemaker.md) | ✓ |  |  | 
| [Amazon EMR](connect-emr.md) | ✓ |  |  | 
| [CodeBuild](connect-codebuild.md) | ✓ |  |  | 
| [Athena](connect-athena.md) | ✓ |  |  | 
| [Amazon EKS](connect-eks.md) | ✓ |  |  | 
| [API Gateway](connect-api-gateway.md) | ✓ |  |  | 
| [AWS Glue DataBrew](connect-databrew.md) | ✓ |  |  | 
| [AWS Step Functions](connect-stepfunctions.md) | ✓ |  |  | 

------

**Topics**
+ [Service Integration Patterns](connect-to-resource.md)
+ [Pass Parameters to a Service API](connect-parameters.md)
+ [Code Snippets](concepts-code-snippets.md)
+ [Supported AWS Services](connect-supported-services.md)