# Handle Lambda Service Exceptions<a name="bp-lambda-serviceexception"></a>

AWS Lambda can occasionally experience transient service errors\. In this case, invoking Lambda will result in a 500 error such as `ServiceException`, `AWSLambdaException`, or `SdkClientException`\. As a best practice, proactively handle these exceptions in your state machine to `Retry` invoking your Lambda function, or to `Catch` the error\.

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
Unhandled errors in Lambda are reported as `Lambda.Unknown` in the error output\. These include out\-of\-memory errors, function timeouts, and hitting the concurrent Lambda invoke limit\. You can match on `Lambda.Unknown`, `States.ALL`, or `States.TaskFailed` to handle these errors\. For more information about Lambda `Handled` and `Unhandled` errors, see `FunctionError` in the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_ResponseSyntax)\. 

For more information, see:
+ [Retrying after an Error](concepts-error-handling.md#error-handling-retrying-after-an-error)
+ [Handling Error Conditions Using a State Machine](tutorial-handling-error-conditions.md)
+ [Lambda Invoke Errors](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_Errors)