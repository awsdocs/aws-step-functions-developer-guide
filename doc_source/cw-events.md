# EventBridge \(CloudWatch Events\) for Step Functions execution status changes<a name="cw-events"></a>

Amazon EventBridge is an AWS service that enables you to respond to state changes in an AWS resource\. You can use AWS Step Functions with EventBridge in two ways\. 

You can configure Step Functions to emit EventBridge events when an execution status changes\. This enables you to monitor your workflows without having to constantly poll using the [https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html) API\. Based on changes in state machine executions you can use an EventBridge target to start new state machine executions, call AWS Lambda functions, publish messages to Amazon Simple Notification Service \(Amazon SNS\) topics, and more\.

You can also configure a Step Functions state machine as a target in EventBridge\. This enables you to trigger an execution of a Step Functions workflow in response to an event from another AWS service\.

For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/) \.

## EventBridge payloads<a name="cw-events-payloads"></a>

An EventBridge event can contain an input property in its definition\. For some events, an EventBridge event can also contain an output property in its definition\. 
+ If the combined escaped input and escaped output sent to EventBridge exceeds 248KB, then the input will be excluded\. Similarly, if the escaped output exceeds 248KB, then the output will be excluded\. This is a result of the EventBridge events quotas\.
+  You can determine whether a payload has been truncated with the `inputDetails` and `outputDetails` properties\. For more information, see the [`CloudWatchEventsExecutionDataDetails` Data Type](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CloudWatchEventsExecutionDataDetails.html)\. 
+  For Standard Workflows, you can see the full input and output by using [DescribeExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html)\. 
+  `DescribeExecution` is not available for Express Workflows\. If you want to see the full input/output, you can wrap your Express Workflow with a Standard Workflow\. Another option is to use Amazon S3 ARNs\. For information about using ARNs, see [Use Amazon S3 ARNs instead of passing large payloads](avoid-exec-failures.md)\. 

