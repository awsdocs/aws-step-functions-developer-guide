# AWS Service Integrations<a name="concepts-connectors"></a>

Step Functions integrates with some AWS services, including the ability to directly call and pass parameters to the API of those services\. You can use Step Functions to coordinate these services directly from the Amazon States Language\. For instance, using Step Functions you can coordinate other services to: 
+ Invoke a Lambda function\.
+ Run a AWS Batch job and then perform different actions based on the results
+ Insert or get an item from DynamoDB
+ Run an Amazon ECS task and wait for it to complete
+ Publish a topic in Amazon SNS
+ Send a message in Amazon SQS

For working examples of state machines that control other AWS services, see [Sample Projects](create-sample-projects.md)\. These sample projects will deploy a state machine and all related resources\.

**Topics**
+ [Connect to Resources](connectors-resource.md)
+ [Pass Parameters to a Service API](connectors-parameters.md)
+ [Code Snippets](concepts-code-snippets.md)
+ [Supported AWS Services](connectors-supported-services.md)