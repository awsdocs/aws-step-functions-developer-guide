# AWS Glue<a name="glue-iam"></a>

These example templates show how AWS Step Functions generates IAM policies based on the resources in your state machine definition\. For more information, see:
+ [IAM Policies for Integrated Services](service-integration-iam-templates.md)
+ [Service Integration Patterns](connect-to-resource.md)

AWS Glue does not have resource\-based control\.

------
#### [ Run a Job \(\.sync\) ]

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:StartJobRun",
                "glue:GetJobRun",
                "glue:GetJobRuns",
                "glue:BatchStopJobRun"
            ],
            "Resource": "*"
        }
    ]
}
```

------
#### [ Request Response and Callback \(\.waitForTaskToken\) ]

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:StartJobRun"
            ],
            "Resource": "*"
        }
    ]
}
```

------