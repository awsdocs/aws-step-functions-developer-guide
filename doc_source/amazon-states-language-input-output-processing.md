# Input and Output Processing<a name="amazon-states-language-input-output-processing"></a>

In this section you will learn how to use paths and reference paths for input and output processing\.

## Paths<a name="amazon-states-language-paths"></a>

In Amazon States Language, a *path* is a string beginning with `$` that you can use to identify components within JSON text\. Paths follow [JsonPath](https://github.com/json-path/JsonPath) syntax\.

## Reference Paths<a name="amazon-states-language-reference-paths"></a>

A *reference path* is a path whose syntax is limited in such a way that it can identify only a single node in a JSON structure:

+ You can access object fields using only dot \(`.`\) and square bracket \(`[ ]`\) notation\.

+ The operators `@ .. , : ? *` aren't supported\.

For example, state input data contains the following values:

```
{
    "foo": 123,
    "bar": ["a", "b", "c"],
    "car": {
        "cdr": true
    }
}
```

In this case, the following reference paths would return:

```
$.foo => 123
$.bar => ["a", "b", "c"]
$.car.cdr => true
```

Certain states use paths and reference paths to control the flow of a state machine or configure a states's settings or options\.

### Paths in InputPath, ResultPath, and OutputPath Fields<a name="amazon-states-language-path-types"></a>

To specify how to use part of the state's input and what to send as output to the next state, you can use `InputPath`, `OutputPath`, and `ResultPath`:

+ For `InputPath` and `OutputPath`, you must use a [path](#amazon-states-language-paths) that follows the [JsonPath](https://github.com/json-path/JsonPath) syntax\.

+ For `ResultPath`, you must use a [reference path](#amazon-states-language-reference-paths)\.

#### InputPath<a name="amazon-states-language-inputpath"></a>

The `InputPath` field selects a portion of the state's input to pass to the state's task for processing\. If you omit the field, it gets the `$` value, representing the entire input\. If you use `null`, the input is discarded \(not sent to the state's task\) and the task receives JSON text representing an empty object `{}`\.

**Note**  
A path can yield a selection of values\. Consider the following example:  

```
{ "a": [1, 2, 3, 4] }
```
If you apply the path `$.a[0:2]`, the following is the result:  

```
[ 1, 2 ]
```

#### ResultPath<a name="amazon-states-language-resultpath"></a>

Usually, if a state executes a task, the task results are sent along as the state's output \(which becomes the input for the next task\)\.

If a state doesn't execute a task, the state's own input is sent, unmodified, as its output\. However, when you specify a path in the value of a state's `ResultPath` and `OutputPath` fields, different scenarios become possible\.

The `ResultPath` takes the results of executing the state's task and places them in the input\. Next, the `OutputPath` selects a portion of the input to send as the state's output\. The `ResultPath` might add the results of executing the state's task to the input, overwrite an existing part, or overwrite the entire input:

+ If the `ResultPath` matches an item in the state's input, only that input item is overwritten with the results of executing the state's task\. The entire modified input becomes available to the state's output\.

+ If the `ResultPath` doesn't match an item in the state's input, an item is added to the input\. The item contains the results of executing the state's task\. The expanded input becomes available to the state's output\.

+ If the `ResultPath` has the default value of `$`, it matches the entire input\. In this case, the results of the state execution overwrite the input entirely and the input becomes available to pass along\.

+ If the `ResultPath` is `null`, the results of executing the state are discarded and the input is untouched\.

**Note**  
 `ResultPath` field values must be [reference paths](#amazon-states-language-reference-paths)\.

#### OutputPath<a name="amazon-states-language-outputpath"></a>

+ If the `OutputPath` matches an item in the state's input, only that input item is selected\. This input item becomes the state's output\.

+ If the `OutputPath` doesn't match an item in the state's input, an exception specifies an invalid path\. For more information, see [Errors](amazon-states-language-errors.md)\.

+ If the `OutputPath` has the default value of `$`, this matches the entire input completely\. In this case, the entire input is passed to the next state\.
**Note**  
For more information about the effect `ResultPath` has on the input for those states that allow it, see [ResultPath](#amazon-states-language-resultpath)\.

+ If the `OutputPath` is `null`, JSON text representing an empty object `{}` is sent to the next state\.

The following example demonstrates how `InputPath`, `ResultPath`, and `OutputPath` fields work in practice\. Consider the following input for the current state:

```
{
  "title": "Numbers to add",
  "numbers": { "val1": 3, "val2": 4 }
}
```

In addition, the state has the following `InputPath`, `ResultPath`, and `OutputPath` fields:

```
"InputPath": "$.numbers",
"ResultPath": "$.sum",
"OutputPath": "$"
```

The state's task receives only the `numbers` object from the input\. In turn, if this task returns `7`, the output of this state is as follows:

```
{
  "title": "Numbers to add",
  "numbers": { "val1": 3, "val2": 4 }
  "sum": 7
}
```

You can slightly modify the `OutputPath`:

```
"InputPath": "$.numbers",
"ResultPath": "$.sum",
"OutputPath": "$.sum"
```

As before, you use the following state input data:

```
{
  "numbers": { "val1": 3, "val2": 4 }
}
```

However, now the state output data is `7`\.