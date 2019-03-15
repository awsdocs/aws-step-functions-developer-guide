# Errors<a name="amazon-states-language-errors"></a>

Any state can encounter runtime errors\. Errors can arise because of state machine definition issues \(for example, no matching rule in a `Choice` state\), task failures \(for example, an exception from an AWS Lambda function\) or because of transient issues, such as network partition events\. When a state reports an error, the default course of action for AWS Step Functions is to fail the execution entirely\.

## Error Representation<a name="amazon-states-language-error-names"></a>

Errors are identified in the Amazon States Language by case\-sensitive strings, called Error Names\. The Amazon States Language defines a set of built\-in strings naming well\-known errors, all of which begin with the prefix `"States."`:

Predefined Error Codes

** `States.ALL` **  
A wildcard that matches any Error Name\.

** `States.Timeout` **  
A `Task` state either ran longer than the `"TimeoutSeconds"` value, or failed to send a heartbeat for a time longer than the `"HeartbeatSeconds"` value\.

** `States.TaskFailed` **  
A `Task` state failed during the execution\.

** `States.Permissions` **  
A `Task` state failed because it had insufficient privileges to execute the specified code\.

States can report errors with other names, which must not begin with the prefix `"States."`\.

## Retrying After an Error<a name="amazon-states-language-retrying-after-error"></a>

 `Task` and `Parallel` states can have a field named `Retry`, whose value must be an array of objects, called Retriers\. An individual Retrier represents a certain number of retries, usually at increasing time intervals\.

**Note**  
Retries are treated as state transitions\. For information about how state transitions affect billing, see [Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/)\.

A Retrier contains the following fields\.

** `ErrorEquals` \(Required\)**  
A non\-empty array of strings that match Error Names\. When a state reports an error, Step Functions scans through the Retriers and, when the Error Name appears in this array, it implements the retry policy described in this Retrier\.

** `IntervalSeconds` \(Optional\)**  
An integer that represents the number of seconds before the first retry attempt \(default 1\)\.

** `MaxAttempts` \(Optional\)**  
A positive integer, representing the maximum number of retry attempts \(default 3\)\. If the error recurs more times than specified, retries cease and normal error handling resumes\. A value of 0 \(zero\) is permitted and indicates that the error or errors should never be retried\.

** `BackoffRate` \(Optional\)**  
A number that is the multiplier by which the retry interval increases on each attempt \(default is 2\.0\)\.

Here is an example of a Retry field that will make four retry attempts after waits of 3, 4\.5, 6\.75 and 10\.125 seconds\.

```
"Retry" : [
    {
      "ErrorEquals": [ "States.Timeout" ],
      "IntervalSeconds": 3,
      "MaxAttempts": 4,
      "BackoffRate": 1.5
    }
]
```

The reserved name `States.ALL` appearing in a Retrier's `ErrorEquals` field is a wildcard that matches any Error Name\. It must appear alone in the `ErrorEquals` array and must appear in the last Retrier in the `Retry` array\.

Here is an example of a `Retry` field that will retry any error except for `States.Timeout`\.

```
"Retry" : [
  {
    "ErrorEquals": [ "States.Timeout" ],
    "MaxAttempts": 0
  },
  {
    "ErrorEquals": [ "States.ALL" ]
  }
]
```

### Complex Retry Scenarios<a name="amazon-states-language-complex-retry-scenarios"></a>

A Retrier's parameters apply across all visits to that Retrier in the context of a single state execution\. This is best illustrated by an example\. Consider the following `Task` state:

```
"X": {
  "Type": "Task",
  "Resource": "arn:aws:states:us-east-1:123456789012:task:X",
  "Next": "Y",
  "Retry": [
    {
      "ErrorEquals": [ "ErrorA", "ErrorB" ],
      "IntervalSeconds": 1,
      "BackoffRate": 2.0,
      "MaxAttempts": 2
    },
    {
      "ErrorEquals": [ "ErrorC" ],
      "IntervalSeconds": 5
    }
  ],
  "Catch": [
    {
      "ErrorEquals": [ "States.ALL" ],
      "Next": "Z"
    }
  ]
}
```

