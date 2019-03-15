# Resource\-Based Policies<a name="resource-based-policies"></a>

Step Functions supports resource\-based policies\. You can create IAM policies that are attached to a specific resource\. For instance, the following policy grants permission to call `StartExecution` for a specific state machine\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "states:StartExecution"              
            ],
            "Resource": "arn:aws:states:us-east-1:123456789012:stateMachine:MyStateMachine"
        }
    ]
}
```

Combined with a more restrictive general policies, this would `Allow` only this state machine to be executed\.

For more information, see the following topics in the IAM User Guide:
+ [Resource\-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based)
+ [How IAM Roles Differ from Resource\-based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)
+ [Identity\-Based Policies and Resource\-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html)
+ [Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)