# CloudWatch Events For Step Functions Execution Status Changes<a name="cw-events"></a>

Amazon CloudWatch Events is an AWS service that enables you to respond to state changes in an AWS resource\. You can use AWS Step Functions with CloudWatch Events in two ways\. 

You can configure Step Functions to emit CloudWatch Events when an execution status changes\. This enables you to monitor your workflows without having to constantly poll using the [https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html) API\. Based on changes in state machine executions you can use a CloudWatch Events target to start new state machine executions, call AWS Lambda functions, publish messages to Amazon Simple Notification Service \(Amazon SNS\) topics, and more\.

You can also configure a Step Functions state machine as a target in CloudWatch Events\. This enables you to trigger an execution of a Step Functions workflow in response to an event from another AWS service\.

For more information, see the [Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)\.

**Topics**
+ [Step Functions Event Examples](#cw-events-events)
+ [Step Functions as a Target](#cw-events-targets)

## Step Functions Event Examples<a name="cw-events-events"></a>

Step Functions supports sending events to CloudWatch Events when the following changes occur\.

**Topics**
+ [Execution Started](#cw-events-execution-started)
+ [Execution Succeeded](#cw-events-execution-succeeded)
+ [Execution Failed](#cw-events-execution-failed)
+ [Execution Timed Out](#cw-events-execution-timed-out)
+ [Execution Aborted](#cw-events-execution-aborted)

In each case, the `detail` section in the event data provides the same information as the [https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html) API\. The `status` field indicates the status of the execution at the time the event was sent, one of `RUNNING`, `SUCCEEDED`, `FAILED`, `TIMED_OUT`, or `ABORTED` depending on the event emitted\.

### Execution Started<a name="cw-events-execution-started"></a>

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
        "output": null
    }
}
```

### Execution Succeeded<a name="cw-events-execution-succeeded"></a>

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
        "output": "\"Hello World!\""
    }
}
```

### Execution Failed<a name="cw-events-execution-failed"></a>

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
        "output": null
    }
}
```

### Execution Timed Out<a name="cw-events-execution-timed-out"></a>

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
        "output": null
    }
}
```

### Execution Aborted<a name="cw-events-execution-aborted"></a>

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
        "output": null
    }
}
```

## Routing a Step Functions Event to a CloudWatch Events Target in the CloudWatch Events Console<a name="cw-events-targets"></a>

**Create a CloudWatch Events rule for Step Functions events**

1. On the **Details** page of a state machine, choose **Actions**, **Create CloudWatch event rule**\.  
![\[Create CloudWatch event rule\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/create-cw-event-rule.png)

   Alternatively, open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\. In the navigation pane, choose **Events**, **Rules**\.

1. Choose **Create rule**\.

1. Under **Event Source**, ensure **Event Pattern** is selected\. 

1. For **Service Name**, choose **Step Functions**\.

1. For **Event Type**, choose **Step Functions Execution Status Change**\.

1. Optionally, configure a specific status, state machine ARN, or execution ARN\.

1. Under **Targets**, choose **Add target** and select a target from the drop\-down\. For example, you could launch a Lambda function, or start an execution of a Step Functions state machine\.

1. Add detailed information for your specific target\.

1. Choose **Configure details**\. On the **Configure rule details** page, enter a **Name** and **Description**\.

1. Choose **Create rule**\.