# Input and Output Processing in Step Functions<a name="concepts-input-output-filtering"></a>

A Step Functions execution receives a JSON file as input and passes that input to the first state in the workflow\. Individual states receive JSON as input and usually pass JSON as output to the next state\. Understanding how this information flows from state to state, and learning how to filter and manipulate this data, is key to effectively designing and implementing workflows in AWS Step Functions\. 

In the Amazon States Language, three fields filter and control the flow of JSON from state to state:
+ `InputPath`
+ `OutputPath`
+ `ResultPath`

The following diagram shows how JSON information moves through a task state\. `InputPath` selects which parts of the JSON input to pass to the task of the `Task` state \(for example, an AWS Lambda function\)\. `ResultPath` then selects what combination of the state input and the task result to pass to the output\. `OutputPath` can filter the JSON output to further limit the information that is passed to the output\.

![\[Input and output processing\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/input-output-processing.png)![\[Input and output processing\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Input and output processing\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

`InputPath`, `OutputPath`, and `ResultPath` each use [paths](amazon-states-language-input-output-processing.md#amazon-states-language-paths) to manipulate JSON as it moves through each state in your workflow\. A path is a string, beginning with `$`, that identifies nodes within JSON text\. Step Functions paths use [JsonPath](https://github.com/json-path/JsonPath) syntax\.

**Note**  
 `ResultPath` is limited to using [reference paths](amazon-states-language-input-output-processing.md#amazon-states-language-reference-paths), which limit scope so that it can identify only a single node in JSON\. See [Reference Paths](amazon-states-language-input-output-processing.md#amazon-states-language-reference-paths) in the [Amazon States Language](concepts-amazon-states-language.md)\.

**Topics**
+ [Understanding `ResultPath`](input-output-resultpath.md)
+ [Filtering with `InputPath` and `OutputPath`](input-output-paths.md)