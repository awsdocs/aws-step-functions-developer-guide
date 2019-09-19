# Map<a name="amazon-states-language-map-state"></a>

The `Map` state \(`"Type": "Map"`\) can be used to run a set of steps for each element of an input array\. While the `[Parallel](amazon-states-language-parallel-state.md)` state executes multiple branches of steps using the same input, a `Map` state will execute the same steps for multiple entries of an array in the state input\. 

For an introduction to using a `Map` state, see the [Map State Tutorial](tutorial-creating-map-state-machine.md)\.

In addition to the [common state fields](amazon-states-language-common-fields.md), `Map` states include these additional fields\.

** `Iterator` \(Required\)**  
The `“Iterator”` field’s value is an object that defines a state machine which will process each element of the array\.

** `ItemsPath` \(Optional\)**  
The `“ItemsPath”` field’s value is a reference path identifying where in the effective input the array field is found\. For more information, see [ItemsPath](input-output-itemspath.md)\.  
States within an `"Iterator"` field can only transition to each other, and no state outside the `“ItemsPath”` field can transition to a state within it\.  
If any iteration fails, entire Map state fails, and all iterations are terminated\.

** `MaxConcurrency` \(Optional\)**  
The `“MaxConcurrency”`field’s value is an integer that provides an upper bound on how many invocations of the Iterator may run in parallel\. For instance, a `"MaxConcurrency"` value of 10 will limit your `Map` state to 10 concurrent iterations running at one time\.  
The `“MaxConcurrency”` value is an upper bound limit, and not a guarantee that it will run that many concurrent iterations\.
The default value is `"0"`, which places no limit on parallelism and iterations are invoked as concurrently as possible\.   
A `"MaxConcurrency"` value of `"1"` invokes the `"Iterator"` once for each array element in the order of their appearance in the input, and will not start a new iteration until the previous has completed\.

** `ResultPath` \(Optional\)**  
Specifies where \(in the input\) to place the output of the branches\. The input is then filtered as specified by the `OutputPath` field \(if present\) before being used as the state's output\. For more information, see [Input and Output Processing](concepts-input-output-filtering.md)\.

