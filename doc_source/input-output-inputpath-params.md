# `InputPath` and `Parameters`<a name="input-output-inputpath-params"></a>

Both `InputPath` and `Parameters` provide a way to manipulate JSON as it moves through your workflow\. `InputPath` can limit the input that is passed by filtering the JSON using a path \(see, [Paths](amazon-states-language-input-output-processing.md#amazon-states-language-paths)\)\. `Parameters` allow you to pass a collection of key\-value pairs, where the values are either static values that you define in your state machine definition, or that are selected from the input using a path\.

AWS Step Functions applies `InputPath` first, and then `Parameters`\. You can first filter your raw input to a selection you want using `InputPath`, and then apply `Parameters` to manipulate that input further, or add new values\.

## `InputPath`<a name="input-output-inputpath"></a>

Use `InputPath` to select a portion of the state input\. For instance, if input to your state includes the following:

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

You could apply the `InputPath`:

```
"InputPath": "$.dataset2",
```

With the above `InputPath`, the JSON that is passed as the input is:

```
{
    "val1": "a",
    "val2": "b",
    "val3": "c"
  }
```

## `Parameters`<a name="input-output-parameters"></a>

Use `Parameters` to create a collection of key\-value pairs that will be passed as input\. The values of each can either be static values that you include in your state machine definition, or selected from the input with a path\. For key\-value pairs where the value is selected using a path, the key name must end in `*.$`\. For instance, given the following input: 

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

To select some of the information you could specify these `Parameters` in your state machine definition:

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

Given the above previous input and specified `Parameters`, this is the JSON that is passed:

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

**Note**  
`Parameters` can also pass information to connected resources\. For instance, if your task state is orchestrating an AWS Batch job, you can pass the relevant API parameters directly to the API actions of that service\. For more information see:  
[Pass Parameters to a Service API](connectors-parameters.md)
[Service Integrations](concepts-connectors.md)