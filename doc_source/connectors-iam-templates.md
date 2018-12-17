# IAM Policies for Integrated Services<a name="connectors-iam-templates"></a>

When you create a state machine in the AWS Step Functions console, Step Functions will produce an IAM policy based on the resources used in your state machine definition\. 

These examples show how Step Functions generates an IAM policy based on your state machine definition\. Items in the example code such as *`[[resourceName]]`* are replaced with the static resources listed in your state machine definition\. If you have multiple static resources, there will be an entry for each in the IAM role\.

## Dynamic vs\. Static Resources<a name="connectors-iam-dynamic-static"></a>

Static resources are defined directly in the task state of your state machine\. When you include the information about the API actions you call directly in your task states, Step Functions creates an IAM role for only those resources\. 

Dynamic resources are those that are passed in to your state input, and accessed using a Path \(see, [Paths](amazon-states-language-input-output-processing.md#amazon-states-language-paths)\)\. If you are passing dynamic resources to your task, Step Functions will create a more privileged policy that specifies: `"Resource": "*"`\.

## Synchronous vs\. Asynchronous IAM Policies<a name="connectors-iam-sync-async"></a>

For synchronous connections \(those ending in `.sync`\), additional permissions are needed to monitor and receive a response from the API actions of connected services\. The related policies need more permissions than non\-synchronous connected services\. See [Connect to Resources](connectors-resource.md) for information on synchronous connections\.

**Note**  
Review these templates to understand how Step Functions creates your IAM policies, and as an example of how to manually create IAM policies for Step Functions when working with other AWS services\. For more information on Step Functions service integration, see: [AWS Service Integrations](concepts-connectors.md)

**Topics**
+ [Dynamic vs\. Static Resources](#connectors-iam-dynamic-static)
+ [Synchronous vs\. Asynchronous IAM Policies](#connectors-iam-sync-async)
+ [Lambda](lambda-iam.md)
+ [AWS Batch](batch-iam.md)
+ [DynamoDB](dynamo-iam.md)
+ [Amazon ECS/Fargate](ecs-iam.md)
+ [Amazon SNS](sns-iam.md)
+ [Amazon SQS](sqs-iam.md)
+ [AWS Glue](glue-iam.md)
+ [Amazon SageMaker](sagemaker-iam.md)
+ [Activities](activities-iam.md)