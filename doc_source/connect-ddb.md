# Call DynamoDB APIs with Step Functions<a name="connect-ddb"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**Note**  
There is a quota for the maximum input or result data size for a task in Step Functions\. This restricts you to 262,144 bytes of data as a UTF\-8 encoded string when you send to, or receive data from, another service\. See [Quotas related to state machine executions](limits-overview.md#service-limits-state-machine-executions)\.

**How the DynamoDB Lambda integration is different than the DynamoDB AWS SDK integration**  
There is no optimization for the [Request Response](connect-to-resource.md#connect-default) integration pattern\.
The [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration pattern is not supported\.

Supported Amazon DynamoDB APIs and syntax:
+ [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html)
  + [Request syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#API_GetItem_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-Key](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-Key)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-TableName](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-TableName)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-AttributesToGet](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-AttributesToGet)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ConsistentRead](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ConsistentRead)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ExpressionAttributeNames](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ExpressionAttributeNames)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ProjectionExpression](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ProjectionExpression)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ReturnConsumedCapacity](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ReturnConsumedCapacity)
  + [Response syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#API_GetItem_ResponseSyntax)
+ [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html)
  + [Request syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#API_PutItem_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-Item](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-Item)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-TableName](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-TableName)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ConditionalOperator](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ConditionalOperator)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ConditionExpression](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ConditionExpression)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-Expected](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-Expected)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ExpressionAttributeNames](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ExpressionAttributeNames)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ExpressionAttributeValues](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ExpressionAttributeValues)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnConsumedCapacity](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnConsumedCapacity)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnItemCollectionMetrics](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnItemCollectionMetrics)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnValues](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnValues)
  + [Response syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#API_PutItem_ResponseSyntax)
+ [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html)
  + [Request syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#API_DeleteItem_ResponseSyntax)
+ [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html)
  + [Request syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#API_UpdateItem_RequestSyntax)
  + Supported parameters:
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
    + [https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#API_UpdateItem_ResponseSyntax)

The following is a `Task` state that retrieves a message from DynamoDB\.

```
    "Read Next Message from DynamoDB": {
      "Type": "Task",
      "Resource": "arn:aws:states:::dynamodb:getItem",
      "Parameters": {
        "TableName": "TransferDataRecords-DDBTable-3I41R5L5EAGT",
        "Key": {
          "MessageId": {"S.$": "$.List[0]"}
        }
      },
      "ResultPath": "$.DynamoDB",
      "Next": "Send Message to SQS"
    },
```

**Note**  
You cannot pass a map or list to DynamoDB inside a map\.

To see this state in a working example, see the [Transfer Data Records \(Lambda, DynamoDB, Amazon SQS\)](sample-project-transfer-data-sqs.md) sample project\.

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.