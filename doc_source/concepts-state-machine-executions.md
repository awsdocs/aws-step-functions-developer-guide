# Executions<a name="concepts-state-machine-executions"></a>

A state machine *execution* occurs when an AWS Step Functions state machine runs and performs its tasks\. Each Step Functions state machine can have multiple simultaneous executions, which you can initiate from the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/), or by using the AWS SDKs, the Step Functions API actions, or the AWS CLI\. An execution receives JSON input and produces JSON output\. You can start a Step Functions execution in the following ways\.
+ Call the [https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) API action\.
+ [Start a new execution](getting-started.md#start-new-execution) in the Step Functions console\.
+ Use [Amazon CloudWatch Events](tutorial-cloudwatch-events-s3.md) to start an execution\.
+ Start an execution with [Amazon API Gateway](tutorial-api-gateway.md)\.
+ Start a [nested workflow execution](concepts-nested-workflows.md) from a `Task` state\.

For more information about the different ways of working with Step Functions, see [Development Options](development-options.md)\. 