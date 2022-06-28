# Microservice orchestration<a name="use-cases-orchestration"></a>

Microservice architecture breaks applications into loosely coupled services\. Benefits include improved scalability, increased resiliency, and faster time to market\. Each microservice is independent, making it easy to scale up a single service or function without needing to scale the entire application\. Individual services are loosely coupled, letting independent teams focus on a single business process, without the need for them to understand the entire application\.  Microservices also let you choose which individual components suit your business needs, giving you the flexibility to change your selection without rewriting your entire workflow\. Different teams can use the programming languages and frameworks of their choice to work with their microservice, and this microservice can still communicate with any other in the application through application programming interfaces \(APIs\)\.

Step Functions gives you several ways to manage your microservice workflows\. For long\-running workflows you can use Standard Workflows with the AWS Fargate integration to orchestrate applications running in containers\. For short\-duration, high\-volume workflows that require an immediate response, [Synchronous Express Workflows](concepts-express-synchronous.md) are ideal\. These can be used for web\-based or mobile applications, which often have workflows of short duration, and require the completion of a series of steps before they return a response\. You can directly trigger a Synchronous Express Workflows from Amazon API Gateway, and the connection is held open until the workflow completes or timeouts\. For short duration workflows that do not require an immediate response, Step Functions provides Asynchronous Express Workflows\. 

Examples of some API orchestrations that use Step Functions include:

**Synchronous or real\-time workflows**
+ Change a value in a record such as updating an employee’s last name and have the change immediately visible on the screen\. 
+ Update an order during checkout, such as adding, removing, or changing the quantity of an item, then immediately reflect the update back to the customer\.
+ Run a quick processing job and immediately return the result back to the requester\.

**Container Orchestration**
+ Run jobs on Kubernetes with Amazon Elastic Kubernetes Service or on Amazon Elastic Container Service \(ECS\) with Fargate and integrate with other AWS services, such as sending notifications with Amazon SNS, as part of the same workflow\. 