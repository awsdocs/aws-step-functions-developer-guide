# Call Amazon SQS with Step Functions<a name="connectors-sqs"></a>

Step Functions can control some AWS services directly from the Amazon States Language\. For more information, see:
+ [Service Integrations](concepts-connectors.md)
+ [Pass Parameters to a Service API](connectors-parameters.md)

Supported APIs:

**Note**  
There is a limit on the maximum input or result data size for a task in Step Functions\. This limits you to 32,768 characters of data when you send to, or receive data from, another service\. See [Limits Related to State Machine Executions](limits.md#service-limits-state-machine-executions)\.
+ [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html)
  + [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters)
  + [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters)
  + [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters)
  + [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters)
  + [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters)
  + [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_RequestParameters)
+ [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_ResponseElements](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html#API_SendMessage_ResponseElements)

The following includes a `Task` state that sends an Amazon SQS message\.

```
{
 "StartAt": "Send to SQS",
 "States": {
   "Send to SQS": {
     "Type": "Task",
     "Resource": "arn:aws:states:::sqs:sendMessage",
     "Parameters": {
       "QueueUrl": "https://sqs.us-east-1.amazonaws.com/123456789012/myQueue",
       "MessageBody.$": "$.input.message",
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

To learn more about receiving messages in Amazon SQS, see [Receive and Delete Your Message](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-getting-started.html#step-receive-delete-message) in the Amazon Simple Queue Service Developer Guide\.

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](connectors-iam-templates.md)\.