# Amazon Virtual Private Cloud Endpoint Policies for Step Functions<a name="vpc-iam"></a>

You can create a Amazon VPC endpoint policy for Step Functions in which you specify the following:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which the actions can be performed\.

The following example shows an Amazon VPC endpoint policy that allows one IAM user to create state machines, and denies all IAM users permission to delete state machines\. The example policy also grants all IAM users execution permission\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "*Execution",
            "Resource": "*",
            "Effect": "Allow",
            "Principal": "*"
        },
        {
            "Action": "states:CreateStateMachine",
            "Resource": "*",
            "Effect": "Allow",
            "Principal": {
              "AWS": "arn:aws:iam:123456789012:user/MyUser"
            }
        },
        {
            "Action": "states:DeleteStateMachine",
            "Resource": "*",
            "Effect": "Deny",
            "Principal": "*"
        }
    ]
}
```

For more information about creating endpoint policies, see the following: 
+  [Creating Granular IAM Permissions for Non\-Admin Users](concept-create-iam-advanced.md) 
+  [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) 