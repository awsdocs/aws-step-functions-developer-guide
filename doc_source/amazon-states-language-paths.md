# Paths<a name="amazon-states-language-paths"></a>

In the Amazon States Language, a *path* is a string beginning with `$` that you can use to identify components within JSON text\. Paths follow [JsonPath](https://github.com/json-path/JsonPath) syntax\. You can specify a path to access subsets of the input when specifying values for `InputPath`, `ResultPath`, and `OutputPath`\. For more information see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.

**Note**  
You can also specify a JSON node of the input or the context object by using paths within the `Parameters` field of a state definition\. See [Pass Parameters to a Service API](connect-parameters.md)\.

## Reference Paths<a name="amazon-states-language-reference-paths"></a>

A *reference path* is a path whose syntax is limited in such a way that it can identify only a single node in a JSON structure:
+ You can access object fields using only dot \(`.`\) and square bracket \(`[ ]`\) notation\.
+ Functions such as `length()` aren't supported\.
+ Lexical (non-symbol) operators such as `subsetof` aren't supported\.
+ Filtering by regular expression or by referencing another value in the JSON structure isn't supported\.
+ The `@` operator, matching the current node being processed in a filter, does not match scalar values, only objects\.

For example, if state input data contains the following values:

```
{
    "foo": 123,
    "bar": ["a", "b", "c"],
    "car": {
        "cdr": true
    },
    "jar": [{"a": 1}, {"a": 5}, {"a": 2}, {"a": 7}, {"a": 3}]
}
```

The following reference paths would return the following\.

```
$.foo => 123
$.bar => ["a", "b", "c"]
$.car.cdr => true
$.jar[?(@.a >= 5)] => [{"a": 5}, {"a": 7}]
```

Certain states use paths and reference paths to control the flow of a state machine or configure a state's settings or options\. For more information see [Modeling workflow input and output path processing with data flow simulator](https://aws.amazon.com/blogs/compute/modeling-workflow-input-output-path-processing-with-data-flow-simulator/) and [Using JSONPath effectively in AWS Step Functions](https://aws.amazon.com/blogs/compute/using-jsonpath-effectively-in-aws-step-functions/)\.
