# IAM Policy for AWS Lambda<a name="lambda-iam"></a>

AWS Step Functions generates an IAM policy based on your state machine definition\. For a state machine with two Lambda task states that call `function1` and `function2`, a policy with `lambda:Invoke` permissions for the two functions must be used\. 

For example:

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