# Context Object<a name="input-output-contextobject"></a>

The context object is an internal JSON structure that is available during an execution, and contains information about your state machine and execution\. This allows your workflows access to information about their specific execution\. You can access the context object from the following fields:
+ `InputPath`
+ `OutputPath`
+ `ItemsPath` \(in Map states\)
+ `Variable` \(in Choice states\)
+ `ResultSelector`
+ Variable to variable comparison operators

## Context Object Format<a name="contextobject-format"></a>

The context object includes information about the state machine, state, execution, and task\. This JSON object includes nodes for each type of data, and is in the following format\.

```
{
    "Execution": {
        "Id": "String",
        "Input": {},
        "Name": "String",
        "RoleArn": "String",
        "StartTime": "Format: ISO 8601"
    },
    "State": {
        "EnteredTime": "Format: ISO 8601",
        "Name": "String",
        "RetryCount": Number
    },
    "StateMachine": {
        "Id": "String",
        "Name": "String"
    },
    "Task": {
        "Token": "String"
    }
}
```

During an execution, the context object is populated with relevant data for the `Parameters` field from where it is accessed\. The value for a `Task` field is null if the `Parameters` field is outside of a task state\.

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
        "Name": "stateMachineName"
    },
    "Task": {
        "Token": "h7XRiCdLtd/83p1E0dMccoxlzFhglsdkzpK9mBVKZsp7d9yrT1W"
    }
}
```

**Note**  
For context object data related to `Map` states, see [Context Object Data for Map States](#contextobject-map)\.

## Accessing the Context Object<a name="contextobject-access"></a>

To access the context object, first specify the parameter name by appending `.$` to the end, as you do when selecting state input with a path\. Then, to access context object data instead of the input, prepend the path with `$$.`\. This tells AWS Step Functions to use the path to select a node in the context object\. 

The following examples show how you can access context objects, such as execution ID, name, and start time\.

**Example to retrieve and pass the execution Amazon Resource Name \(ARN\) to an Amazon Simple Queue Service \(Amazon SQS\) message**  
This example `Task` state uses a path to retrieve and pass the execution Amazon Resource Name \(ARN\) to an Amazon Simple Queue Service \(Amazon SQS\) message\.  

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

**Example to access the execution start time and name in a `Pass` state**  

```
{
  "Comment": "Accessing context object in a state machine",
  "StartAt": "Get execution context data",
  "States": {
    "Get execution context data": {
      "Type": "Pass",
      "Parameters": {
        "startTime.$": "$$.Execution.StartTime",
        "execName.$": "$$.Execution.Name"
      },
      "ResultPath": "$.executionContext",
      "End": true
    }
  }
}
```

## Context Object Data for Map States<a name="contextobject-map"></a>

There are two additional items available in the context object when processing a [`Map` state](amazon-states-language-map-state.md): `Index` and `Value`\. For each `Map` state iteration, `Index` contains the index number for the array item that is being currently processed, while `Value` contains the array item being processed\. Within a `Map` state, the context object includes the following data:

```
"Map": {
   "Item": {
      "Index": Number,
      "Value": "String"
   }
}
```

These are available only in a `Map` state, and can be specified in the [`Parameters`](input-output-inputpath-params.md#input-output-parameters) field, before the `Iterator` section\.

**Note**  
You must define parameters from the context object in the `Parameters` block of the main `Map` state, not within the states included in the `Iterator` section\.

Given a state machine with a simple `Map` state, we can inject information from the context object as follows\.

```
{
  "StartAt": "ExampleMapState",
  "States": {
    "ExampleMapState": {
      "Type": "Map",
      "Parameters": {
        "ContextIndex.$": "$$.Map.Item.Index",
        "ContextValue.$": "$$.Map.Item.Value"
      },
      "Iterator": {
         "StartAt": "TestPass",
         "States": {
           "TestPass": {
             "Type": "Pass",    
             "End": true
           }
         }
      },
      "End": true
    }
  }
}
```

If you execute the previous state machine with the following input, `Index` and `Value` are inserted in the output\.

```
[
  {
    "who": "bob"
  },
  {
    "who": "meg"
  },
  {
    "who": "joe"
  }
]
```

The output for the execution returns the values of `Index` and `Value` items for each of the three iterations as follows:

```
[
  {
    "ContextIndex": 0,
    "ContextValue": {
      "who": "bob"
    }
  },
  {
    "ContextIndex": 1,
    "ContextValue": {
      "who": "meg"
    }
  },
  {
    
    "ContextIndex": 2,
    "ContextValue": {
      "who": "joe"
    }
  }
]
```
