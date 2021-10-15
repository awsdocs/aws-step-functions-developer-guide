# AWS Step Functions and AWS SAM<a name="concepts-sam-sfn"></a>

You can use the AWS SAM CLI in conjunction with the AWS Toolkit for Visual Studio Code as part of an integrated experience to create state machines locally\. You can build a serverless application with AWS SAM, then build out your state machine in the VS Code IDE\. After you've done that, you can validate, package, and deploy your resources\. Optionally, you can publish to the AWS Serverless Application Repository\. 

## Why use Step Functions with AWS SAM?<a name="concepts-sam-sfn-integration"></a>

When you use Step Functions with AWS SAM you can:
+ Get started in minutes using a AWS SAM sample template\.
+ Build your state machine into your serverless application\.
+ Use variable substitution to substitute ARNs into your state machine at time of deployment\.
+ Simplify specifying your state machine role using AWS SAM policy templates\.
+ Trigger state machine executions with API Gateway, EventBridge events, or on a schedule within your AWS SAM template\. 

### Step Functions integration with the AWS SAM specification<a name="concepts-sam-sfn-ots2"></a>

You can use the [AWS SAM Policy Templates](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-templates.html) available to easily add permissions to your state machine\. These enable you to orchestrate Lambda functions and other AWS resources to form complex and robust workflows\. 

### Step Functions integration with the SAM CLI<a name="concepts-sam-sfn-ots3"></a>

Step Functions is integrated with the AWS SAM CLI\. Use this to quickly develop a state machine into your serverless application\.

Try the [Create a Step Functions State Machine Using AWS SAM](tutorial-state-machine-using-sam.md) tutorial to learn how to use AWS SAM to create state machines\.

Supported AWS SAM CLI functions include: 


| CLI Command | Description | 
| --- | --- | 
| sam init |  Initializes a Serverless Application with an AWS SAM template\. Can be used with a SAM template for Step Functions\.  | 
| sam validate | Validates an AWS SAM template\. | 
| sam package |  Packages an AWS SAM application\. It creates a ZIP file of your code and dependencies, and uploads it to Amazon S3\. It then returns a copy of your AWS SAM template, replacing references to local artifacts with the Amazon S3 location where the command uploaded the artifacts\.  | 
| sam deploy | Deploys an AWS SAM application\. | 
| sam publish |  Publish an AWS SAM application to the AWS Serverless Application Repository\. This command takes a packaged AWS SAM template and publishes the application to the specified region\.  | 

**Note**  
Using AWS SAM local you can emulate Lambda and API Gateway locally\. However, you cannot emulate Step Functions locally using AWS SAM\.

To learn more, see the following: 
+ Go through a step\-by\-step tutorial [Create a Step Functions State Machine Using AWS SAM](tutorial-state-machine-using-sam.md)\.
+ Specify a [AWS::Serverless::StateMachine](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-statemachine.html) resource\.
+ Find [AWS SAM Policy Templates](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-templates.html) to use\.
+ Use [AWS Toolkit for Visual Studio Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/stepfunctions.html) with Step Functions\. 
+ The [AWS SAM CLI reference\.](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-command-reference.html)