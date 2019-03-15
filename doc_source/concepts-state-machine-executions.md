# Executions<a name="concepts-state-machine-executions"></a>

A state machine *execution* occurs when an AWS Step Functions state machine runs and performs its tasks\. Each Step Functions state machine can have multiple simultaneous executions, which you can initiate from the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/), or by using the AWS SDKs, the Step Functions API actions, or the AWS CLI\. An execution receives JSON input and produces JSON output\.

For more information about the different ways of working with Step Functions, see [Development Options](development-options.md)\. 

For more information about initiating an execution from the Step Functions console, see [To start a new execution](getting-started.md#start-new-execution)\.