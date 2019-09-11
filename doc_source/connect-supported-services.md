# Supported AWS Service Integrations for Step Functions<a name="connect-supported-services"></a>

The following topics include the supported APIs, parameters, and request/response syntax in the Amazon States Language for coordinating other AWS services, and provide example code\. You can call integrated services directly from the Amazon States Language in the `Resource` field of a task state\. There are three different service integration patterns you can use\.
+ [Default response](connect-to-resource.md#connect-default)
+ [Wait for a job to complete \(`.sync`\)\.](connect-to-resource.md#connect-sync)
+ [Wait for a task token \(`.waitForTaskToken`\)\.](connect-to-resource.md#connect-wait-token)

For more information on service integrations, see [Service Integration Patterns](connect-to-resource.md)\.


**Supported Service Integrations**  

| Service | [Request Response](connect-to-resource.md#connect-default) | [Run a Job \(`.sync`\)](connect-to-resource.md#connect-sync) | [Wait for Callback \(`.waitForTaskToken`\)](connect-to-resource.md#connect-wait-token) | 
| --- | --- | --- | --- | 
| [Lambda](connect-lambda.md) | ✓ |  | ✓ | 
| [AWS Batch](connect-batch.md) | ✓ | ✓ | ✓ | 
| [DynamoDB](connect-ddb.md) | ✓ |  |  | 
| [Amazon ECS/Fargate](connect-ecs.md) | ✓ | ✓ | ✓ | 
| [Amazon SNS](connect-sns.md) | ✓ |  | ✓ | 
| [Amazon SQS](connect-sqs.md) | ✓ |  | ✓ | 
| [AWS Glue](connect-glue.md) | ✓ | ✓ |  | 
| [Amazon SageMaker](connect-sagemaker.md) | ✓ | ✓ |  | 
| [Step Functions](connect-stepfunctions.md) | ✓ | ✓ | ✓ | 