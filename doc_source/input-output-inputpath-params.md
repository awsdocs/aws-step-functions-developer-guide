# InputPath, Parameters and ResultSelector<a name="input-output-inputpath-params"></a>

The `InputPath`, `Parameters` and `ResultSelector` fields provide a way to manipulate JSON as it moves through your workflow\. `InputPath` can limit the input that is passed by filtering the JSON notation by using a path \(see [Paths](amazon-states-language-paths.md)\)\. The `Parameters` field enables you to pass a collection of key\-value pairs, where the values are either static values that you define in your state machine definition, or that are selected from the input using a path\. The `ResultSelector` field provides a way to manipulate the state’s result before `ResultPath` is applied\. 

AWS Step Functions applies the `InputPath` field first, and then the `Parameters` field\. You can first filter your raw input to a selection you want using `InputPath`, and then apply `Parameters` to manipulate that input further, or add new values\. You can then use the `ResultSelector` field to manipulate the state's output before `ResultPath` is applied\.

**Tip**  
Use the [ data flow simulator in the Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/simulator) to test JSON path syntax, to better understand how data is manipulated within a state, and to see how data is passed between states\.

## InputPath<a name="input-output-inputpath"></a>

Use `InputPath` to select a portion of the state input\. 

For example, suppose the input to your state includes the following\.

```
{
  "comment": "Example for InputPath.",
  "dataset1": {
    "val1": 1,
    "val2": 2,
    "val3": 3
  },
  "dataset2": {
    "val1": "a",
    "val2": "b",
    "val3": "c"
  }
}
```

You could apply the `InputPath`\.

```
"InputPath": "$.dataset2",
```

With the previous `InputPath`, the following is the JSON that is passed as the input\.

```
{
  "val1": "a",
  "val2": "b",
  "val3": "c"
}
```

**Note**  
A path can yield a selection of values\. Consider the following example\.  

```
{ "a": [1, 2, 3, 4] }
```
If you apply the path `$.a[0:2]`, the following is the result\.  

```
[ 1, 2 ]
```

## Parameters<a name="input-output-parameters"></a>

This section describes the different ways you can use the Parameters field\. 

### Key\-value pairs<a name="input-output-parameters-keyvalue"></a>

Use the `Parameters` field to create a collection of key\-value pairs that are passed as input\. The values of each can either be static values that you include in your state machine definition, or selected from either the input or the context object with a path\. For key\-value pairs where the value is selected using a path, the key name must end in `.$`\. 

For example, suppose you provide the following input\. 

```
{
  "comment": "Example for Parameters.",
  "product": {
    "details": {
       "color": "blue",
       "size": "small",
       "material": "cotton"
    },
    "availability": "in stock",
    "sku": "2317",
    "cost": "$23"
  }
}
```

To select some of the information, you could specify these parameters in your state machine definition\. 

```
"Parameters": {
        "comment": "Selecting what I care about.",
        "MyDetails": {
          "size.$": "$.product.details.size",
          "exists.$": "$.product.availability",
          "StaticValue": "foo"
        }
      },
```

Given the previous input and the `Parameters` field, this is the JSON that is passed\.

```
{
  "comment": "Selecting what I care about.",
  "MyDetails": {
      "size": "small",
      "exists": "in stock",
      "StaticValue": "foo"
  }
},
```

In addition to the input, you can access a special JSON object, known as the context object\. The context object includes information about your state machine execution\. See [Context Object](input-output-contextobject.md)\.

### Connected resources<a name="input-output-parameters-connected"></a>

The `Parameters` field can also pass information to connected resources\. For example, if your task state is orchestrating an AWS Batch job, you can pass the relevant API parameters directly to the API actions of that service\. For more information, see:
+ [Pass Parameters to a Service API](connect-parameters.md)
+ [Working with other services](concepts-service-integrations.md)

### Amazon S3<a name="input-output-parameters-s3"></a>

Alternatively, if the Lambda function data you are passing between states might grow to more than 262,144 bytes, we recommend using Amazon S3 to store the data, and parse the Amazon Resource Name \(ARN\) of the bucket in the `Payload` parameter to get the bucket name and key value\. Alternatively, you can adjust your implementation to pass smaller payloads in your executions\. For more information, see [Use Amazon S3 ARNs instead of passing large payloads](avoid-exec-failures.md)\.

## ResultSelector<a name="input-output-resultselector"></a>

 Use the `ResultSelector` field to manipulate a state's result before `ResultPath` is applied\. The `ResultSelector` field lets you create a collection of key value pairs, where the values are static or selected from the state's result\. Using the `ResultSelector` field, you can choose what parts of a state's result you want to pass to the `ResultPath` field\.

**Note**  
With the `ResultPath` field, you can add the output of the `ResultSelector` field to the original input\.

`ResultSelector` is an optional field in the following states:
+ [Map](amazon-states-language-map-state.md)
+ [Task](amazon-states-language-task-state.md)
+ [Parallel](amazon-states-language-parallel-state.md)

For example, Step Functions service integrations return metadata in addition to the payload in the result\. `ResultSelector` can select portions of the result and merge them with the state input with `ResultPath`\. In this example, we want to select just the `resourceType` and `ClusterId`, and merge that with the state input from an Amazon EMR createCluster\.sync\. Given the following:

```
{
  "resourceType": "elasticmapreduce",
  "resource": "createCluster.sync",
  "output": {
    "SdkHttpMetadata": {
      "HttpHeaders": {
        "Content-Length": "1112",
        "Content-Type": "application/x-amz-JSON-1.1",
        "Date": "Mon, 25 Nov 2019 19:41:29 GMT",
        "x-amzn-RequestId": "1234-5678-9012"
      },
      "HttpStatusCode": 200
    },
    "SdkResponseMetadata": {
      "RequestId": "1234-5678-9012"
    },
    "ClusterId": "AKIAIOSFODNN7EXAMPLE"
  }
}
```

You can then select the `resourceType` and `ClusterId` using `ResultSelector`:

```
"Create Cluster": {
  "Type": "Task",
  "Resource": "arn:aws:states:::elasticmapreduce:createCluster.sync",
  "Parameters": {
    <some parameters>
  },
  "ResultSelector": {
    "ClusterId.$": "$.output.ClusterId",
    "ResourceType.$": "$.resourceType"
  },
  "ResultPath": "$.EMROutput",
  "Next": "Next Step"
}
```

With the given input, using `ResultSelector` produces:

```
{
  "OtherDataFromInput": {},
  "EMROutput": {
    "ResourceType": "elasticmapreduce",
    "ClusterId": "AKIAIOSFODNN7EXAMPLE"
  }
}
```