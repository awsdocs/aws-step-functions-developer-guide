# Tag\-based Policies<a name="tag-based-policies"></a>

Step Functions supports policies based on tags\. For instance, you could restrict access to all Step Functions resources that include a tag with the key `environment` and the value `production`:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "states:TagResource",
                "states:UntagResource",
                "states:DeleteActivity",
                "states:DeleteStateMachine",
                "states:StopExecution"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {"aws:ResourceTag/environment": "production"}
            }
        }
    ]
}
```

This policy will `Deny` the ability to delete state machines or activities, stop executions, and add or delete new tags for all resources that have been tagged as `environment/production`\.

For more information on tagging, see:
+ [Tagging](concepts-tagging.md)
+ [Controlling Access Using IAM Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_iam-tags.html)