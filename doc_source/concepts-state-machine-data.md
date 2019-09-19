# State Machine Data<a name="concepts-state-machine-data"></a>

State machine data takes the following forms:
+ The initial input into a state machine
+ Data passed between states
+ The output from a state machine

This section describes how state machine data is formatted and used in AWS Step Functions\.

**Topics**
+ [Data Format](#concepts-state-machine-data-format)
+ [State Machine Input/Output](#concepts-state-machine-data-state-machine-input-output)
+ [State Input/Output](#concepts-state-machine-data-state-input-output)

## Data Format<a name="concepts-state-machine-data-format"></a>

State machine data is represented by JSON text, so you can provide values using any data type supported by JSON: 

**Note**  
Numbers in JSON text format conform to JavaScript semantics\. These numbers typically correspond to double\-precision [IEEE\-854](https://standards.ieee.org/findstds/standard/854-1987.html) values\.
The following is valid JSON text: standalone, quote\-delimited strings; objects; arrays; numbers; Boolean values; and `null`\.
The output of a state becomes the input into the next state\. However, you can restrict states to working on a subset of the input data by using [Input and Output Processing](concepts-input-output-filtering.md)\.

## State Machine Input/Output<a name="concepts-state-machine-data-state-machine-input-output"></a>

You can give AWS Step Functions initial input data by passing it to a `[StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html)` action when you start an execution, or by passing initial data using the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\. Initial data is passed to the state machine's `StartAt` state\. If no input is provided, the default is an empty object \(`{}`\)\.

The output of the execution is returned by the last state \(`terminal`\)\. This output appears as JSON text in the execution's result\. You can retrieve execution results from the execution history using external callers \(for example, in the `[DescribeExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html)` action\)\. You can view execution results on the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

## State Input/Output<a name="concepts-state-machine-data-state-input-output"></a>

Each state's input consists of JSON text from the preceding state or, for the `StartAt` state, the input into the execution\. Certain flow\-control states echo their input to their output\.

In the following example, the state machine adds two numbers together\.

1. Define the AWS Lambda function\.

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

1. Start an execution with the following JSON text\.

   ```
   { "numbers": [3, 4] }
   ```

   The `Add` state receives the JSON text and passes it to the Lambda function\.

   The Lambda function returns the result of the calculation to the state\.

   The state returns the following value in its output\.

   ```
   { "result": 7 }
   ```

   Because `Add` is also the final state in the state machine, this value is returned as the state machine's output\.

   If the final state returns no output, then the state machine returns an empty object \(`{}`\)\.

For more information, see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.