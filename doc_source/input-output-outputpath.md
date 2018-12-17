# `OutputPath`<a name="input-output-outputpath"></a>

`OutputPath` allows you to select a portion of the state output to pass to the next state\. This allows you to filter out unwanted information, and pass only the portion of JSON that you care about\.

If you do not specify an `OutputPath` the default value is `$`, which passes the entire JSON node \(determined by the state input, the task result, and `ResultPath`\) to the next state\.

For more information, see:
+ [Paths in the Amazon States Language](amazon-states-language-input-output-processing.md#amazon-states-language-paths)
+ [`InputPath`, `ResultPath` and `OutputPath` Example](input-output-example.md)
+ [Pass Static JSON as Parameters](connectors-parameters.md#connectors-parameters-json)
+ [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)