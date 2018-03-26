# Activities<a name="concepts-activities"></a>

Activities are an AWS Step Functions concept that refers to a task to be performed by a *worker* that can be hosted on EC2, ECS, mobile devices—basically anywhere\.

**Topics**
+ [Creating an Activity](#programming-activities-creating)
+ [Writing a Worker](#programming-activities-coding)
+ [Example Activity Worker in Ruby](example-ruby-activity-worker.md)

## Creating an Activity<a name="programming-activities-creating"></a>

Activities are referred to by name\. An activity's name can be any string that adheres to the following rules:
+ It must be between 0 – 80 characters in length\.
+ It must be unique within your AWS account and region\.

Activities can be created with Step Functions in any of the following ways:
+ Call [CreateActivity](http://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateActivity.html) with the activity name\.
+ Using the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

**Note**  
Activities are not versioned and are expected to always be backwards compatible\. If you must make a backwards\-incompatible change to an activity definition, then a *new* activity should be created with Step Functions using a unique name\.

## Writing a Worker<a name="programming-activities-coding"></a>

Workers can be implemented in any language that can make AWS Step Functions API actions\. Workers should repeatedly poll for work by implementing the following pseudo\-code algorithm:

```
[taskToken, jsonInput] = GetActivityTask();
try {
     // Do some work...
     SendTaskSuccess(taskToken, jsonOutput);
 } catch (Exception e) {
     SendTaskFailure(taskToken, reason, errorCode);
}
```

### Sending Heartbeat Notifications<a name="programming-activities-send-heartbeat"></a>

States that have long\-running activities should provide a heartbeat timeout value to verify that the activity is still running successfully\.

If your activity has a heartbeat timeout value, the worker which implements it must send heartbeat updates to Step Functions\. To send a heartbeat notification from a worker, use the [SendTaskHeartbeat](http://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html) action\.