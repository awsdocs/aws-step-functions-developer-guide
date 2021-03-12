# Amazon EMR<a name="emr-iam"></a>

These example templates show how AWS Step Functions generates IAM policies based on the resources in your state machine definition\. For more information, see:
+ [IAM Policies for Integrated Services](service-integration-iam-templates.md)
+ [Service Integration Patterns](connect-to-resource.md)

## `addStep`<a name="emr-iam-addstep"></a>

*Static resources*

```
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Effect": "Allow",
          "Action": [
              "elasticmapreduce:AddJobFlowSteps",
              "elasticmapreduce:DescribeStep", 
              "elasticmapreduce:CancelSteps"
          ],
          "Resource": [
              "arn:aws:elasticmapreduce:[[region]]:[[accountId]]:cluster/[[clusterId]]"
          ]
      },
      {
          "Effect": "Allow",
          "Action": [
            "events:PutTargets",
            "events:PutRule",
            "events:DescribeRule"
          ],
          "Resource": [
            "arn:aws:events:[[region]]:[[accountId]]:rule/StepFunctionsGetEventForEMRAddJobFlowStepsRule"
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
              "elasticmapreduce:AddJobFlowSteps",
              "elasticmapreduce:DescribeStep",
              "elasticmapreduce:CancelSteps"
          ],
          "Resource": "arn:aws:elasticmapreduce:*:*:cluster/*"
      },
      {
          "Effect": "Allow",
          "Action": [
            "events:PutTargets",
            "events:PutRule",
            "events:DescribeRule"
          ],
          "Resource": [
            "arn:aws:events:[[region]]:[[accountId]]:rule/StepFunctionsGetEventForEMRAddJobFlowStepsRule"
          ]
      }
  ]
}
```

## `cancelStep`<a name="emr-iam-cancelstep"></a>

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "elasticmapreduce:CancelSteps",
            "Resource": [
                "arn:aws:elasticmapreduce:[[region]]:[[accountId]]:cluster/[[clusterId]]"
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
            "Action": "elasticmapreduce:CancelSteps",
            "Resource": "arn:aws:elasticmapreduce:*:*:cluster/*"
        }
    ]
}
```

## `createCluster`<a name="emr-iam-createcluster"></a>

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:RunJobFlow",
                "elasticmapreduce:DescribeCluster", 
                "elasticmapreduce:TerminateJobFlows"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": [
                "arn:aws:iam::{{account}}:role/[[roleName]]"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
              "events:PutTargets",
              "events:PutRule",
              "events:DescribeRule"
            ],
            "Resource": [
                "arn:aws:events:[[region]]:[[accountId]]:rule/StepFunctionsGetEventForEMRRunJobFlowRule"
            ]
        }
    ]
}
```

## `setClusterTerminationProtection`<a name="emr-iam-clusterterminationprotection"></a>

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "elasticmapreduce:SetTerminationProtection",
            "Resource": [
                "arn:aws:elasticmapreduce:[[region]]:[[accountId]]:cluster/[[clusterId]]"
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
            "Action": "elasticmapreduce:SetTerminationProtection",
            "Resource": "arn:aws:elasticmapreduce:*:*:cluster/*"
        }
    ]
}
```

## `modifyInstanceFleetByName`<a name="emr-iam-modifyinstancefleetbyname"></a>

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:ModifyInstanceFleet",
                "elasticmapreduce:ListInstanceFleets"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:[[region]]:[[accountId]]:cluster/[[clusterId]]"
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
                "elasticmapreduce:ModifyInstanceFleet",
                "elasticmapreduce:ListInstanceFleets"
            ],
            "Resource": "arn:aws:elasticmapreduce:*:*:cluster/*"
        }
    ]
}
```

## `modifyInstanceGroupByName`<a name="emr-iam-modifyinstancegroupbyname"></a>

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:ModifyInstanceGroups",
                "elasticmapreduce:ListInstanceGroups"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:[[region]]:[[accountId]]:cluster/[[clusterId]]"
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
                "elasticmapreduce:ModifyInstanceGroups",
                "elasticmapreduce:ListInstanceGroups"
            ],
            "Resource": "*"
        }
    ]
}
```

## `terminateCluster`<a name="emr-iam-terminatecluster"></a>

*Static resources*

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:TerminateJobFlows",
                "elasticmapreduce:DescribeCluster"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:[[region]]:[[accountId]]:cluster/[[clusterId]]"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "events:PutTargets",
                "events:PutRule",
                "events:DescribeRule"
            ],
            "Resource": [
                "arn:aws:events:[[region]]:[[accountId]]:rule/StepFunctionsGetEventForEMRTerminateJobFlowsRule"
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
                "elasticmapreduce:TerminateJobFlows",
                "elasticmapreduce:DescribeCluster"
            ],
            "Resource": "arn:aws:elasticmapreduce:*:*:cluster/*"
        },
        {
            "Effect": "Allow",
            "Action": [
              "events:PutTargets",
              "events:PutRule",
              "events:DescribeRule"
            ],
            "Resource": [
                "arn:aws:events:[[region]]:[[accountId]]:rule/StepFunctionsGetEventForEMRTerminateJobFlowsRule"
            ]
        }
    ]
}
```