# AWS Service Integrations<a name="concepts-connectors"></a>

AWS Step Functions integrates with some AWS services, including the ability to directly call and pass parameters to the API of those services\. You can use Step Functions to coordinate these services directly from the Amazon States Language\. For example, using Step Functions, you can coordinate other services to: 
+ Invoke an AWS Lambda function\.
+ Run an AWS Batch job and then perform different actions based on the results\.
+ Insert or get an item from Amazon DynamoDB\.
+ Run an Amazon Elastic Container Service \(Amazon ECS\) task and wait for it to complete\.
+ Publish a topic in Amazon Simple Notification Service \(Amazon SNS\)\.
+ Send a message in Amazon Simple Queue Service \(Amazon SQS\)\.
+ Manage a job for AWS Glue or Amazon SageMaker\.

For working examples of state machines that control other AWS services, see [Sample Projects](create-sample-projects.md)\. These sample projects will deploy a state machine and all related resources\.

**Topics**
+ [Connect to Resources](connectors-resource.md)
+ [Pass Parameters to a Service API](connectors-parameters.md)
+ [Code Snippets](concepts-code-snippets.md)
+ [Supported AWS Services](connectors-supported-services.md)