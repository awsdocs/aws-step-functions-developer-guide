# InputPath and Parameters<a name="input-output-inputpath-params"></a>

Both the `"InputPath"` and `"Parameters"` fields provide a way to manipulate JSON as it moves through your workflow\. `InputPath` can limit the input that is passed by filtering the JSON notation by using a path \(see [Paths](amazon-states-language-paths.md)\)\. The `"Parameters"` field enables you to pass a collection of key\-value pairs, where the values are either static values that you define in your state machine definition, or that are selected from the input using a path\.

AWS Step Functions applies the `"InputPath"` field first, and then the `"Parameters"` field\. You can first filter your raw input to a selection you want using `InputPath`, and then apply `Parameters` to manipulate that input further, or add new values\.

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

Use `"Parameters"` field to create a collection of key\-value pairs that are passed as input\. The values of each can either be static values that you include in your state machine definition, or selected from either the input or the context object with a path\. For key\-value pairs where the value is selected using a path, the key name must end in `.$`\. 

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

Given the previous input and the `"Parameters"` field, this is the JSON that is passed\.

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

In addition to the input, you can access a special JSON object, known as the context object\. The context object includes information about your state machine execution\. See [The Context Object](input-output-contextobject.md)\.

**Note**  
The `"Parameters"` field can also pass information to connected resources\. For example, if your task state is orchestrating an AWS Batch job, you can pass the relevant API parameters directly to the API actions of that service\. For more information, see:  
[Pass Parameters to a Service API](connect-parameters.md)
[Service Integrations](concepts-service-integrations.md)