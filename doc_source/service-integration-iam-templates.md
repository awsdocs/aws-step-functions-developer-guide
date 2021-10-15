# IAM Policies for integrated services<a name="service-integration-iam-templates"></a>

When you create a state machine in the AWS Step Functions console, Step Functions produces an AWS Identity and Access Management \(IAM\) policy based on the resources used in your state machine definition as follows: 
+  If your state machine uses one of the Optimized integrations, it will create a policy with the necessary permissions and roles for your state machine\. 
+  If your state machine uses one of the AWS SDK integrations, an IAM role with partial permissions will be created\. Afterwards, you can use the IAM console to add any missing role policies\. 

The following examples show how Step Functions generates an IAM policy based on your state machine definition\. Items in the example code such as *`[[resourceName]]`* are replaced with the static resources listed in your state machine definition\. If you have multiple static resources, there will be an entry for each in the IAM role\.

## Dynamic vs\. Static Resources<a name="connect-iam-dynamic-static"></a>

Static resources are defined directly in the task state of your state machine\. When you include the information about the API actions you call directly in your task states, Step Functions creates an IAM role for only those resources\. 

Dynamic resources are those that are passed in to your state input, and accessed using a Path \(see [Paths](amazon-states-language-paths.md)\)\. If you are passing dynamic resources to your task, Step Functions will create a more privileged policy that specifies: `"Resource": "*"`\.

## Additional permissions for tasks using the Run a Job pattern<a name="connect-iam-sync-async"></a>

For tasks that use the [Run a Job](connect-to-resource.md#connect-sync) pattern \(those ending in `.sync`\), additional permissions are needed to monitor and receive a response from the API actions of connected services\. The related policies include more permissions than for tasks that use the Request Response or Wait for Callback patterns\. See [Service Integration Patterns](connect-to-resource.md) for information about synchronous tasks\.

Step Functions uses two methods to monitor a job's status when a job is run on a connected service, polling and events\. 

Polling requires permission for `Describe` or `Get` API actions, such as `ecs:DescribeTasks` or `glue:GetJobRun`\. If these permissions are missing from your role, then Step Functions may be unable to determine the status of your job\. This is because some Run a Job \(\.sync\) service integrations do not support EventBridge events, and some services only send events on a best\-effort basis\. 

Events sent from AWS services to Amazon EventBridge are directed to Step Functions using a managed rule, and require permissions for `events:PutTargets`, `events:PutRule`, and `events:DescribeRule`\. If these permissions are missing from your role, there may be a delay before Step Functions becomes aware of the completion of your job\. For more information about EventBridge events, see [Events from AWS services](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-service-event.html)\. 

**Note**  
For Run a Job \(\.sync\) tasks that support both polling and events, your task may still complete properly using events\. This can occur even if your role lacks the required permissions for polling\. In this case, you may not immediately notice that the polling permissions are incorrect or missing\. In the rare instance that the event fails to be delivered to or processed by Step Functions, your execution could become stuck\. To verify that your polling permissions are configured correctly, you can run an execution in an environment without EventBridge events in the following ways:   
 Delete the managed rule from EventBridge, which is responsible for forwarding events to Step Functions\. This managed rule is shared by all state machines in your account, so you should perform this action only in a test or development account to avoid any unintentional impact on other state machines\. You can identify the specific managed rule to delete by inspecting the `Resource` field used for `events:PutRule` in the policy template for the target service\. The managed rule will be recreated the next time you create or update a state machine that uses that service integration\. For more information on deleting EventBridge rules, see [Disabling or deleting a rule](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-delete-rule.html)\. 
 Test with Step Functions Local, which does not support the use of events to complete Run a Job \(\.sync\) tasks\. To use Step Functions Local, assume the IAM role used by your state machine\. You may need to edit the Trust Relationship\. Set the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN` environment variables to the assumed role's values, then launch Step Functions Local using java \-jar StepFunctionsLocal\.jar\. Last, use the AWS CLI with the \-\-endpoint\-url parameter to create a state machine, start an execution, and get the execution history\. For more information, see [Setting Up Step Functions Local \(Downloadable Version\)](sfn-local.md)\. 

If a task that uses the Run a Job \(\.sync\) pattern is stopped, Step Functions will make a best\-effort attempt to cancel the task\. This requires permission to `Cancel`, `Stop`, `Terminate`, or `Delete` API actions, such as `batch:TerminateJob` or `eks:DeleteCluster`\. If these permissions are missing from your role, Step Functions will be unable to cancel your task and you may accrue additional charges while it continues to run\. For more information on stopping tasks, see [Run a Job](connect-to-resource.md#connect-sync)\. 

## Policy templates used to create IAM roles<a name="connect-iam-topics"></a>

 The following topics include the policy templates used when you choose to have Step Functions create a new role for you\. 

**Note**  
Review these templates to understand how Step Functions creates your IAM policies, and as an example of how to manually create IAM policies for Step Functions when working with other AWS services\. For more information about Step Functions service integrations, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

**Topics**
+ [Dynamic vs\. Static Resources](#connect-iam-dynamic-static)
+ [Additional permissions for tasks using the Run a Job pattern](#connect-iam-sync-async)
+ [Policy templates used to create IAM roles](#connect-iam-topics)
+ [AWS Lambda](lambda-iam.md)
+ [AWS Batch](batch-iam.md)
+ [Amazon DynamoDB](dynamo-iam.md)
+ [Amazon ECS/Fargate](ecs-iam.md)
+ [Amazon SNS](sns-iam.md)
+ [Amazon SQS](sqs-iam.md)
+ [AWS Glue](glue-iam.md)
+ [SageMaker](sagemaker-iam.md)
+ [Amazon EMR](emr-iam.md)
+ [Amazon EMR on EKS](emr-eks-iam.md)
+ [AWS CodeBuild](codebuild-iam.md)
+ [X\-Ray](xray-iam.md)
+ [Athena](athena-iam.md)
+ [Amazon EKS](eks-iam.md)
+ [API Gateway](api-gateway-iam.md)
+ [AWS Glue DataBrew](databrew-iam.md)
+ [Amazon EventBridge](eventbridge-iam.md)
+ [AWS Step Functions](stepfunctions-iam.md)
+ [Activities](activities-iam.md)