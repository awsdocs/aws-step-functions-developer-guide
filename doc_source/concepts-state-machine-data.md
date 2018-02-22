# State Machine Data<a name="concepts-state-machine-data"></a>

State Machine data takes the following forms:

+ The initial input into a state machine

+ Data passed between states

+ The output from a state machine

This section describes how state machine data is formatted and used in AWS Step Functions\.



## Data Format<a name="concepts-state-machine-data-format"></a>

State machine data is represented by JSON text, so you can provide values using any data type supported by JSON: 

**Note**  
Numbers in JSON text format conform to JavaScript semantics\. These numbers typically correspond to double\-precision [IEEE\-854](https://standards.ieee.org/findstds/standard/854-1987.html) values\.
The following is valid JSON text: stand\-alone, quote\-delimited strings; objects; arrays; numbers; Boolean values; and `null`\.
The output of a state becomes the input into the next state\. However, you can restrict states to working on a subset of the input data by using [Input and Output Processing](amazon-states-language-input-output-processing.md)\.

## State Machine Input/Output<a name="concepts-state-machine-data-state-machine-input-output"></a>

You can give AWS Step Functions initial input data by passing it to a `[StartExecution](http://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html)` action when you start an execution, or by passing initial data using the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\. Initial data is passed to the state machine's `StartAt` state\. If no input is provided, the default is an empty object \(`{}`\)\.

The output of the execution is returned by the last state \(`terminal`\)\. This output appears as JSON text in the execution's result\. You can retrieve execution results from the execution history using external callers \(for example, in the `[DescribeExecution](http://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html)` action\)\. You can view execution results on the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

## State Input/Output<a name="concepts-state-machine-data-state-input-output"></a>

Each state's input consists of JSON text from the preceding state or, for the `StartAt` state, the input into the execution\. Certain flow\-control states echo their input to their output\.

In the following example, the state machine adds two numbers together:

1. Define the Lambda function\.

   ```
   function Add(input) {
     var numbers = JSON.parse(input).numbers;
     var total = numbers.reduce(
       function(previousValue, currentValue, index, array) {
         return previousValue + currentValue; });
     return JSON.stringify({ result: total });
   }
   ```

1. Define the state machine\.

   ```
   {
     "Comment": "An example that adds two numbers together.",
     "StartAt": "Add",
     "Version": "1.0",
     "TimeoutSeconds": 10,
     "States":
       {
           "Add": {
             "Type": "Task",
             "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Add",
             "End": true
           }
       }
   }
   ```

1. Start an execution with the following JSON text:

   ```
   { "numbers": [3, 4] }
   ```

   The `Add` state receives the JSON text and passes it to the Lambda function

   The Lambda function returns the result of the calculation to the state\.

   The state returns the following value in its output:

   ```
   { "result": 7 }
   ```

   Because `Add` is also the final state in the state machine, this value is returned as the state machine's output\.

   If the final state returns no output, then the state machine returns an empty object \(`{}`\)\.

### Filters<a name="concepts-state-machine-data-filters"></a>

Some states, such as [Task](amazon-states-language-task-state.md), have `InputPath`, `ResultPath`, and `OutputPath` fields\. The values of these fields are [path](amazon-states-language-input-output-processing.md)\.

+ The `InputPath` field selects a portion of the state's input to pass into the state's processing logic \(an activity, Lambda function, or so on\)\. If the field is `null`, an empty object \(`{}`\) is passed\.
**Note**  
If you omit the `InputPath` field, the entire state input \(`$`\) is selected by default\.

+ The `ResultPath` field selects a portion of the state's input to overwrite with, or add to, result data from the state's processing logic\.

  The `ResultPath` field's value can be `null`, which causes any output from your state's processing logic to be discarded instead of being added to the state's input\. In this scenario, the state's output is identical to the state's input, given the default value for the `OutputPath` field\.
**Note**  
If you omit the optional `ResultPath` field, it defaults to `$`, which overwrites the entire input\. However, you can select a portion of the input using the `OutputPath` field before the input is sent as the state's output\.

+ If the `OutputPath` field's value is `null`, an empty object \(`{}`\) is sent as the state's output\.
**Note**  
If you omit the optional `OutputPath` field, it defaults to `$`, which selects the entire input \(as modified by the `ResultPath` field\), and sends the input as the state's output\.

In the following example, a state outputs the sum of its input values\.

1. Define the `InputPath`, `ResultPath`, and `OutputPath`\.

   ```
   "InputPath": "$.numbers",
   "ResultPath": "$.sum",
   "OutputPath": "$"
   ```

1. Add state input data\.

   ```
   {
     "numbers": [3, 4]
   }
   ```

   The state output data is as follows:

   ```
   {
     "numbers": [3, 4],
     "sum": 7
   }
   ```

1. Adjust the `OutputPath` without changing the state input data\.

   ```
   "InputPath": "$.numbers",
   "ResultPath": "$.sum",
   "OutputPath": "$.sum"
   ```

   The state output data changes as follows:

   ```
   { 7 }
   ```

You can separate the names of data members in your state machine data from the functions that process the data using the `InputPath` and `ResultPath` fields\.