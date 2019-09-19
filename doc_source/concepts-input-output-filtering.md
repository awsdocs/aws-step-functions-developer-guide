# Input and Output Processing in Step Functions<a name="concepts-input-output-filtering"></a>

A Step Functions execution receives a JSON file as input and passes that input to the first state in the workflow\. Individual states receive JSON as input and usually pass JSON as output to the next state\. Understanding how this information flows from state to state, and learning how to filter and manipulate this data, is key to effectively designing and implementing workflows in AWS Step Functions\. 

In the Amazon States Language, these fields filter and control the flow of JSON from state to state:
+ `InputPath`
+ `OutputPath`
+ `ResultPath`
+ `Parameters`

The following diagram shows how JSON information moves through a task state\. `InputPath` selects which parts of the JSON input to pass to the task of the `Task` state \(for example, an AWS Lambda function\)\. `ResultPath` then selects what combination of the state input and the task result to pass to the output\. `OutputPath` can filter the JSON output to further limit the information that's passed to the output\.

![\[Input and output processing\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/input-output-processing.png)![\[Input and output processing\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Input and output processing\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

`InputPath`, `Parameters`, `ResultPath`, and `OutputPath` each manipulate JSON as it moves through each state in your workflow\.

 Each can use [paths](amazon-states-language-paths.md) to select portions of the JSON from the input or the result\. A path is a string, beginning with `$`, that identifies nodes within JSON text\. Step Functions paths use [JsonPath](https://github.com/json-path/JsonPath) syntax\.

**Topics**
+ [Paths](amazon-states-language-paths.md)
+ [InputPath and Parameters](input-output-inputpath-params.md)
+ [ItemsPath](input-output-itemspath.md)
+ [ResultPath](input-output-resultpath.md)
+ [OutputPath](input-output-outputpath.md)
+ [InputPath, ResultPath and `OutputPath` Example](input-output-example.md)
+ [The Context Object](input-output-contextobject.md)