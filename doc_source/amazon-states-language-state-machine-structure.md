# State Machine Structure<a name="amazon-states-language-state-machine-structure"></a>

State machines are defined using JSON text that represents a structure containing the following fields\.

** `Comment` \(Optional\)**  
A human\-readable description of the state machine\.

** `StartAt` \(Required\)**  
A string that must exactly match \(is case sensitive\) the name of one of the state objects\.

** `TimeoutSeconds` \(Optional\)**  
The maximum number of seconds an execution of the state machine can run\. If it runs longer than the specified time, the execution fails with a `States.Timeout` [Error Name](concepts-error-handling.md#error-handling-error-representation)\.

** `Version` \(Optional\)**  
The version of the Amazon States Language used in the state machine \(default is "1\.0"\)\.

** `States` \(Required\)**  
An object containing a comma\-delimited set of states\.

The `States` field contains [States](concepts-states.md)\.

```
{
    "State1" : {
    },

    "State2" : {
    },
    ...
}
```

A state machine is defined by the states it contains and the relationships between them\.

The following is an example\.

```
{
  "Comment": "A Hello World example of the Amazon States Language using a Pass state",
  "StartAt": "HelloWorld",
  "States": {
    "HelloWorld": {
      "Type": "Pass",
      "Result": "Hello World!",
      "End": true
    }
  }
}
```

When an execution of this state machine is launched, the system begins with the state referenced in the `StartAt` field \(`"HelloWorld"`\)\. If this state has an `"End": true` field, the execution stops and returns a result\. Otherwise, the system looks for a `"Next":` field and continues with that state next\. This process repeats until the system reaches a terminal state \(a state with `"Type": "Succeed"`, `"Type": "Fail"`, or `"End": true`\), or a runtime error occurs\.

The following rules apply to states within a state machine:
+ States can occur in any order within the enclosing block, but the order in which they're listed doesn't affect the order in which they're run\. The contents of the states determines this order\.
+ Within a state machine, there can be only one state that's designated as the `start` state, designated by the value of the `StartAt` field in the top\-level structure\. This state is the one that is executed first when the execution starts\.
+ Any state for which the `End` field is `true` is considered an `end` \(or `terminal`\) state\. Depending on your state machine logic—for example, if your state machine has multiple branches of execution—you might have more than one `end` state\.
+ If your state machine consists of only one state, it can be both the `start` state and the `end` state\.