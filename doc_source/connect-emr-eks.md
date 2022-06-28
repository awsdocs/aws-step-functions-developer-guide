# Call Amazon EMR on EKS with AWS Step Functions<a name="connect-emr-eks"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized Amazon EMR on EKS integration is different than the Amazon EMR on EKS AWS SDK integration**  
The [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern is supported\.
There are no optimizations for the [Request Response](connect-to-resource.md#connect-default) integration pattern\.
The [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration pattern is not supported\.

To integrate AWS Step Functions with Amazon EMR on EKS, use the Amazon EMR on EKS service integration APIs\. The service integration APIs are the same as the corresponding Amazon EMR on EKS APIs, but not all APIs support all integration patterns, as shown in the following table\.


| API | Request response | Run a job \(\.sync\) | 
| --- | --- | --- | 
| CreateVirtualCluster | ✓ |  | 
| DeleteVirtualCluster | ✓ | ✓ | 
| StartJobRun | ✓ | ✓ | 

Supported Amazon EMR on EKS APIs:

**Note**  
There is a quota for the maximum input or result data size for a task in Step Functions\. This restricts you to 262,144 bytes of data as a UTF\-8 encoded string when you send to, or receive data from, another service\. See [Quotas related to state machine executions](limits-overview.md#service-limits-state-machine-executions)\.


+ [https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_CreateVirtualCluster.html](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_CreateVirtualCluster.html)
  + [Request syntax](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_CreateVirtualCluster.html#API_CreateVirtualCluster_RequestSyntax)
  + [Supported parameters](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_CreateVirtualCluster.html#API_CreateVirtualCluster_RequestBody)
  + [Response syntax](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_CreateVirtualCluster.html#API_CreateVirtualCluster_ResponseSyntax)
+ [https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_DeleteVirtualCluster.html](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_DeleteVirtualCluster.html)
  + [Request syntax](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_DeleteVirtualCluster.html#API_DeleteVirtualCluster_RequestSyntax)
  + [Supported parameters](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_DeleteVirtualCluster.html#API_DeleteVirtualCluster_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_CreateVirtualCluster.html#API_CreateVirtualCluster_ResponseSyntax)
+ [https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_StartJobRun.html](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_StartJobRun.html)
  + [Request syntax](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_StartJobRun.html#API_StartJobRun_RequestSyntax)
  + [Supported parameters](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_StartJobRun.html#API_StartJobRun_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/emr-on-eks/latest/APIReference/API_StartJobRun.html#API_StartJobRun_ResponseSyntax)

The following includes a `Task` state that creates a virtual cluster\.

```
"Create_Virtual_Cluster": {
  "Type": "Task",
  "Resource": "arn:aws:states:::emr-containers:createVirtualCluster",
  "Parameters": {
    "Name": "MyVirtualCluster",
    "ContainerProvider": {
      "Id": "EKSClusterName",
      "Type": "EKS",
      "Info": {
        "EksInfo": {
          "Namespace": "Namespace"
        }
      }
    }
  },
  "End": true
}
```

The following includes a `Task` state that submits a job to a virtual cluster and waits for it to complete\.

```
"Submit_Job": {
  "Type": "Task",
  "Resource": "arn:aws:states:::emr-containers:startJobRun.sync",
  "Parameters": {
    "Name": "MyJobName",
    "VirtualClusterId.$": "$.VirtualClusterId",
    "ExecutionRoleArn": "arn:aws:iam::<accountId>:role/job-execution-role",
    "ReleaseLabel": "emr-6.2.0-latest",
    "JobDriver": {
      "SparkSubmitJobDriver": {
        "EntryPoint": "s3://<mybucket>/jobs/trip-count.py",
        "EntryPointArguments": [
          "60"
        ],
        "SparkSubmitParameters": "--conf spark.driver.cores=2 --conf spark.executor.instances=10 --conf spark.kubernetes.pyspark.pythonVersion=3 --conf spark.executor.memory=10G --conf spark.driver.memory=10G --conf spark.executor.cores=1 --conf spark.dynamicAllocation.enabled=false"
      }
    },
    "ConfigurationOverrides": {
      "ApplicationConfiguration": [
        {
          "Classification": "spark-defaults",
          "Properties": {
            "spark.executor.instances": "2",
            "spark.executor.memory": "2G"
          }
        }
      ],
      "MonitoringConfiguration": {
        "PersistentAppUI": "ENABLED",
        "CloudWatchMonitoringConfiguration": {
          "LogGroupName": "MyLogGroupName",
          "LogStreamNamePrefix": "MyLogStreamNamePrefix"
        },
        "S3MonitoringConfiguration": {
          "LogUri": "s3://<mylogsbucket>"
        }
      }
    }
  },
  "End": true
}
```

The following includes a `Task` state that deletes a virtual cluster and waits for the deletion to complete\.

```
"Delete_Virtual_Cluster": {
  "Type": "Task",
  "Resource": "arn:aws:states:::emr-containers:deleteVirtualCluster.sync",
  "Parameters": {
    "Id.$": "$.VirtualClusterId"
  },
  "End": true
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.