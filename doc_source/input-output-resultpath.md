# ResultPath<a name="input-output-resultpath"></a>

The output of a state can be a copy of its input, the result it produces \(for example, output from a `Task` state’s Lambda function\), or a combination of its input and result\. Use `ResultPath` to control which combination of these is passed to the state output\. 

The following state types can generate a result and can include `ResultPath:`
+ [Pass](amazon-states-language-pass-state.md)
+ [Task](amazon-states-language-task-state.md)
+ [Parallel](amazon-states-language-parallel-state.md)

Use `ResultPath` to combine a task result with task input, or to select one of these\. The path you provide to `ResultPath` controls what information passes to the output\. 

**Note**  
 `ResultPath` is limited to using [reference paths](amazon-states-language-paths.md#amazon-states-language-reference-paths), which limit scope so that it can identify only a single node in JSON\. See [Reference Paths](amazon-states-language-paths.md#amazon-states-language-reference-paths) in the [Amazon States Language](concepts-amazon-states-language.md)\.

These examples are based on the state machine and Lambda function described in the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial\. Work through that tutorial and test different outputs by trying various paths in a `ResultPath` field\.

**Use `ResultPath` to:**
+ [Replace Input With Result](#input-output-resultpath-default)
+ [Include Result With Input](#input-output-resultpath-append)
+ [Update a Node in Input with Result](#input-output-resultpath-amend)
+ [Include Error and Input in a `Catch`](#input-output-resultpath-catch)

## Use ResultPath to Replace the Input with the Result<a name="input-output-resultpath-default"></a>

If you don't specify a `ResultPath`, the default behavior is as if you had specified `"ResultPath": "$"`\. Because this tells the state to replace the entire input with the result, the state input is completely replaced by the result coming from the task result\.

The following diagram shows how `ResultPath` can completely replace the input with the result of the task\.

![\[Replace input with ResultPath.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/input-output-resultpath-replace.png)![\[Replace input with ResultPath.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Replace input with ResultPath.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

Using the state machine and Lambda function described in [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md), if we pass the following input:

```
{
  "comment": "This is a test of the input and output of a Task state.",
  "details": "Default example",
  "who": "AWS Step Functions"
}
```

The Lambda function provides the following result\.

```
"Hello, AWS Step Functions!"
```

If `ResultPath` isn't specified in the state, or if `"ResultPath": "$"` is set, the input of the state is replaced by the result of the Lambda function, and the output of the state is the following\.

```
"Hello, AWS Step Functions!"
```

**Note**  
`ResultPath` is used to include content from the result with the input, before passing it to the output\. But, if `ResultPath` isn't specified, the default is to replace the entire input\.

## Use ResultPath to Include the Result with the Input<a name="input-output-resultpath-append"></a>

The following diagram shows how `ResultPath` can include the result with the input\.

![\[Include input with ResultPath\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/input-output-resultpath-append.png)![\[Include input with ResultPath\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Include input with ResultPath\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

Using the state machine and Lambda function described in the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial, we could pass the following input\.

```
{
  "comment": "This is a test of the input and output of a Task state.",
  "details": "Default example",
  "who": "AWS Step Functions"
}
```

The result of the Lambda function is the following\.

```
"Hello, AWS Step Functions!"
```

If we want to preserve the input, insert the result of the Lambda function, and then pass the combined JSON to the next state, we could set `ResultPath` to the following\.

```
"ResultPath": "$.taskresult"
```

This includes the result of the Lambda function with the original input\.

```
{
  "comment": "This is a test of input and output of a Task state.",
  "details": "Default behavior example",
  "who": "AWS Step Functions",
  "taskresult": "Hello, AWS Step Functions!"
}
```

The output of the Lambda function is appended to the original input as a value for `taskresult`\. The input, including the newly inserted value, is passed to the next state\.

You can also insert the result into a child node of the input\. Set the `ResultPath` to the following\.

```
"ResultPath": "$.strings.lambdaresult"
```

Start an execution using the following input\.

```
{
  "comment": "An input comment.",
  "strings": {
    "string1": "foo",
    "string2": "bar",
    "string3": "baz"
  },
  "who": "AWS Step Functions"
}
```

The result of the Lambda function is inserted as a child of the `strings` node in the input:

```
{
  "comment": "An input comment.",
  "strings": {
    "string1": "foo",
    "string2": "bar",
    "string3": "baz",
    "lambdaresult": "Hello, AWS Step Functions!"
  },
  "who": "AWS Step Functions"
}
```

The state output now includes the original input JSON with the result as a child node\.

## Use ResultPath to Update a Node in the Input with the Result<a name="input-output-resultpath-amend"></a>

The following diagram shows how `ResultPath` can update the value of existing JSON nodes in the input with values from the task result\.

![\[Replace input with ResultPath\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/input-output-resultpath-amend.png)![\[Replace input with ResultPath\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Replace input with ResultPath\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

Using the example of the state machine and Lambda function described in the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial, we could pass the following input\.

```
{
  "comment": "This is a test of the input and output of a Task state.",
  "details": "Default example",
  "who": "AWS Step Functions"
}
```

The result of the Lambda function is the following\.

```
Hello, AWS Step Functions!
```

Instead of preserving the input and inserting the result as a new node in the JSON, we can overwrite an existing node\. 

For example, just as omitting or setting `"ResultPath": "$"` overwrites the entire node, you can specify an individual node to overwrite with the result\.

```
"ResultPath": "$.comment"
```

Because the `comment` node already exists in the state input, setting `ResultPath` to `"$.comment"` replaces that node in the input with the result of the Lambda function\. Without further filtering by `OutputPath`, the following is passed to the output\.

```
{
  "comment": "Hello, AWS Step Functions!",
  "details": "Default behavior example",
  "who": "AWS Step Functions",
}
```

The value for the `comment` node, `"This is a test of the input and output of a Task state."`, is replaced by the result of the Lambda function: `"Hello, AWS Step Functions!"` in the state output\.

## Use ResultPath to Include Both Error and Input in a `Catch`<a name="input-output-resultpath-catch"></a>

The [Handling Error Conditions Using a State Machine](tutorial-handling-error-conditions.md) tutorial shows how to use a state machine to catch an error\. In some cases, you might want to preserve the original input with the error\. Use `ResultPath` in a `Catch` to include the error with the original input, instead of replacing it: 

```
"Catch": [{ 
  "ErrorEquals": ["States.ALL"], 
  "Next": "NextTask", 
  "ResultPath": "$.error" 
}]
```

If the previous `Catch` statement catches an error, it includes the result in an `error` node within the state input\. For example, with the following input:

```
{"foo": "bar"}
```

The state output when catching an error is the following\.

```
{
  "foo": "bar",
  "error": {
    "Error": "Error here"
  }
}
```

For more information about error handling, see:
+ [Error Handling](concepts-error-handling.md)
+ [Handling Error Conditions Using a State Machine](tutorial-handling-error-conditions.md)