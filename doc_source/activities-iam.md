# Activities or no Tasks<a name="activities-iam"></a>

For a state machine that has only Activity tasks, or no tasks at all, use an IAM policy that denies access to all actions and resources\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

For more information on using activity tasks, see: [Activities](concepts-activities.md)\.