# States<a name="concepts-states"></a>

A finite state machine can express an algorithm as a number of states, their relationships, and their input and output\. AWS Step Functions allows you to coordinate individual tasks by expressing your workflow as a finite state machine, written in the Amazon States Language\.

Individual states can make decisions based on their input, perform actions, and pass output to other states\. In Step Functions you express your workflows in the Amazon States Language, and the Step Functions console provides a graphical representation of that state machine to help visualize your application logic\.

States are elements in your state machine\. A state is referred to by its *name*, which can be any string, but which must be unique within the scope of the entire state machine\.

**Note**  
An instance of a state exists until the end of its execution\.

States can perform a variety of functions in your state machine:
+ Do some work in your state machine \(a [Task](concepts-tasks.md) state\)\.
+ Make a choice between branches of execution \(a [Choice](amazon-states-language-choice-state.md) state\)
+ Stop an execution with a failure or success \(a [Fail](amazon-states-language-fail-state.md) or [Succeed](amazon-states-language-succeed-state.md) state\)
+ Simply pass its input to its output or inject some fixed data \(a [Pass](amazon-states-language-pass-state.md) state\)
+ Provide a delay for a certain amount of time or until a specified time/date \(a [Wait](amazon-states-language-wait-state.md) state\)
+ Begin parallel branches of execution \(a [Parallel](amazon-states-language-parallel-state.md) state\)

The following is an example state named `HelloWorld` that performs an AWS Lambda function\.

```
"HelloWorld": {
  "Type": "Task",
  "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
  "Next": "AfterHelloWorldState",
  "Comment": "Run the HelloWorld Lambda function"
}
```

States share many common features:
+ Each state must have a `Type` field indicating what type of state it is\.
+ Each state can have an optional `Comment` field to hold a human\-readable comment about, or description of, the state\.
+ Each state \(except a `Succeed` or `Fail` state\) requires a `Next` field or, alternatively, can become a terminal state by specifying an `End` field\.
**Note**  
A `Choice` state may have more than one `Next`, but only one within each Choice Rule\. A `Choice` state cannot use `End`\.

Certain state types require additional fields, or may redefine common field usage\.

For more information regarding the various states that you can define using Amazon States Language, see [States](amazon-states-language-states.md)\.

After you create a state machine and have executed it, you can access information about each state, its input and output, when it was active and for how long, by viewing the **Execution Details** page on the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.