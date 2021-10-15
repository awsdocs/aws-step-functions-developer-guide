# OutputPath<a name="input-output-outputpath"></a>

`OutputPath` enables you to select a portion of the state output to pass to the next state\. This enables you to filter out unwanted information, and pass only the portion of JSON that you care about\.

If you don't specify an `OutputPath` the default value is `$`\. This passes the entire JSON node \(determined by the state input, the task result, and `ResultPath`\) to the next state\.

**Tip**  
Use the [ data flow simulator in the Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/simulator) to test JSON path syntax, to better understand how data is manipulated within a state, and to see how data is passed between states\.

For more information, see the following:
+ [Paths in the Amazon States Language](amazon-states-language-paths.md)
+ [InputPath, ResultPath and `OutputPath` Example](input-output-example.md)
+ [Pass Static JSON as Parameters](connect-parameters.md#connect-parameters-json)
+ [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)