# The Context Object<a name="input-output-contextobject"></a>

The context object is an internal JSON structure that is available during an execution\. It includes information about your state machine and execution that you can access from within the `"Parameters"` field of a state definition\. This allows your workflows access to information about their specific execution\.

## Context Object Format<a name="contextobject-format"></a>

The context object includes information about the state machine, state, execution, and task\. This JSON object includes nodes for each type of data, and is in the following format\.

```
{
    "Execution": {
        "Id": "String",
        "Input": {},
        "StartTime": "Format: ISO 8601"
    },
    "State": {
        "EnteredTime": "Format: ISO 8601",
        "Name": "String",
        "RetryCount": Number
    },
    "StateMachine": {
        "Id": "String"
    },
    "Task": {
        "Token": "String"
    }
}
```

During an execution, the context object is populated with relevant data for the `"Parameters"` field from where it is accessed\. The value for a `"Task"` field is null if the `"Parameters"` field is outside of a task state\.

Content from a running execution includes specifics in the following format\. 

```
{
    "Execution": {
        "Id": "arn:aws:states:us-east-1:123456789012:execution:stateMachineName:executionName",
        "Input": {
           "key": "value"
        },
        "Name": "executionName",
        "RoleArn": "arn:aws:iam::123456789012:role...",
        "StartTime": "2019-03-26T20:14:13.192Z"
    },
    "State": {
        "EnteredTime": "2019-03-26T20:14:13.192Z",
        "Name": "Test",
        "RetryCount": 3
    },
    "StateMachine": {
        "Id": "arn:aws:states:us-east-1:123456789012:stateMachine:stateMachineName",
        "Name": "name"
    },
    "Task": {
        "Token": "h7XRiCdLtd/83p1E0dMccoxlzFhglsdkzpK9mBVKZsp7d9yrT1W"
    }
}
```

## Accessing the Context Object<a name="contextobject-access"></a>

To access the context object, first specify the parameter name by appending `.$` to the end, as you do when selecting state input with a path\. Then, to access context object data instead of the input, prepend the path with `$$.`\. This tells Step Functions to use the path to select a node in the context object\. 

This example task state uses a path to retrieve and pass the execution ARN to an Amazon SQS message\.

```
{
  "Order Flight Ticket Queue": {
    "Type": "Task",
    "Resource": "arn:aws:states:::sqs:sendMessage",
    "Parameters": {
        "QueueUrl": "https://sqs.us-east-1.amazonaws.com/123456789012/flight-purchase",
        "MessageBody": {
          "From": "YVR",
          "To": "SEA",
      "Execution.$": "$$.Execution.Id"
      }
    },
    "Next": "NEXT_STATE"
  }
}
```

**Note**  
For more information about using the task token when calling an integrated service, see [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token)\.