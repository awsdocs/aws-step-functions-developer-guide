# Read Consistency<a name="concepts-read-consistency"></a>

State machine updates in AWS Step Functions are eventually consistent\. All `StartExecution` calls within a few seconds will use the updated definition and `roleArn` \(the Amazon Resource Name for the IAM role\)\. Executions started immediately after calling `UpdateStateMachine` might use the previous state machine definition and `roleArn`\.

For more information, see:
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_UpdateStateMachine.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_UpdateStateMachine.html) in the *AWS Step Functions API Reference*
+ [Update a state machine](getting-started.md#update-state-machine) in [Getting Started](getting-started.md)