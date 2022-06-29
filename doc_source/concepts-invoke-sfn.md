# Invoke AWS Step Functions from other services<a name="concepts-invoke-sfn"></a>

You can configure several other services to invoke state machines\. Depending on the state machine's [Workflow Type](concepts-standard-vs-express.md), you can invoke state machines synchronously or asynchronously\. To invoke state machines synchronously, use the `[StartSyncExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartSyncExecution.html)` API calls or Amazon API Gateway integration with Express Workflows\. With asynchronous invocation, Step Functions returns a response immediately\. Services that can be configured to invoke Step Functions include:
+  AWS Lambda, using the [`StartExecution`](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) call\. 
+  [Amazon API Gateway](https://docs.aws.amazon.com/step-functions/latest/dg/tutorial-api-gateway.html) 
+  [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) 
+  [AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/action-reference-StepFunctions.html) 
+  [AWS IoT Rules Engine](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html) 
+  [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/connect-stepfunctions.html) 

Step Functions invocations are governed by the `StartExecution` quota\. For more information, see:

+ [Quotas](limits-overview.md)
