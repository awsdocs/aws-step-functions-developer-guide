# Paths<a name="amazon-states-language-paths"></a>

In the Amazon States Language, a *path* is a string beginning with `$` that you can use to identify components within JSON text\. Paths follow [JsonPath](https://github.com/json-path/JsonPath) syntax\. You can specify a path to access subsets of the input when specifying values for `InputPath`, `ResultPath`, and `OutputPath`\. For more information see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.

**Note**  
You can also specify a JSON node of the input or the context object by using paths within the `"Parameters"` field of a state definition\. See [Pass Parameters to a Service API](connect-parameters.md)\.

## Reference Paths<a name="amazon-states-language-reference-paths"></a>

A *reference path* is a path whose syntax is limited in such a way that it can identify only a single node in a JSON structure:
+ You can access object fields using only dot \(`.`\) and square bracket \(`[ ]`\) notation\.
+ The operators `@ .. , : ? *` aren't supported\.
+ Functions such as `length()` aren't supported\.

For example, if state input data contains the following values:

```
{
    "foo": 123,
    "bar": ["a", "b", "c"],
    "car": {
        "cdr": true
    }
}
```

The following reference paths would return the following\.

```
$.foo => 123
$.bar => ["a", "b", "c"]
$.car.cdr => true
```

Certain states use paths and reference paths to control the flow of a state machine or configure a state's settings or options\.