**Topics**
+ [EventBridge payloads](#cw-events-payloads)
+ [Step Functions event examples](#cw-events-events)
+ [Step Functions as a Target](#cw-events-targets)

## Step Functions event examples<a name="cw-events-events"></a>

The following are examples of Step Functions sending events to EventBridge:

**Topics**
+ [Execution started](#cw-events-execution-started)
+ [Execution succeeded](#cw-events-execution-succeeded)
+ [Execution failed](#cw-events-execution-failed)
+ [Execution timed out](#cw-events-execution-timed-out)
+ [Execution aborted](#cw-events-execution-aborted)

In each case, the `detail` section in the event data provides the same information as the [https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html) API\. The `status` field indicates the status of the execution at the time the event was sent, one of `RUNNING`, `SUCCEEDED`, `FAILED`, `TIMED_OUT`, or `ABORTED` depending on the event emitted\.

### Execution started<a name="cw-events-execution-started"></a>

```
{
    "version": "0",
    "id": "315c1398-40ff-a850-213b-158f73e60175",
    "detail-type": "Step Functions Execution Status Change",
    "source": "aws.states",
    "account": "012345678912",
    "time": "2019-02-26T19:42:21Z",
    "region": "us-east-1",
    "resources": [
      "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name"
    ],
    "detail": {
        "executionArn": "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name",
        "stateMachineArn": "arn:aws:states:us-east-1:012345678912:stateMachine:state-machine",
        "name": "execution-name",
        "status": "RUNNING",
        "startDate": 1551225271984,
        "stopDate":  null,
        "input": "{}",
        "inputDetails": {
             "included": true
        },
        "output": null,
        "outputDetails": null
    }
}
```

### Execution succeeded<a name="cw-events-execution-succeeded"></a>

```
{
    "version": "0",
    "id": "315c1398-40ff-a850-213b-158f73e60175",
    "detail-type": "Step Functions Execution Status Change",
    "source": "aws.states",
    "account": "012345678912",
    "time": "2019-02-26T19:42:21Z",
    "region": "us-east-1",
    "resources": [
      "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name"
    ],
    "detail": {
        "executionArn": "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name",
        "stateMachineArn": "arn:aws:states:us-east-1:012345678912:stateMachine:state-machine",
        "name": "execution-name",
        "status": "SUCCEEDED",
        "startDate": 1547148840101,
        "stopDate": 1547148840122,
        "input": "{}",
        "inputDetails": {
             "included": true
        },
        "output": "\"Hello World!\"",
        "outputDetails": {
             "included": true
        }
    }
}
```

### Execution failed<a name="cw-events-execution-failed"></a>

```
{
    "version": "0",
    "id": "315c1398-40ff-a850-213b-158f73e60175",
    "detail-type": "Step Functions Execution Status Change",
    "source": "aws.states",
    "account": "012345678912",
    "time": "2019-02-26T19:42:21Z",
    "region": "us-east-1",
    "resources": [
      "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name"
    ],
    "detail": {
        "executionArn": "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name",
        "stateMachineArn": "arn:aws:states:us-east-1:012345678912:stateMachine:state-machine",
        "name": "execution-name",
        "status": "FAILED",
        "startDate": 1551225146847,
        "stopDate": 1551225151881,
        "input": "{}",
        "inputDetails": {
             "included": true
        },
        "output": null,
        "outputDetails": null
    }
}
```

### Execution timed out<a name="cw-events-execution-timed-out"></a>

```
{
    "version": "0",
    "id": "315c1398-40ff-a850-213b-158f73e60175",
    "detail-type": "Step Functions Execution Status Change",
    "source": "aws.states",
    "account": "012345678912",
    "time": "2019-02-26T19:42:21Z",
    "region": "us-east-1",
    "resources": [
      "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name"
    ],
    "detail": {
        "executionArn": "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name",
        "stateMachineArn": "arn:aws:states:us-east-1:012345678912:stateMachine:state-machine",
        "name": "execution-name",
        "status": "TIMED_OUT",
        "startDate": 1551224926156,
        "stopDate": 1551224927157,
        "input": "{}",
        "inputDetails": {
             "included": true
        },
        "output": null,
        "outputDetails": null
    }
}
```

### Execution aborted<a name="cw-events-execution-aborted"></a>



```
{
    "version": "0",
    "id": "315c1398-40ff-a850-213b-158f73e60175",
    "detail-type": "Step Functions Execution Status Change",
    "source": "aws.states",
    "account": "012345678912",
    "time": "2019-02-26T19:42:21Z",
    "region": "us-east-1",
    "resources": [
      "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name"
    ],
    "detail": {
        "executionArn": "arn:aws:states:us-east-1:012345678912:execution:state-machine-name:execution-name",
        "stateMachineArn": "arn:aws:states:us-east-1:012345678912:stateMachine:state-machine",
        "name": "execution-name",
        "status": "ABORTED",
        "startDate": 1551225014968,
        "stopDate": 1551225017576,
        "input": "{}",
        "inputDetails": {
             "included": true
        },
        "output": null,
        "outputDetails": null
    }
}
```

## Routing a Step Functions event to EventBridge in the EventBridge console<a name="cw-events-targets"></a>

1. On the **Details** page of a state machine, choose **Actions**, **Create EventBridge \(CloudWatch Events\) event rule**\.  
![\[Create EventBridge event rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/create-cw-event-rule.png)

   Alternatively, open the EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\. In the navigation pane, choose **Events**, **Rules**\.

1. Choose **Create rule**\.

1. Under **Event Source**, ensure **Event Pattern** is selected\. 

1. For **Service Name**, choose **Step Functions**\.

1. For **Event Type**, choose **Step Functions Execution Status Change**\.

1. Optionally, configure a specific status, state machine Amazon Resource Name \(ARN\), or execution ARN\.

1. Under **Targets**, choose **Add target**, and select a target from the list\. For example, you could launch a Lambda function, or start an execution of a Step Functions state machine\.

1. Add detailed information for your specific target\.

1. Choose **Configure details**\. On the **Configure rule details** page, enter a **Name** and **Description**\.

1. Choose **Create rule**\.