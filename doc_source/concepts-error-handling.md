# Error Handling<a name="concepts-error-handling"></a>

Any state can encounter runtime errors\. Errors can happen for various reasons:
+ State machine definition issues \(for example, no matching rule in a `Choice` state\)
+ Task failures \(for example, an exception in a Lambda function\)
+ Transient issues \(for example, network partition events\)

By default, when a state reports an error, AWS Step Functions causes the execution to fail entirely\.

## Error Names<a name="error-handling-error-representation"></a>

Step Functions identifies errors in the Amazon States Language using case\-sensitive strings, known as *error names*\. The Amazon States Language defines a set of built\-in strings that name well\-known errors, all beginning with the `States.` prefix\.

** `States.ALL` **  
A wildcard that matches any known error name\.

** `States.Timeout` **  
A `Task` state either ran longer than the `TimeoutSeconds` value, or failed to send a heartbeat for a period longer than the `HeartbeatSeconds` value\.

** `States.TaskFailed` **  
A `Task` state failed during the execution\.

** `States.Permissions` **  
A `Task` state failed because it had insufficient privileges to execute the specified code\.

States can report errors with other names\. However, these must not begin with the `States.` prefix\.

As a best practice, ensure production code can handle AWS Lambda service exceptions \(`Lambda.ServiceException` and `Lambda.SdkclientException`\)\. For more information, see [Handle Lambda Service Exceptions](bp-lambda-serviceexception.md)\.

**Note**  
Unhandled errors in Lambda are reported as `Lambda.Unknown` in the error output\. These include out\-of\-memory errors, function timeouts, and hitting the concurrent Lambda invoke limit\. You can match on `Lambda.Unknown`, `States.ALL`, or `States.TaskFailed` to handle these errors\. For more information about Lambda `Handled` and `Unhandled` errors, see `FunctionError` in the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_ResponseSyntax)\. 

## Retrying after an Error<a name="error-handling-retrying-after-an-error"></a>

 `Task` and `Parallel` states can have a field named `Retry`, whose value must be an array of objects known as *retriers*\. An individual retrier represents a certain number of retries, usually at increasing time intervals\.

**Note**  
Retries are treated as state transitions\. For information about how state transitions affect billing, see [Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/)\.

A retrier contains the following fields\.

** `ErrorEquals` \(Required\)**  
A non\-empty array of strings that match error names\. When a state reports an error, Step Functions scans through the retriers\. When the error name appears in this array, it implements the retry policy described in this retrier\.

** `IntervalSeconds` \(Optional\)**  
An integer that represents the number of seconds before the first retry attempt \(`1` by default\)\.

** `MaxAttempts` \(Optional\)**  
A positive integer that represents the maximum number of retry attempts \(`3` by default\)\. If the error recurs more times than specified, retries cease and normal error handling resumes\. A value of `0` specifies that the error or errors are never retried\.

** `BackoffRate` \(Optional\)**  
The multiplier by which the retry interval increases during each attempt \(`2.0` by default\)\.

This example of a `Retry` makes 2 retry attempts after waiting for 3 and 4\.5 seconds\.

```
"Retry": [ {
   "ErrorEquals": [ "States.Timeout" ],
   "IntervalSeconds": 3,
   "MaxAttempts": 2,
   "BackoffRate": 1.5
} ]
```

The reserved name `States.ALL` that appears in a retrier's `ErrorEquals` field is a wildcard that matches any error name\. It must appear alone in the `ErrorEquals` array and must appear in the last retrier in the `Retry` array\.

This example of a `Retry` field retries any error except `States.Timeout`\.

```
"Retry": [ {
   "ErrorEquals": [ "States.Timeout" ],
   "MaxAttempts": 0
}, {
   "ErrorEquals": [ "States.ALL" ]
} ]
```

### Complex Retry Scenarios<a name="error-handling-complex-retry-scenarios"></a>

A retrier's parameters apply across all visits to the retrier in the context of a single\-state execution\. 

Consider the following `Task` state:

