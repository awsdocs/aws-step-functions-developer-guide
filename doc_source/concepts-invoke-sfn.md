# Invoke AWS Step Functions from other services<a name="concepts-invoke-sfn"></a>

You can configure several other services to invoke state machines\. You can only invoke state machines asynchronously\. With asynchronous invocation, Step Functions returns a response immediately\. Services that can be configured to invoke Step Functions include:
+  AWS Lambda, using the [https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) call\. 
+  [Amazon API Gateway](https://docs.aws.amazon.com/step-functions/latest/dg/tutorial-api-gateway.html) 
+  [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) 
+ [AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/action-reference-StepFunctions.html) 
+  [AWS IoT Rules Engine](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html) 
+  [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/connect-stepfunctions.html) 

Step Functions invocations are governed by the `StartExecution` quota\. For more information, see:
+ [Quotas for Standard Workflows](limits.md)
+ [Quotas for Express Workflows](express-limits.md)