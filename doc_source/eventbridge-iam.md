# Amazon EventBridge<a name="eventbridge-iam"></a>

These example templates show how AWS Step Functions generates IAM policies based on the resources in your state machine definition\. For more information, see:
+ [IAM Policies for integrated services](service-integration-iam-templates.md)
+ [Service Integration Patterns](connect-to-resource.md)

## `PutEvents`<a name="eventbridge-iam-listconnections"></a>

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "events:PutEvents"
            ],
            "Resource": [
                "arn:aws:events:us-east-1:123456789012:event-bus/stepfunctions-sampleproject-eventbus"
            ],
            "Effect": "Allow"
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
                "events:PutEvents"
            ],
            "Resource": "arn:aws:events:*:*:event-bus/*"
        }
    ]
}
```

For more information about using EventBridge with Step Functions, see [Call EventBridge with Step Functions](connect-eventbridge.md)\.