```
"X": {
   "Type": "Task",
   "Resource": "arn:aws:states:us-east-1:123456789012:task:X",
   "Next": "Y",
   "Retry": [ {
      "ErrorEquals": [ "ErrorA", "ErrorB" ],
      "IntervalSeconds": 1,
      "BackoffRate": 2.0,
      "MaxAttempts": 2
   }, {
      "ErrorEquals": [ "ErrorC" ],
      "IntervalSeconds": 5
   } ],
   "Catch": [ {
      "ErrorEquals": [ "States.ALL" ],
      "Next": "Z"
   } ]
}
```

This task fails five times in succession, outputting these error names: `ErrorA`, `ErrorB`, `ErrorC`, `ErrorB`, and `ErrorB`\. The following occurs as a result:
+ The first two errors match the first retrier and cause waits of 1 and 2 seconds\.
+ The third error matches the second retrier and causes a wait of 5 seconds\.
+ The fourth error matches the first retrier and causes a wait of 4 seconds\.
+ The fifth error also matches the first retrier\. However, it has already reached its limit of two retries \(`MaxAttempts`\) for that particular error \(`ErrorB`\), so it fails and execution is redirected to the `Z` state via the `Catch` field\.

## Fallback States<a name="error-handling-fallback-states"></a>

 `Task` and `Parallel` states can have a field named `Catch`\. This field's value must be an array of objects, known as *catchers*\.

A catcher contains the following fields\.

** `ErrorEquals` \(Required\)**  
A non\-empty array of strings that match error names, specified exactly as they are with the retrier field of the same name\.

** `Next` \(Required\)**  
A string that must exactly match one of the state machine's state names\.

** `ResultPath` \(Optional\)**  
A [path](concepts-input-output-filtering.md) that determines what input is sent to the state specified in the `Next` field\.

When a state reports an error and either there is no `Retry` field, or if retries fail to resolve the error, Step Functions scans through the catchers in the order listed in the array\. When the error name appears in the value of a catcher's `ErrorEquals` field, the state machine transitions to the state named in the `Next` field\.

The reserved name `States.ALL` that appears in a catcher's `ErrorEquals` field is a wildcard that matches any error name\. It must appear alone in the `ErrorEquals` array and must appear in the last catcher in the `Catch` array\.

The following example of a `Catch` field transitions to the state named `RecoveryState` when a Lambda function outputs an unhandled Java exception\. Otherwise, the field transitions to the `EndState` state\.

```
"Catch": [ {
   "ErrorEquals": [ "java.lang.Exception" ],
   "ResultPath": "$.error-info",
   "Next": "RecoveryState"
}, {
   "ErrorEquals": [ "States.ALL" ],
   "Next": "EndState"
} ]
```

**Note**  
Each catcher can specify multiple errors to handle\.

### Error Output<a name="error-handling-error-output"></a>

When Step Functions transitions to the state specified in a catch name, the object usually contains the field `Cause`\. This field's value is a human\-readable description of the error\. This object is known as the *error output*\.

In this example, the first catcher contains a `ResultPath` field\. This works similarly to a `ResultPath` field in a state's top level, resulting in two possibilities:
+ It takes the results of executing the state and overwrites a portion of the state's input \(or all of the state's input\)\.
+ It takes the results and adds them to the input\. In the case of an error handled by a catcher, the result of executing the state is the error output\.

