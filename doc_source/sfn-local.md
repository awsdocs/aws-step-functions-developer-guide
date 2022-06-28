# Testing Step Functions State Machines Locally<a name="sfn-local"></a>

AWS Step Functions Local is a downloadable version of Step Functions that lets you develop and test applications using a version of Step Functions running in your own development environment\. The local version of Step Functions can invoke AWS Lambda functions, both in AWS and while running locally\. You can also coordinate other [supported AWS services](concepts-service-integrations.md)\.

While running Step Functions Local, you can use one of the following ways to invoke service integrations:
+ Configuring local endpoints for AWS Lambda and other services\. For information about the supported endpoints, see [Setting Configuration Options for Step Functions Local](sfn-local-config-options.md)\.
+ Making calls directly to an AWS service from Step Functions Local\.
+ Mocking the response from service integrations\. For information about using mocked service integrations, see [Using Mocked Service Integrations](sfn-local-test-sm-exec.md)\.

AWS Step Functions Local is available as a JAR package or a self\-contained Docker image that runs on Microsoft Windows, Linux, macOS, and other platforms that support Java or Docker\.

**Warning**  
The downloadable version of AWS Step Functions is intended to be used only for testing and should never be used to process sensitive information\.

The following topics describe how you can set up Step Functions Local using Docker and JAR file, and run Step Functions Local to work with AWS Lambda, AWS Serverless Application Model\(AWS SAM\) CLI Local, or other supported services\.

**Topics**
+ [Setting Up Step Functions Local \(Downloadable Version\) and Docker](sfn-local-docker.md)
+ [Setting Up Step Functions Local \(Downloadable Version\) \- Java Version](sfn-local-jar.md)
+ [Setting Configuration Options for Step Functions Local](sfn-local-config-options.md)
+ [Running Step Functions Local on Your Computer](sfn-local-computer.md)
+ [Testing Step Functions and AWS SAM CLI Local](sfn-local-lambda.md)
+ [Using Mocked Service Integrations](sfn-local-test-sm-exec.md)