# Call Amazon SNS with Step Functions<a name="connect-sns"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized Amazon SNS integration is different than the Amazon SNS AWS SDK integration**  
There are no optimizations for the [Request Response](connect-to-resource.md#connect-default) or [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration patterns\.

Supported Amazon SNS APIs:

**Note**  
There is a quota for the maximum input or result data size for a task in Step Functions\. This restricts you to 262,144 bytes of data as a UTF\-8 encoded string when you send to, or receive data from, another service\. See [Quotas related to state machine executions](limits-overview.md#service-limits-state-machine-executions)\.
+ [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html)
  + [Request syntax](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_Example_1_Request)
  + Supported Parameters
    + [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters)
    + [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters)
    + [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters)
    + [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters)
    + [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters)
    + [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters)
    + [https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html#API_Publish_Example_1_Response)

The following includes a `Task` state that publishes to an Amazon Simple Notification Service \(Amazon SNS\) topic\.

```
{
 "StartAt": "Publish to SNS",
 "States": {
   "Publish to SNS": {
     "Type": "Task",
     "Resource": "arn:aws:states:::sns:publish",
     "Parameters": {
       "TopicArn": "arn:aws:sns:us-east-1:123456789012:myTopic",
       "Message.$": "$.input.message",
       "MessageAttributes": {
         "my_attribute_no_1": {
           "DataType": "String",
           "StringValue": "value of my_attribute_no_1"
         },
         "my_attribute_no_2": {
           "DataType": "String",
           "StringValue": "value of my_attribute_no_2"
         }
       }
     },
     "End": true
    }
  }
}
```

**Passing dynamic values**\. You can modify the above example to dynamically pass an attribute from this JSON payload:

```
{
"input": {
  "message": "Hello world"
},
"SNSDetails": {
  "attribute1": "some value",
  "attribute2": "some other value",
}
}
```

Append the `.$` to the `StringValue` field:

```
"MessageAttributes": {
  "my_attribute_no_1": {
      "DataType": "String",
      "StringValue.$": "$.SNSDetails.attribute1"
  },
  "my_attribute_no_2": {
      "DataType": "String",
      "StringValue.$": "$.SNSDetails.attribute2"
  }
```

The following includes a `Task` state that publishes to an Amazon SNS topic, and then waits for the task token to be returned\. See [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token)\.

```
{  
   "StartAt":"Send message to SNS",
   "States":{  
      "Send message to SNS":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::sns:publish.waitForTaskToken",
         "Parameters":{  
            "TopicArn":"arn:aws:sns:us-east-1:123456789012:myTopic",
            "Message":{  
               "Input.$":"$",
               "TaskToken.$":"$$.Task.Token"
            }
         },
         "End":true
      }
   }
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.