Suppose that this task fails five successive times, outputting Error Names "`ErrorA`", "`ErrorB`", "`ErrorC`", "`ErrorB`", and "`ErrorB`"\. The first two errors match the first retrier, and cause waits of one and two seconds\. The third error matches the second retrier, and causes a wait of five seconds\. The fourth error matches the first retrier and causes a wait of four seconds\. The fifth error also matches the first retrier, but it has already reached its limit of two retries \("`MaxAttempts`"\) for that particular error \("`ErrorB`"\) so it fails and execution is redirected to the `"Z"` state via the `"Catch"` field\.

Note that once the system transitions to another state, no matter how, all Retrier parameters are reset\.

**Note**  
You can generate custom error names \(such as `ErrorA` and `ErrorB` above\) using either an [activity](amazon-states-language-task-state.md#amazon-states-language-task-state-activity) or [Lambda functions](amazon-states-language-task-state.md#amazon-states-language-task-state-lambda)\. For more information, see [Handling Error Conditions Using a State Machine](tutorial-handling-error-conditions.md)\.

## Fallback States<a name="amazon-states-language-fallback-states"></a>

 `Task` and `Parallel` states can have a field named `Catch`, whose value must be an array of objects, called Catchers\.

A Catcher contains the following fields\.

** `ErrorEquals` \(Required\)**  
A non\-empty array of strings that match Error Names, specified exactly as with the Retrier field of the same name\.

** `Next` \(Required\)**  
A string that must exactly match one of the state machine's state names\.

** `ResultPath` \(Optional\)**  
A [path](amazon-states-language-input-output-processing.md) that determines what is sent as input to the state specified by the `Next` field\.

When a state reports an error and either there is no `Retry` field, or retries have failed to resolve the error, AWS Step Functions scans through the Catchers in the order listed in the array\. When the Error Name appears in the value of a Catcher's `ErrorEquals` field, the state machine transitions to the state named in the `Next` field\.

The reserved name `States.ALL` appearing in a Catcher's `ErrorEquals` field is a wildcard that matches any Error Name\. It must appear alone in the `ErrorEquals` array and must appear in the last Catcher in the `Catch` array\.

Here is an example of a `Catch` field that will transition to the state named "RecoveryState" when a Lambda function outputs an unhandled Java exception, and otherwise to the `"EndState"` state\.

```
"Catch": [
  {
    "ErrorEquals": [ "java.lang.Exception" ],
    "ResultPath": "$.error-info",
    "Next": "RecoveryState"
  },
  {
    "ErrorEquals": [ "States.ALL" ],
    "Next": "EndState"
  }
]
```

Each Catcher can specify multiple errors to handle\.

When Step Functions transitions to the state specified in a Catcher, it sends along as input JSON text that is different from what it would normally send to the next state when there was no error\. This JSON text represents an object containing a field `"Error"` whose value is a string containing the error name\. The object will also, usually, contain a field `"Cause"` that has a human\-readable description of the error\. We refer to this object as the Error Output\.

In this example, the first Catcher contains a `ResultPath` field\. This works in a similar fashion to a `ResultPath` field in a state's top level—it takes the results of executing the state and overwrites a portion of the state's input, or all of the state's input, or it takes the results and adds them to the input\. In the case of an error handled by a Catcher, the result of executing the state is the Error Output\.

So in the example, for the first Catcher the Error Output will be added to the input as a field named `error-info` \(assuming there isn't already a field by that name in the input\) and the entire input will be sent to `RecoveryState`\. For the second Catcher, the Error Output will overwrite the input and so just the Error Output will be sent to `EndState`\. When not specified, the `ResultPath` field defaults to `$` which selects, and so overwrites, the entire input\.

When a state has both Retry and Catch fields, Step Functions uses any appropriate Retriers first and only applies the matching Catcher transition if the retry policy fails to resolve the error\.