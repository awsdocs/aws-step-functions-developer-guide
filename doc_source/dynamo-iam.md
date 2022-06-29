# Amazon DynamoDB<a name="dynamo-iam"></a>

These example templates show how AWS Step Functions generates IAM policies based on the resources in your state machine definition\. For more information, see:
+ [IAM Policies for integrated services](service-integration-iam-templates.md)
+ [Service Integration Patterns](connect-to-resource.md)

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "dynamodb:DeleteItem"
            ],
            "Resource": [
                "arn:aws:dynamodb:[[region]]:[[accountId]]:table/[[tableName]]"
            ]
        }
    ]
}
```

*Dynamic resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "dynamodb:DeleteItem"
            ],
            "Resource": "*"
        }
    ]
}
```

For more information about the IAM policies for all DynamoDB API actions, see [IAM policies with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/using-identity-based-policies.html) in the *Amazon DynamoDB Developer Guide*\. Additionally, for information about the IAM policies for PartiQL for DynamoDB, see [IAM policies with PartiQL for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ql-iam.html) in the *Amazon DynamoDB Developer Guide*\.