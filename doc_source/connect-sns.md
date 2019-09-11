# Call Amazon SNS with Step Functions<a name="connect-sns"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information, see:
+ [Service Integrations](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

Supported APIs:

**Note**  
There is a limit on the maximum input or result data size for a task in Step Functions\. This limits you to 32,768 characters of data when you send to, or receive data from, another service\. See [Limits Related to State Machine Executions](limits.md#service-limits-state-machine-executions)\.
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

The following includes a `Task` state that publishes an Amazon SNS topic\.

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
         "my attribute no 1": {
           "DataType": "String",
           "StringValue": "value of my attribute no 1"
         },
         "my attribute no 2": {
           "DataType": "String",
           "StringValue": "value of my attribute no 2"
         }
       }
     },
     "End": true
    }
  }
}
```

The following includes a `Task` state that publishes an Amazon SNS topic, and then waits for the task token to be returned\. See [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token)\.

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

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](service-integration-iam-templates.md)\.