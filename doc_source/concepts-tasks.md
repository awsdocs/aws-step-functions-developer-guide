# Tasks<a name="concepts-tasks"></a>

All work in your state machine is done by *tasks*\. A task can be an activity or a Lambda function\.
+ An activity consists of program code that waits for an operator to perform an action or to provide input\. You can host activities on Amazon EC2, on Amazon ECS, or even on mobile devices\. Activities poll Step Functions using the `GetActivityTask` and `SendTaskSuccess`, `SendTaskFailure`, and `SendTaskHeartbeat` API actions\.
+ A [Lambda function](tutorial-creating-lambda-state-machine.md) is a cloud\-native task that runs on AWS Lambda\. You can write Lambda functions in a variety of programming languages, using the AWS Management Console or by uploading code to Lambda\.

Amazon States Language represents tasks by setting a state's type to `Task` and by providing the task with the ARN of the activity or Lambda function\. For more information about specifying task types, see [Task](amazon-states-language-task-state.md) in [Amazon States Language](concepts-amazon-states-language.md)\.

To see a list of your tasks, use the **Tasks** page in the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.