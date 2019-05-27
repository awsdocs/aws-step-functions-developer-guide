# AWS Service Integrations<a name="concepts-service-integrations"></a>

AWS Step Functions integrates with some AWS services so that you can call API actions, and coordinate executions directly from the Amazon States Language in Step Functions\. You can directly call and pass parameters to the API of those services\. You coordinate these services directly from a task state in the Amazon States Language\. For example, using Step Functions, you can call other services to: 
+ Invoke an AWS Lambda function\.
+ Run an AWS Batch job and then perform different actions based on the results\.
+ Insert or get an item from Amazon DynamoDB\.
+ Run an Amazon Elastic Container Service \(Amazon ECS\) task and wait for it to complete\.
+ Publish a topic in Amazon Simple Notification Service \(Amazon SNS\)\.
+ Send a message in Amazon Simple Queue Service \(Amazon SQS\)\.
+ Manage a job for AWS Glue or Amazon SageMaker\.
+ Have Step Functions wait for a task to return a specific task token\.

**Topics**
+ [Service Integration Patterns](connect-to-resource.md)
+ [Pass Parameters to a Service API](connect-parameters.md)
+ [Code Snippets](concepts-code-snippets.md)
+ [Supported AWS Services](connect-supported-services.md)