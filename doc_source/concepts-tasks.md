# Tasks<a name="concepts-tasks"></a>

All work in your state machine is done by *tasks*\. A task performs work by using an activity, a Lambda function, or by passing parameters to the API actions of other services\.
+ AWS Step Functions can invoke Lambda functions directly from a task state\. A Lambda function is a cloud\-native task that runs on AWS Lambda\. You can write Lambda functions in a variety of programming languages, using the AWS Management Console or by uploading code to Lambda\. For more information see [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md)\.
+ Step Functions can coordinate some AWS services directly from a task state\. For more information see [Service Integrations](concepts-connectors.md)\.
+ An activity consists of program code that waits for an operator to perform an action or to provide input\. You can host activities on Amazon EC2, on Amazon ECS, or even on mobile devices\. Activities poll Step Functions using the `GetActivityTask` and `SendTaskSuccess`, `SendTaskFailure`, and `SendTaskHeartbeat` API actions\.

Amazon States Language represents tasks by setting a state's type to `Task` and by providing the task with the ARN of the activity or Lambda function\. For more information about specifying task types, see [Task](amazon-states-language-task-state.md) in [Amazon States Language](concepts-amazon-states-language.md)\.

For examples of how different kinds of tasks are used, see the [Tutorials](tutorials.md) section\.