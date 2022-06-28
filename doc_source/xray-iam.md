# X\-Ray<a name="xray-iam"></a>

These example templates show how AWS Step Functions generates IAM policies based on the resources in your state machine definition\. For more information, see:
+ [IAM Policies for integrated services](service-integration-iam-templates.md)
+ [Service Integration Patterns](connect-to-resource.md)

To enable X\-Ray tracing, you will need an IAM policy with suitable permissions to allow tracing\. If your state machine uses other integrated services, you may need additional IAM policies\. See the IAM policies for your specific service integrations\.

When you create a state machine with X\-Ray tracing enabled, an IAM policy is automatically created\. 

**Note**  
 If you enable X\-Ray tracing for an existing state machine you must ensure that you add a policy with sufficient permissions to enable X\-Ray traces\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "xray:PutTraceSegments",
                "xray:PutTelemetryRecords",
                "xray:GetSamplingRules",
                "xray:GetSamplingTargets"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

For more information about using X\-Ray with Step Functions, see [AWS X\-Ray and Step Functions](concepts-xray-tracing.md)\.