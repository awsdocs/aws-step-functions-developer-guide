# EventBridge \(CloudWatch Events\) for Step Functions execution status changes<a name="cw-events"></a>

Amazon EventBridge is an AWS service that enables you to respond to state changes in an AWS resource\. You can use AWS Step Functions Standard Workflows with EventBridge, while Express Workflows do not emit events to EventBridge\. There are two ways to use Step Functions Standard Workflows with EventBridge\.

You can configure EventBridge rules to react to events that are emitted when the execution status of a Step Functions state machine changes\. This enables you to monitor your workflows without having to constantly poll using the [https://docs.aws.amazon.com/https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html](https://docs.aws.amazon.com/https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html) API\. Based on changes in state machine executions, you can use an EventBridge target to start new state machine executions, call AWS Lambda functions, publish messages to Amazon Simple Notification Service \(Amazon SNS\) topics, and more\.

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
+ [Routing a Step Functions event to EventBridge](#cw-events-targets)

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

Use the following instructions to learn how to trigger the execution of a Step Functions state machine whenever a specific Step Functions state machine completes running successfully\. You use the Amazon EventBridge console to specify the state machine whose execution you want to trigger\.

1. On the **Details** page of a state machine, choose **Actions**, and then choose **Create EventBridge \(CloudWatch Events\) rule**\.  
![\[Create EventBridge event rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/create-cw-event-rule.png)

   Alternatively, open the EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\. In the navigation pane, choose **Rules** under **Events**\.

1. Choose **Create rule**\.

1. Enter a **Name** for your rule \(for example, `StepFunctionsEventRule`\) and optionally enter a **Description** for the rule\.

1. For **Event bus** and **Rule type**, keep the default selections\.

1. Choose **Next**\.

1. Under **Event Source**, keep the default selection of **AWS events or EventBridge partner events**\. 

1. Under **Event pattern**, do the following:

   1. In the **Event source** dropdown list, keep the default selection of **AWS services**\.

   1. From the **AWS service** dropdown list, choose **Step Functions**\.

   1. From the **Event Type** dropdown list, choose **Step Functions Execution Status Change**\.

   1. \(Optional\) Configure a specific status, state machine Amazon Resource Name \(ARN\), or execution ARN\. For this procedure, choose **Specific status\(es\)**, and then choose **SUCCEEDED** from the dropdown list\.

1. Choose **Next**\.

1. On the **Select target\(s\)** page, keep the default selection of **AWS service**\.

1. From the **Select a target** dropdown list, choose an AWS service\. For example, you could launch a Lambda function, or run a Step Functions state machine\. For this procedure, choose **Step Functions state machine**\.

1. From the **State machine** dropdown list, choose a state machine\.

1. Under **Execution role**, keep the default selection of **Create a new role for this specific resource**\.

1. Choose **Next**, and then choose **Next** again\.

1. On the **Review and create** page, review the details of the rule and choose **Create rule**\.

   The rule is created and the **Rules** page is displayed, listing all your Amazon EventBridge rules\.