# Pass Parameters to a Service API<a name="connect-parameters"></a>

Use the `"Parameters"` field in a `Task` state to control what parameters are passed to a service API\.

## Pass Static JSON as Parameters<a name="connect-parameters-json"></a>

You can include a JSON object directly in your state machine definition to pass as a parameter to a resource\. 

For example, to set the `RetryStrategy` parameter for the `SubmitJob` API for AWS Batch, you could include the following in your parameters\.

```
"RetryStrategy": {
          "attempts": 5
        }
```

You can also pass multiple parameters with static JSON\. As a more complete example, the following are the `"Resource"` and `"Parameters"` fields of the specification of a task that publishes an Amazon SNS topic\.

```
"Resource": "arn:aws:states:::sns:publish",
     "Parameters": {
       "TopicArn": "arn:aws:sns:us-east-1:123456789012:myTopic",
       "Message": "test message",
       "MessageAttributes": {
         "my attribute no 1": {
           "DataType": "String",
           "StringValue": "value of my attribute no 1"
         },
         "my attribute no 2": {
           "DataType": "String",
           "StringValue": "value of my attribute no 2"
         }
       }
     },
```

## Pass State Input as Parameters Using Paths<a name="connect-parameters-path"></a>

You can pass portions of the state input into parameters by using [paths](amazon-states-language-paths.md)\. A path is a string, beginning with `$`, that's used to identify components within JSON text\. Step Functions paths use [JsonPath](https://github.com/json-path/JsonPath) syntax\.

To specify that a parameter use a path to reference a JSON node in the input, end the parameter name with `.$`\. For example, if you have text in your state input in a node named `message`, you could pass that to a parameter by referencing the input JSON with a path\. 

Using the following state input:

```
{
  "comment": "A message in the state input",
  "input": {
    "message": "foo",
    "otherInfo": "bar"
  },
  "data": "example"
}
```

You could pass the message `foo` as a parameter using: the following\.

```
"Parameters": {"Message.$": "$.input.message"},
```

For more information about using parameters in Step Functions, see:
+ [Input and Output Processing](concepts-input-output-filtering.md)
+ [InputPath and Parameters](input-output-inputpath-params.md)

## Pass Context Object Nodes as Parameters<a name="connect-parameters-context"></a>

In addition to static content, and nodes from the state input, you can pass nodes from the context object as parameters\. The context object is dynamic JSON data that exists during a state machine execution\. It includes information about your state machine and the current execution\. You can access the context object using a path in the `"Parameters"` field of a state definition\.

For more information about the context object and how to access that data from a `"Parameters"` field, see:
+ [The Context Object](input-output-contextobject.md)
+ [Accessing the Context Object](input-output-contextobject.md#contextobject-access)
+ [Get a Token from the Context Object](connect-to-resource.md#wait-token-contextobject)