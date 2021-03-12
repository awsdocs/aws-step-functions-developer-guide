# Call Amazon EMR with Step Functions<a name="connect-emr"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information, see the following:
+ [Service Integrations](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

To integrate AWS Step Functions with Amazon EMR, you use the provided Amazon EMR service integration APIs\.  The service integration APIs are similar to the corresponding Amazon EMR APIs, with some differences in the fields that are passed and in the responses that are returned\.

Step Functions does not terminate an Amazon EMR cluster automatically if execution is stopped\. If your state machine stops before your Amazon EMR cluster has terminated, your cluster may continue running indefinitely, and can accrue additional charges\. To avoid this, ensure that any Amazon EMR cluster you create is terminated properly\. For more information, see:
+ [Control Cluster Termination](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-plan-termination.html) in the Amazon EMR User Guide\.
+ The Service Integration Patterns [Run a Job](connect-to-resource.md#connect-sync) section\.

**Note**  
As of `emr-5.28.0`, you can specify the parameter `StepConcurrencyLevel` when creating a cluster to allow multiple steps to run in parallel on a single cluster\. You can use the Step Functions `Map` and `Parallel` states to submit work in parallel to the cluster\.

The availability of Amazon EMR service integration is subject to the availability of Amazon EMR APIs\. Please check the [Amazon EMR](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/govcloud-emr.html) documentation for limitations in special regions\.

The following table describes the differences between each service integration API and its corresponding Amazon EMR API\.


**Amazon EMR Service Integration APIs and Corresponding Amazon EMR APIs**  

| Amazon EMR Service Integration API | Corresponding EMR API | Differences | 
| --- | --- | --- | 
| createCluster Creates and starts running a cluster \(job flow\)\.  Amazon EMR is linked directly to a unique type of IAM role known as a service\-linked role\. For `createCluster` and `createCluster.sync` to work, you must have configured the necessary permissions to create the service\-linked role `AWSServiceRoleForEMRCleanup`\. For more information about this, including a statement you can add to your IAM permissions policy, see [Using the Service\-Linked Role for Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/using-service-linked-roles.html)\.  | [runJobFlow](https://docs.aws.amazon.com/emr/latest/APIReference/API_RunJobFlow.html) | createCluster uses the same request syntax as [runJobFlow](https://docs.aws.amazon.com/emr/latest/APIReference/API_RunJobFlow.html), except for the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/connect-emr.html)Response is this: <pre>{<br />  "ClusterId": "string"<br />}</pre> Amazon EMR uses this: <pre>{<br />  "JobFlowId": "string"<br />}</pre>  | 
| createCluster\.sync Creates and starts running a cluster \(job flow\)\.  | [runJobFlow](https://docs.aws.amazon.com/emr/latest/APIReference/API_RunJobFlow.html) | The same as createCluster, but waits for the cluster to reach the WAITING state\. | 
| setClusterTerminationProtection Locks a cluster \(job flow\) so the EC2 instances in the cluster cannot be terminated by user intervention, an API call, or a job\-flow error\. | [setTerminationProtection](https://docs.aws.amazon.com/emr/latest/APIReference/API_SetTerminationProtection.html) | Request uses this:<pre>{<br />  "ClusterId": "string"<br />}</pre> Amazon EMR uses this:<pre>{<br />  "JobFlowIds": ["string"]<br />}</pre>  | 
| terminateCluster Shuts down a cluster \(job flow\)\.  | [terminateJobFlows](https://docs.aws.amazon.com/emr/latest/APIReference/API_TerminateJobFlows.html) | Request uses this:<pre>{<br />  "ClusterId": "string"<br />}</pre> Amazon EMR uses this:<pre>{<br />  "JobFlowIds": ["string"]<br />}</pre> | 
| terminateCluster\.syncShuts down a cluster \(job flow\)\. | [terminateJobFlows](https://docs.aws.amazon.com/emr/latest/APIReference/API_TerminateJobFlows.html) | The same as terminateCluster, but waits for the cluster to terminate\. | 
| addStep Adds a new step to a running cluster\. | [addJobFlowSteps](https://docs.aws.amazon.com/emr/latest/APIReference/API_AddJobFlowSteps.html) | Request uses the key "ClusterId"\. Amazon EMR uses "JobFlowId"\. Request uses a single step\.<pre>{<br />  "Step": <"StepConfig object"><br />}</pre> Amazon EMR uses this:<pre>{<br />  "Steps": [<StepConfig objects>]<br />}</pre> Response is this:<pre>{<br />  "StepId": "string"<br />}</pre> Amazon EMR returns this:<pre>{<br />  "StepIds": [<strings>]<br />}</pre>  | 
| addStep\.syncAdds a new step to a running cluster\. | [addJobFlowSteps](https://docs.aws.amazon.com/emr/latest/APIReference/API_AddJobFlowSteps.html) | The same as addStep, but waits for the step to complete\. | 
| cancelStep Cancels a pending step in a running cluster\. | [cancelSteps](https://docs.aws.amazon.com/emr/latest/APIReference/API_CancelSteps.html) |  Request uses this:<pre>{<br />  "StepId": "string"<br />}</pre> Amazon EMR uses this:<pre>{<br />  "StepIds": [<strings>]<br />}</pre> Response is this:<pre>{<br />  "CancelStepsInfo": <CancelStepsInfo object><br />}</pre> Amazon EMR uses this:<pre>{<br />  "CancelStepsInfoList": [<CancelStepsInfo objects>]<br />}</pre>  | 
| modifyInstanceFleetByName Modifies the target On\-Demand and target Spot capacities for the instance fleet with the specified `InstanceFleetName`\. | [modifyInstanceFleet](https://docs.aws.amazon.com/emr/latest/APIReference/API_ModifyInstanceFleet.html) | Request is the same as for modifyInstanceFleet, except for the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/connect-emr.html)  | 
| modifyInstanceGroupByName Modifies the number of nodes and configuration settings of an instance group\. | [modifyInstanceGroups](https://docs.aws.amazon.com/emr/latest/APIReference/API_ModifyInstanceGroups.html) | Request is this:<pre>{<br />  "ClusterId": "string",<br />  "InstanceGroup": <InstanceGroupModifyConfig object><br />}</pre> Amazon EMR uses a list: <pre>{<br />  "ClusterId": ["string"],<br />  "InstanceGroups": [<InstanceGroupModifyConfig objects>]<br />}</pre> Within the `InstanceGroupModifyConfig` object, the field `InstanceGroupId` is not allowed\. A new field, `InstanceGroupName`, has been added\. At runtime the `InstanceGroupId` is determined automatically by the service integration by calling `ListInstanceGroups` and parsing the result\.  | 

The following includes a `Task` state that creates a cluster\.

```
"Create_Cluster": {
    "Type": "Task",
    "Resource": "arn:aws:states:::elasticmapreduce:createCluster.sync",
    "Parameters": {
        "Name": "MyWorkflowCluster",
        "VisibleToAllUsers": true,
        "ReleaseLabel": "emr-5.28.0",
        "Applications": [
            {
                "Name": "Hive"
            }
        ],
        "ServiceRole": "EMR_DefaultRole",
        "JobFlowRole": "EMR_EC2_DefaultRole",
        "LogUri": "s3n://aws-logs-123456789012-us-east-1/elasticmapreduce/",
        "Instances": {
            "KeepJobFlowAliveWhenNoSteps": true,
            "InstanceFleets": [
                {
                    "InstanceFleetType": "MASTER",
                    "Name": "MASTER",   
                    "TargetOnDemandCapacity": 1,
                    "InstanceTypeConfigs": [
                        {
                            "InstanceType": "m4.xlarge"
                        }
                    ]
                },
                {
                    "InstanceFleetType": "CORE",
                    "Name": "CORE",
                    "TargetOnDemandCapacity": 1,
                    "InstanceTypeConfigs": [
                        {
                            "InstanceType": "m4.xlarge"
                        }
                    ]
                }
            ]
        }
    },
    "End": true
}
```

The following includes a `Task` state that enables termination protection\. 

```
"Enable_Termination_Protection": {
    "Type": "Task",
    "Resource": "arn:aws:states:::elasticmapreduce:setClusterTerminationProtection",
    "Parameters": {
        "ClusterId.$": "$.ClusterId",
        "TerminationProtected": true
    },
    "End": true
}
```

The following includes a `Task` state that submits a step to a cluster\. 

```
"Step_One": {
    "Type": "Task",
    "Resource": "arn:aws:states:::elasticmapreduce:addStep.sync",
    "Parameters": {
        "ClusterId.$": "$.ClusterId",
        "Step": {
            "Name": "The first step",
            "ActionOnFailure": "CONTINUE",
            "HadoopJarStep": {
                "Jar": "command-runner.jar",
                "Args": [
                    "hive-script",
                    "--run-hive-script",
                    "--args",
                    "-f",
                    "s3://<region>.elasticmapreduce.samples/cloudfront/code/Hive_CloudFront.q",
                    "-d",
                    "INPUT=s3://<region>.elasticmapreduce.samples",
                    "-d",
                    "OUTPUT=s3://<mybucket>/MyHiveQueryResults/"
                ]
            }
        }
    },
    "End": true
}
```

The following includes a `Task` state that cancels a step\. 

```
"Cancel_Step_One": {
    "Type": "Task",
    "Resource": "arn:aws:states:::elasticmapreduce:cancelStep",
    "Parameters": {
        "ClusterId.$": "$.ClusterId",
        "StepId.$": "$.AddStepsResult.StepId"
    },
    "End": true
}
```

The following includes a `Task` state that terminates a cluster\. 

```
"Terminate_Cluster": {
    "Type": "Task",
    "Resource": "arn:aws:states:::elasticmapreduce:terminateCluster.sync",
    "Parameters": {
        "ClusterId.$": "$.ClusterId"
    },
    "End": true
}
```

The following includes a `Task` state that scales a cluster up or down for an instance group\.

```
"ModifyInstanceGroupByName": {
    "Type": "Task",
    "Resource": "arn:aws:states:::elasticmapreduce:modifyInstanceGroupByName",
    "Parameters": {
        "ClusterId": "j-1234567890123",
        "InstanceGroupName": "MyCoreGroup",
        "InstanceGroup": {
            "InstanceCount": 8
        }
    },
    "End": true
}
```

The following includes a `Task` state that scales a cluster up or down for an instance fleet\.

```
"ModifyInstanceFleetByName": {
    "Type": "Task",
    "Resource": "arn:aws::states:::elasticmapreduce:modifyInstanceFleetByName",
    "Parameters": {
        "ClusterId": "j-1234567890123",
        "InstanceFleetName": "MyCoreFleet",
        "InstanceFleet": {
            "TargetOnDemandCapacity": 8,
           "TargetSpotCapacity": 0
        }
    },
    "End": true
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](service-integration-iam-templates.md)\.