** `Retry` \(Optional\)**  
An array of objects, called Retriers, that define a retry policy in case the state encounters runtime errors\. For more information, see [Examples Using Retry and Using Catch](concepts-error-handling.md#error-handling-examples)\.

** `Catch` \(Optional\)**  
An array of objects, called Catchers, that define a fallback state that is executed if the state encounters runtime errors and its retry policy is exhausted or isn't defined\. For more information, see [Fallback States](concepts-error-handling.md#error-handling-fallback-states)\.

## Map State Example<a name="map-state-examples"></a>

Consider the following input data for a `Map` state\.

```
{
  "ship-date": "2016-03-14T01:59:00Z",
  "detail": {
    "delivery-partner": "UQS",
    "shipped": [
      { "prod": "R31", "dest-code": 9511, "quantity": 1344 },
      { "prod": "S39", "dest-code": 9511, "quantity": 40 },
      { "prod": "R31", "dest-code": 9833, "quantity": 12 },
      { "prod": "R40", "dest-code": 9860, "quantity": 887 },
      { "prod": "R40", "dest-code": 9511, "quantity": 1220 }
    ]
  }
}
```

Given the previous input, the `Map` state in the following example will invoke a AWS Lambda function \(`ship-val`\) once for each item of the array in the `"shipped"` field\. 

```
"Validate-All": {
  "Type": "Map",
  "InputPath": "$.detail",
  "ItemsPath": "$.shipped",
  "MaxConcurrency": 0,
  "Iterator": {
    "StartAt": "Validate",
    "States": {
      "Validate": {
        "Type": "Task",
	"Resource": "arn:aws:lambda:us-east-1:123456789012:function:ship-val",
        "End": true
      }
    }
  },
  "ResultPath": "$.detail.shipped",
  "End": true
}
```

Each iteration of the `Map` state will send an item in the array \(selected with the [`"ItemsPath"`](input-output-itemspath.md) field\) as input to the Lambda function\. For instance, the input to one invocation of Lambda would be the following\.

```
{
  "prod": "R31",
  "dest-code": 9511,
  "quantity": 1344
}
```

When complete, the output of the `Map` state is a JSON array where each item is the output of an iteration \(in this case, the output of the `ship-val` Lambda function\)\.

## Map State Example With Parameters<a name="map-state-example-params"></a>

Suppose that the `ship-val` Lambda function in the previous example also needs information about the shipment's courier as well as the items in the array for each iteration\. You can include information from the input, along with information specific to the current iteration of the map state\. Note the `"Parameters"` field in the following example\.

```
"Validate-All": {
  "Type": "Map",
  "InputPath": "$.detail",
  "ItemsPath": "$.shipped",
  "MaxConcurrency": 0,
  "ResultPath": "$.detail.shipped",
  "Parameters": {
    "parcel.$": "$$.Map.Item.Value",
    "courier.$": "$.delivery-partner"
  },
  "Iterator": {
    "StartAt": "Validate",
    "States": {
      "Validate": {
        "Type": "Task",
	"Resource": "arn:aws:lambda:us-east-1:123456789012:function:ship-val",
        "End": true
      }
    }
  },
  "End": true
}
```

The `"Parameters"` block replaces the input to the iterations with a JSON node that contains both the current item data from the [context object](input-output-contextobject.md#contextobject-map), and the courier information from the `"delivery-partner"` field from the `Map` state input\. The following is an example of input to a single iteration, that is passed to an invocation of the `ship-val` Lambda function\.

```
{
  "parcel": {
    "prod": "R31",
    "dest-code": 9511,
    "quantity": 1344
   },
   "courier": "UQS"
}
```

In the previous `Map` state example, the [`"ResultPath"`](input-output-resultpath.md) field produces output the same as the input, but with the `“detail.shipped”` field overwritten by an array in which each element is the output of the `“ship-val”` Lambda function for each iteration\.

For more information see the following\.
+ [Use a Map State to Call Lambda Multiple Times](tutorial-creating-map-state-machine.md)
+ [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)
+ [ItemsPath](input-output-itemspath.md)
+ [Context Object Data For Map States](input-output-contextobject.md#contextobject-map)

## Map State Input and Output Processing<a name="amazon-states-language-map-state-output"></a>

For a map state, [`InputPath`](input-output-inputpath-params.md#input-output-inputpath) works as it does for other state types, selecting a subset of the input\.

The input of a `Map` state must include a JSON array, and it will run the `Iterator` section once for each item in the array\. You specify where in the input to find this array using the [`ItemsPath`](input-output-itemspath.md) field\. If not specified, the value of `ItemsPath` is `"$"`, and the `Iterator` section expects that the array is the only input\. A `Map` state may also include an [`ItemsPath`](input-output-itemspath.md) field, whose value must be a [Reference Path](amazon-states-language-paths.md#amazon-states-language-reference-paths)\. The `ItemsPath` field selects where in the input to find the array to use for iterations\. The Reference Path is applied to the effective input \(after `InputPath` is applied\) and must identify a field whose value is a JSON array\.

The input to each iteration, by default, is a single element of the array field identified by the `ItemsPath` value, This may be overridden using the `[Parameters](input-output-inputpath-params.md#input-output-parameters)` field\.

When complete, the output of the `Map` state is a JSON array, where each item is the output of an iteration\.

 For more information, see the following\. 
+ [Map State Tutorial](tutorial-creating-map-state-machine.md)
+ [Map State Example With Parameters](#map-state-example-params)
+ [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)
+ [Context Object Data For Map States](input-output-contextobject.md#contextobject-map)
+ [Dynamically Process Data with a Map State](sample-map-state.md)