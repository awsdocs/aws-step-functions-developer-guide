# States<a name="concepts-states"></a>

Individual states can make decisions based on their input, perform actions, and pass output to other states\. In Step Functions you express your workflows in the Amazon States Language, and the Step Functions console provides a graphical representation of that state machine to help visualize your application logic\.

States are elements in your state machine\. A state is referred to by its *name*, which can be any string, but which must be unique within the scope of the entire state machine\.

States can perform a variety of functions in your state machine:
+ Do some work in your state machine \(a [Task](amazon-states-language-task-state.md) state\)\.
+ Make a choice between branches of execution \(a [Choice](amazon-states-language-choice-state.md) state\)
+ Stop an execution with a failure or success \(a [Fail](amazon-states-language-fail-state.md) or [Succeed](amazon-states-language-succeed-state.md) state\)
+ Simply pass its input to its output or inject some fixed data \(a [Pass](amazon-states-language-pass-state.md) state\)
+ Provide a delay for a certain amount of time or until a specified time/date \(a [Wait](amazon-states-language-wait-state.md) state\)
+ Begin parallel branches of execution \(a [Parallel](amazon-states-language-parallel-state.md) state\)
+ Dynamically iterate steps using a [Map](amazon-states-language-map-state.md) state

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

After you create a state machine and have executed it, you can access information about each state, its input and output, when it was active and for how long, by viewing the **Execution Details** page on the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

**Topics**
+ [Amazon States Language](concepts-amazon-states-language.md)
+ [Pass](amazon-states-language-pass-state.md)
+ [Task](amazon-states-language-task-state.md)
+ [Choice](amazon-states-language-choice-state.md)
+ [Wait](amazon-states-language-wait-state.md)
+ [Succeed](amazon-states-language-succeed-state.md)
+ [Fail](amazon-states-language-fail-state.md)
+ [Parallel](amazon-states-language-parallel-state.md)
+ [Map](amazon-states-language-map-state.md)