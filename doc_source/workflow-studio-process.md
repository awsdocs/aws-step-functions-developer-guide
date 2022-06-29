# Configure inputs and outputs for your states<a name="workflow-studio-process"></a>

Each state makes a decision or performs an action based on input that it receives\. In most cases, it then passes output to other states\. In Workflow Studio, you can configure how a state filters and manipulates its input and output data in the **Input** and **Output** tabs of the **Form** panel\.  Use the **Info** links to access contextual help when configuring inputs and outputs\. 

![\[Inputs, outputs and help panel\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_input_output_01.png)

For detailed information about how Step Functions processes input and output, see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\. 

## Configure input to a state<a name="workflow-studio-process-input"></a>

Each state receives input from the previous state as JSON\. If you want to filter the input, you can use the `InputPath` under the **Input** tab in the **Inspector** to select this\. The `InputPath` is a string, beginning with `$`, that identifies a specific JSON node\. These are called [reference paths](amazon-states-language-paths.md), and they follow JsonPath syntax\. 

![\[Input options\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_input_01.png)

To filter the input:
+ Choose **Filter input with `InputPath`**\.
+ Enter a valid `InputPath`\.

Your `InputPath` filter will be added to your workflow\.

**Example 1**

For this example, the input to your state includes the following:

```
{
  "comment": "Example for InputPath",
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

To apply the `InputPath` choose **Filter input with InputPath**, then enter an appropriate reference path\. If you enter `$.dataset2.val1`, the following JSON is passed as input to the state:

```
{"a"}
```

A reference path can also have a selection of values\. If your reference is `{ "a": [1, 2, 3, 4] }` and you apply the reference path `$.a[0:2]` as the InputPath, the following is the result:

```
[ 1, 2 ]
```

`Parallel`, `Map` and `Pass` flow states have an additional input filtering option called `Parameters` under their **Input** tab\. This filter takes effect after the InputPath filter and can be used to construct a custom JSON object consisting of one or more key\-value pairs\. The values of each pair can either be static values, can be selected from the input or can be selected from the [Context Object](input-output-contextobject.md) with a path\. 

**Note**  
To specify that a parameter uses a reference path to point to a JSON node in the input, the parameter name must end with `.$`\.

**Example 2**

In this example, the following is the input to a `Parallel` state:

```
{
  "comment": "Example for Parameters",
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

To select part of this information and pass additional key\-value pairs with a static value, you can specify the following in the **Parameters** field, under the **Parallel** state’s **Input** tab\.

```
{
 "comment": "Selecting what I care about.",
 "MyDetails": {
    "size.$": "$.product.details.size",
    "exists.$": "$.product.availability",
    "StaticValue": "foo"
    }
 }
```

The following JSON will be the result:

```
{
  "comment": "Selecting what I care about.",
  "MyDetails": {
    "size": "small",
    "exists": "in stock",
    "StaticValue": "foo"
  }
}
```

## Configure output of a state<a name="workflow-studio-process-output"></a>

Each state produces JSON output that can be filtered before it is passed to the next state\. There are several filters available, and each affects the output in a different way\. Output filters available for each state are listed under the **Output** tab in the **Inspector**\. For task states, any output filters you select are processed in this order: 

1.   `ResultSelector` Use `ResultSelector` to manipulate the state’s result\. You can construct a new JSON object with parts of the result\. 

1.   `ResultPath`\. Use `ResultPath` to select a combination of the state input and the task result to pass to the output\. 

1.   `OutputPath`\. Use `OutputPath` to filter the JSON output to choose which information from the result will be passed to the next state\. 

![\[Output options\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_output_01.png)

### Use `ResultSelector`<a name="workflow-studio-process-output-resultselector"></a>

`ResultSelector` is an optional output filter for the following states:
+  *Task states*, which are all states listed in the **Actions** panel of the States browser\. 
+  *Map states*, in the **Flow** panel of the States browser\. 
+  *Parallel states*, in the **Flow** panel of the States browser\. 

`ResultSelector` can be used to construct a custom JSON object consisting of one or more key\-value pairs\. The values of each pair can either be static values or selected from the state's result with a path\. 

**Note**  
To specify that a parameter uses a path to reference a JSON node in the result, the parameter name must end with `.$`\.

**Example 1**

In this example, you use `ResultSelector` to manipulate the response from the Amazon EMR CreateCluster API call for an Amazon EMR `CreateCluster` state\. The following is the result from the Amazon EMR `CreateCluster` API call:

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

To select part of this information and pass an additional key\-value pair with a static value, specify the following in the `ResultSelector`field, under the state’s **Output** tab:

```
{
 "result": "found",
 "ClusterId.$": "$.output.ClusterId", 
 "ResourceType.$": "$.resourceType"
 }
```

Using `ResultSelector` produces the following result:

```
{
 "result": "found",
 "ClusterId": "AKIAIOSFODNN7EXAMPLE",
 "ResourceType": "elasticmapreduce"
}
```

### Use `ResultPath`<a name="workflow-studio-process-output-resultpath"></a>

The output of a state can be a copy of its input, the result it produces , or a combination of its input and result\. Use `ResultPath` to control which combination of these is passed to the state output\. For more use cases of `ResultPath`, see [ResultPath](input-output-resultpath.md)\.

`ResultPath` is an optional output filter for the following states:
+  *Task states*, which are all states listed in the **Actions** panel of the States browser\. 
+  *Map states*, in the **Flow** panel of the States browser\. 
+  *Parallel states*, in the **Flow** panel of the States browser\. 
+  *Pass states*, in the **Flow** panel of the States browser\. 

`ResultPath` can be used to add the result into the original state input\. The specified path indicates where to add the result\.

**Example**

The following is the input to a given Task state:

```
{
  "details": "Default example",
  "who": "AWS Step Functions"
}
```

The result of the Task state is the following:

```
Hello, AWS Step Functions
```

You can add this result to the state’s input by applying `ResultPath` and entering a reference [path](amazon-states-language-paths.md) that indicates where to add the result, such as `$.taskresult`:

With this `ResultPath`, the following is the JSON that is passed as the state’s output\.

```
{
  "details": "Default example",
  "who": "AWS Step Functions",
  "taskresult": "Hello, AWS Step Functions!"
}
```

### Use OutputPath<a name="workflow-studio-process-output-resultselector"></a>

The `OutputPath` filter lets you filter out unwanted information, and pass only the portion of JSON that you care about\. The `OutputPath` is a string, beginning with $, that identifies nodes within JSON text\.

**Example**

A Lambda Invoke API call returns metadata in addition to the payload, which is the Lambda function’s result\. An example of the response from this API call is shown under the **Output** tab of the state’s **Form** mode\.

![\[Output panel\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_input_output_output_01.png)

You can use `OutputPath` to filter out the additional metadata\. By default, Lambda Invoke states created through the Workflow Studio contain a `$.Payload OutputPath` filter\. This default removes the additional metadata and returns an output equivalent to running the Lambda function directly\.

The Lambda Invoke task result example and the `$.Payload` **Output** path pass the following JSON as the output:

```
{
 "foo": "bar",
 "colors": [
      "red",
      "blue",
      "green"    
 ],
 "car": {
      "year": 2008,
      "make": "Toyota",
      "model": "Matrix"
 }
}
```

**Note**  
Since the `OutputPath` filter is the last output filter to take effect, if you use additional output filters such as `ResultSelector` or `ResultPath`, you should modify the default `$.Payload` `OutputPath` filter accordingly\. 