Thus, in this example, for the first catcher the error output is added to the input as a field named `error-info` \(if there isn't already a field with this name in the input\)\. Then, the entire input is sent to `RecoveryState`\. For the second catcher, the error output overwrites the input and only the error output is sent to `EndState`\.

**Note**  
If you don't specify the `ResultPath` field, it defaults to `$`, which selects and overwrites the entire input\.

When a state has both `Retry` and `Catch` fields, Step Functions uses any appropriate retriers first, and only afterward applies the matching catcher transition if the retry policy fails to resolve the error\.

## Examples Using Retry and Using Catch<a name="error-handling-examples"></a>

The state machines defined in the following examples assume the existence of two Lambda functions: one that always fails and one that waits long enough to allow a timeout defined in the state machine to occur\.

This is a definition of a Lambda function that always fails, returning the message `error`\. In the state machine examples that follow, this Lambda function is named `FailFunction`\.

```
exports.handler = (event, context, callback) => {
    callback("error");
};
```

This is a definition of a Lambda function that sleeps for 10 seconds\. In the state machine examples that follow, this Lambda function is named `sleep10`\.

**Note**  
When you create this Lambda function in the Lambda console, remember to change the **Timeout** value in the **Advanced settings** section from 3 seconds \(default\) to 11 seconds\.

```
exports.handler = (event, context, callback) => {
    setTimeout(function(){
    }, 11000);
};
```

### Handling a Failure Using Retry<a name="error-handling-handling-failure-using-retry"></a>

This state machine uses a `Retry` field to retry a function that fails and outputs the error name `HandledError`\. The function is retried twice with an exponential backoff between retries\.

```
{
   "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
   "StartAt": "HelloWorld",
   "States": {
      "HelloWorld": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:FailFunction",
         "Retry": [ {
            "ErrorEquals": ["HandledError"],
            "IntervalSeconds": 1,
            "MaxAttempts": 2,
            "BackoffRate": 2.0
         } ],
      "End": true
      }
   }
}
```

This variant uses the predefined error code `States.TaskFailed`, which matches any error that a Lambda function outputs\.

```
{
   "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
   "StartAt": "HelloWorld",
   "States": {
      "HelloWorld": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:FailFunction",
         "Retry": [ {
            "ErrorEquals": ["States.TaskFailed"],
            "IntervalSeconds": 1,
            "MaxAttempts": 2,
            "BackoffRate": 2.0
         } ],
         "End": true
      }
   }
}
```

**Note**  
As a best practice, tasks that reference a Lambda function should handle Lambda service exceptions\. For more information, see [Handle Lambda Service Exceptions](bp-lambda-serviceexception.md)\. 

### Handling a Failure Using Catch<a name="error-handling-handling-failure-using-catch"></a>

This example uses a `Catch` field\. When a Lambda function outputs an error, the error is caught and the state machine transitions to the `fallback` state\.

```
{
   "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
   "StartAt": "HelloWorld",
   "States": {
      "HelloWorld": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:FailFunction",
         "Catch": [ {
            "ErrorEquals": ["HandledError"],
            "Next": "fallback"
         } ],
         "End": true
      },
      "fallback": {
         "Type": "Pass",
         "Result": "Hello, AWS Step Functions!",
         "End": true
      }
   }
}
```

This variant uses the predefined error code `States.TaskFailed`, which matches any error that a Lambda function outputs\.

```
{
   "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
   "StartAt": "HelloWorld",
   "States": {
      "HelloWorld": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:FailFunction",
         "Catch": [ {
            "ErrorEquals": ["States.TaskFailed"],
            "Next": "fallback"
         } ],
      "End": true
      },
      "fallback": {
         "Type": "Pass",
         "Result": "Hello, AWS Step Functions!",
         "End": true
      }
   }
}
```

### Handling a Timeout Using Retry<a name="error-handling-handling-timeout-using-retry"></a>

This state machine uses a `Retry` field to retry a function that times out\. The function is retried twice with an exponential backoff between retries\.

```
{
   "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
   "StartAt": "HelloWorld",
   "States": {
      "HelloWorld": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:sleep10",
         "TimeoutSeconds": 2,
         "Retry": [ {
            "ErrorEquals": ["States.Timeout"],
            "IntervalSeconds": 1,
            "MaxAttempts": 2,
            "BackoffRate": 2.0
         } ],
         "End": true
      }
   }
}
```

### Handling a Timeout Using Catch<a name="error-handling-handling-timeout-using-catch"></a>

This example uses a `Catch` field\. When a timeout occurs, the state machine transitions to the `fallback` state\.

```
{
   "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
   "StartAt": "HelloWorld",
   "States": {
      "HelloWorld": {
         "Type": "Task",
         "Resource": "arn:aws:lambda:us-east-1:123456789012:function:sleep10",
         "TimeoutSeconds": 2,
         "Catch": [ {
            "ErrorEquals": ["States.Timeout"],
            "Next": "fallback"
         } ],
         "End": true
      },
      "fallback": {
         "Type": "Pass",
         "Result": "Hello, AWS Step Functions!",
         "End": true
      }
   }
}
```

**Note**  
You can preserve the state input and the error by using `ResultPath`\. See [Use ResultPath to Include Both Error and Input in a `Catch`](input-output-resultpath.md#input-output-resultpath-catch)\.