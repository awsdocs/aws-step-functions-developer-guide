# Connect to Resources<a name="connectors-resource"></a>

Connections to supported services are configured with an Amazon Resource Name in the `Resource` field of a `Task` state\. For a list of integrated services that can be managed directly from a Step Functions task state, see [Supported AWS Service Integrations for Step Functions](connectors-supported-services.md)

For instance, to publish an Amazon SNS topic:

```
"Resource": "arn:aws:states:::sns:publish",
```

The above example references the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) API of Amazon SNS, which will accept different parameters\. See [Call Amazon SNS With Step Functions](connectors-sns.md) for a list of supported parameters for Amazon SNS\.

## Make Synchronous Connections to Resources<a name="connectors-sync"></a>

When you reference a resource that has a response, the Amazon Resource Name will end with `.sync`\. For instance, when submitting a AWS Batch job, the `Resource` line in the state machine definition reads:

```
"Resource": "arn:aws:states:::batch:submitJob.sync",
```

Synchronous connections allow you to wait for a task to complete\. The task will pause until the task is complete, and will then progress to the next state\.

**Note**  
For information on configuring IAM for connected resources, see [IAM Policies for Integrated Services](connectors-iam-templates.md)\.