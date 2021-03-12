# Supported AWS Service Integrations for Step Functions<a name="connect-supported-services"></a>

The following topics include the supported APIs, parameters, and request/response syntax in the Amazon States Language for coordinating other AWS services\. The topics also provide example code\. You can call integrated services directly from the Amazon States Language in the `Resource` field of a task state\. 

You can use three service integration patterns:
+ [Default response](connect-to-resource.md#connect-default)
+ [Wait for a job to complete \(`.sync`\)\.](connect-to-resource.md#connect-sync)
+ [Wait for a task token \(`.waitForTaskToken`\)\.](connect-to-resource.md#connect-wait-token)

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