# Call EventBridge with Step Functions<a name="connect-eventbridge"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized EventBridge integration is different than the EventBridge AWS SDK integration**  
The execution ARN and the state machine ARN are automatically appended to the `Resources` field of each `PutEventsRequestEntry`\.
If the response from `PutEvents` contains a non\-zero `FailedEntryCount` then the `Task` state fails with the error `EventBridge.FailedEntry`\.

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.

Step Functions provides a service integration API for integrating with Amazon EventBridge\. This lets you build event\-driven applications by sending custom events directly from Step Functions workflows\.

 To use the `PutEvents` API, you will need to create an EventBridge rule in your account that matches the specific pattern of the events you will send\. For example, you could: 
+ Create a Lambda function in your account that receives and prints an event that matches an EventBridge rule\.
+  Create an EventBridge rule in your account on the default event bus that matches a specific event pattern and targets the Lambda function\. 

 For more information, see:
+ [Adding Amazon EventBridge events with PutEvents](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-putevents.html) in the EventBridge User Guide\.
+ [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) in Service Integration Patterns\.

**Note**  
There is a quota for the maximum input or result data size for a task in Step Functions\. This restricts you to 262,144 bytes of data as a UTF\-8 encoded string when you send to, or receive data from, another service\. See [Quotas related to state machine executions](limits-overview.md#service-limits-state-machine-executions)\.

## Supported EventBridge API<a name="connect-eventbridge-apis"></a>

Supported EventBridge API and syntax include:
+ [https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutEvents.html](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutEvents.html)
  +  [Request syntax](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutEvents.html#API_PutEvents_RequestSyntax) 
  + Supported parameter:
    + [https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutEvents.html#eventbridge-PutEvents-request-Entries](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutEvents.html#eventbridge-PutEvents-request-Entries)
  +  [Response syntax](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutEvents.html#API_PutEvents_ResponseSyntax) 

The following includes a `Task` that sends a custom event:

```
{
  "Type": "Task",
  "Resource": "arn:aws:states:::events:putEvents",
  "Parameters": {
    "Entries": [
      {
        "Detail": {
          "Message": "MyMessage"
        },
        "DetailType": "MyDetailType",
        "EventBusName": "MyEventBus",
        "Source": "my.source"
      }
    ]
  },
  "End": true
}
```

## Error handling<a name="connect-eventbridge-error"></a>

The `PutEvents` API accepts an array of entries as input, then returns an array of result entries\. As long as the `PutEvents` action was successful, `PutEvents` will return an HTTP 200 response, even if one or more entries failed\. `PutEvents` returns the number of failed entries in the `FailedEntryCount` field\.

Step Functions checks whether the `FailedEntryCount` is greater than zero\. If it is greater than zero, Step Functions fails the state with the error `EventBridge.FailedEntry`\. This lets you use the built\-in error handling of Step Functions on task states to catch or retry when there are failed entries, rather than needing to use an additional state to analyze the `FailedEntryCount` from the response\.

**Note**  
If you have implemented idempotency and can safely retry on all entries, you can use Step Functions' retry logic\. Step Functions does not remove successful entries from the `PutEvents` input array before retrying\. Instead, it retries with the original array of entries\. 