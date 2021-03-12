# Handle Lambda service exceptions<a name="bp-lambda-serviceexception"></a>

AWS Lambda can occasionally experience transient service errors\. In this case, invoking Lambda results in a 500 error, such as `ServiceException`, `AWSLambdaException`, or `SdkClientException`\. As a best practice, proactively handle these exceptions in your state machine to `Retry` invoking your Lambda function, or to `Catch` the error\.

Lambda errors are reported as `Lambda.ErrorName`\. To retry a Lambda service exception error, you could use the following `Retry` code\.

```
"Retry": [ {
   "ErrorEquals": [ "Lambda.ServiceException", "Lambda.AWSLambdaException", "Lambda.SdkClientException"],
   "IntervalSeconds": 2,
   "MaxAttempts": 6,
   "BackoffRate": 2
} ]
```

**Note**  
Unhandled errors in Lambda are reported as `Lambda.Unknown` in the error output\. These include out\-of\-memory errors and function timeouts\. You can match on `Lambda.Unknown`, `States.ALL`, or `States.TaskFailed` to handle these errors\. When Lambda hits the maximum number of invocations, the error is `Lambda.TooManyRequestsException`\. For more information about Lambda `Handled` and `Unhandled` errors, see `FunctionError` in the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_ResponseSyntax)\. 

For more information, see the following:
+ [Retrying after an error](concepts-error-handling.md#error-handling-retrying-after-an-error)
+ [Handling Error Conditions Using a Step Functions State Machine](tutorial-handling-error-conditions.md)
+ [Lambda Invoke Errors](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_Errors)