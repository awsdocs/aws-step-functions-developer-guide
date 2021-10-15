# Callback Pattern Example \(Amazon SQS, Amazon SNS, Lambda\)<a name="callback-task-sample-sqs"></a>

This sample project demonstrates how to have AWS Step Functions pause during a task, and wait for an external process to return a task token that was generated when the task started\. 

When this sample project is deployed and an execution is started, the following steps occur: 

1. Step Functions passes a message that includes a task token to an Amazon Simple Queue Service \(Amazon SQS\) queue\.

1. Step Functions then pauses, waiting for that token to be returned\.

1. The Amazon SQS queue triggers an AWS Lambda function that calls [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html) with that same task token\.

1. When the task token is received, the workflow continues\.

1. The `"Notify Success"` task publishes an Amazon Simple Notification Service \(Amazon SNS\) message that the callback was received\.

To learn how to implement the callback pattern in Step Functions, see [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token)\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

## Create State Machine and Provision Resources<a name="callback-task-sample-sqs-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Callback Pattern Example**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Callback example workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-callback-example.png)

1. Choose **Create Sample Project**\.

   The **Create Project Resources** page is displayed, listing the resources that will be created\. For this sample project the resources include:
   + An Amazon SQS message queue\.
   + A Lambda function, that calls the Step Functions API action [https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html)\.
   + An Amazon SNS topic to notify success or failure when the workflow can continues\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Create Project Resources** page displays **Creating resources**, you can open the **Stack ID:** link to see which resources are being provisioned\.

   When complete, the **New execution** page is displayed\.

1. \(Optional\) Enter an execution name, and sample input\.  
![\[Start callback execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-callback-execution.png)

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.  
![\[Callback execution\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-callback-success.png)

1. To review how Step Functions progressed through the workflow and received a callback from Amazon SQS, review the entries in the **Execution event history**\.

## Lambda Callback Example<a name="call-back-lambda-example"></a>

To see how the components of this sample project work together, see the resources that were deployed in your AWS account\. For example, here is the Lambda function that calls Step Functions with the task token\.

```
console.log('Loading function');
const aws = require('aws-sdk');

exports.lambda_handler = (event, context, callback) => {
    const stepfunctions = new aws.StepFunctions();

    for (const record of event.Records) {
        const messageBody = JSON.parse(record.body);
        const taskToken = messageBody.TaskToken;

        const params = {
            output: "\"Callback task completed successfully.\"",
            taskToken: taskToken
        };

        console.log(`Calling Step Functions to complete callback task with params ${JSON.stringify(params)}`);

        stepfunctions.sendTaskSuccess(params, (err, data) => {
            if (err) {
                console.error(err.message);
                callback(err.message);
                return;
            }
            console.log(data);
            callback(null);
        });
    }
};
```