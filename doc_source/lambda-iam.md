# AWS Lambda<a name="lambda-iam"></a>

These example templates show how AWS Step Functions generates IAM policies based on the resources in your state machine definition\. For more information, see:
+ [IAM Policies for Integrated Services](service-integration-iam-templates.md)
+ [Service Integration Patterns](connect-to-resource.md)

AWS Step Functions generates an IAM policy based on your state machine definition\. For a state machine with two AWS Lambda task states that call `function1` and `function2`, a policy with `lambda:Invoke` permissions for the two functions must be used\. 

This is shown in the following example\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:[[region]]:[[accountId]]:function:[[function1]]",
                "arn:aws:lambda:[[region]]:[[accountId]]:function:[[function2]]"
            ]
        }
    ]
}
```