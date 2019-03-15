# Connect to Resources<a name="connectors-resource"></a>

Connections to supported services are configured with an Amazon Resource Name \(ARN\) in the `Resource` field of a `Task` state\. For a list of integrated services that can be managed directly from a Step Functions task state, see [Supported AWS Service Integrations for Step Functions](connectors-supported-services.md)\.

For example, to publish an Amazon SNS topic:

```
"Resource": "arn:aws:states:::sns:publish",
```

This example references the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) API of Amazon SNS, which will accept different parameters\. See [Call Amazon SNS with Step Functions](connectors-sns.md) for a list of supported parameters for Amazon SNS\.

## Make Synchronous Connections to Resources<a name="connectors-sync"></a>

When you reference a resource that has a response, the ARN will end with `.sync`\. For example, when submitting an AWS Batch job, the `Resource` line in the state machine definition reads as follows\.

```
"Resource": "arn:aws:states:::batch:submitJob.sync",
```

Synchronous connections allow you to wait for a task to complete\. The task pauses until the task is complete, and then progresses to the next state\.

**Note**  
For information about configuring AWS Identity and Access Management \(IAM\) for connected resources, see [IAM Policies for Integrated Services](connectors-iam-templates.md)\.