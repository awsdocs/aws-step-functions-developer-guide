# InputPath, ResultPath and `OutputPath` Example<a name="input-output-example"></a>

Any state other than a `Fail` state can include `InputPath`, `ResultPath` or `OutputPath`\. These allow you to use a path to filter the JSON as it moves through your workflow\. 

For example, start with the AWS Lambda function and state machine described in the [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial\. Modify the state machine so that it includes the following `InputPath`, `ResultPath`, and `OutputPath`\.

```
{
  "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
  "StartAt": "HelloWorld",
  "States": {
    "HelloWorld": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
      "InputPath": "$.lambda",
      "ResultPath": "$.data.lambdaresult",
      "OutputPath": "$.data",
      "End": true
    }
  }
}
```

Start an execution using the following input\.

```
{
  "comment": "An input comment.",
  "data": {
    "val1": 23,
    "val2": 17
  },
  "extra": "foo",
  "lambda": {
    "who": "AWS Step Functions"
  }
}
```

Assume that the `comment` and `extra` nodes can be discarded, but that we want to include the output of the Lambda function, and preserve the information in the `data` node\.

In the updated state machine, the `Task` state is altered to process the input to the task\.

```
"InputPath": "$.lambda",
```

This line in the state machine definition limits the task input to only the `lambda` node from the state input\. The Lambda function receives only the JSON object `{"who": "AWS Step Functions"}` as input\. 

```
"ResultPath": "$.data.lambdaresult",
```

This `ResultPath` tells the state machine to insert the result of the Lambda function into a node named `lambdaresult`, as a child of the `data` node in the original state machine input\. Without further processing with `OutputPath`, the input of the state now includes the result of the Lambda function with the original input\.

```
{
  "comment": "An input comment.",
  "data": {
    "val1": 23,
    "val2": 17,
    "lambdaresult": "Hello, AWS Step Functions!"
  },
  "extra": "foo",
  "lambda": {
    "who": "AWS Step Functions"
  }
}
```

But, our goal was to preserve only the `data` node, and include the result of the Lambda function\. `OutputPath` filters this combined JSON before passing it to the state output\.

```
"OutputPath": "$.data",
```

This selects only the `data` node from the original input \(including the `lambdaresult` child inserted by `ResultPath`\) to be passed to the output\. The state output is filtered to the following\.

```
{
  "val1": 23,
  "val2": 17,
  "lambdaresult": "Hello, AWS Step Functions!"
}
```

In this `Task` state:

1. `InputPath` sends only the `lambda` node from the input to the Lambda function\.

1. `ResultPath` inserts the result as a child of the `data` node in the original input\.

1. `OutputPath` filters the state input \(which now includes the result of the Lambda function\) so that it passes only the `data` node to the state output\.

For more information, see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.