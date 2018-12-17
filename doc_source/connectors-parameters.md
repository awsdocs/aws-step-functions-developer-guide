# Pass Parameters to a Service API<a name="connectors-parameters"></a>

Use the `Parameters` field in a `Task` state to control what parameters are passed to a service API\.

## Pass Static JSON as Parameters<a name="connectors-parameters-json"></a>

You can include a JSON object directly in your state machine definition to pass as a parameter to a resource\. For example, to set the `RetryStrategy` parameter for the `SubmitJob` API for AWS Batch, you could include in your parameters:

```
"RetryStrategy": {
          "attempts": 5
        }
```

You can pass multiple parameters with static JSON as well\. As a more complete example, here are the `Resource` and `Parameters` of the specification of a task that publishes an Amazon SNS topic:

```
"Resource": "arn:aws:states:::sns:publish",
     "Parameters": {
       "TopicArn": "arn:aws:sns:us-east-1:123456789012:myTopic",
       "Message": "test message",
       "MessageStructure": "json",
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

## Pass State Input as Parameters Using Paths<a name="connectors-parameters-path"></a>

You can pass portions of the state input into parameters by using [Paths](amazon-states-language-input-output-processing.md#amazon-states-language-paths)\. A Path is a string, beginning with `$`, that is used to identify components within JSON text\. Step Functions paths use [JsonPath](https://github.com/json-path/JsonPath) syntax\.

To specify that a parameter use a path to reference a JSON node in the input, end the name of the parameter with `.$`\. For instance, if you have text in your state input in a node named `message`, you could pass that to a parameter by referencing the input JSON with a path\. With the following state input:

```
{
  "comment": "A message in the state input",
  "input": {
    "message": "foo",
    "otherInfo": "bar",
  },
  "data": "example"
}
```

You could pass the message `foo` as a parameter using: 

```
"Parameters": {"Message.$": "$.input.message"},
```

For more information about using parameters in Step Functions, see:
+ [Input and Output Processing](concepts-input-output-filtering.md)
+ [`InputPath` and `Parameters`](input-output-inputpath-params.md)

**Note**  
For a list of services that can be controlled directly from the Amazon States Language, see [Supported AWS Service Integrations for Step Functions](connectors-supported-services.md)\.