# Transitions<a name="concepts-transitions"></a>

When an execution of a state machine is launched, the system begins with the state referenced in the top\-level `StartAt` field\. This field \(a string\) must exactly match, including case, the name of one of the states\.

After executing a state, AWS Step Functions uses the value of the `Next` field to determine the next state to advance to\.

 `Next` fields also specify state names as strings, and must match the name of a state specified in the state machine description exactly \(case sensitive\)\.

For example, the following state includes a transition to `NextState`\.

```
"SomeState" : {
  ...,
  "Next" : "NextState"
}
```

Most states permit only a single transition rule via the `Next` field\. However, certain flow\-control states \(for example, a `Choice` state\) allow you to specify multiple transition rules, each with its own `Next` field\. The [Amazon States Language](concepts-amazon-states-language.md) provides details about each of the state types you can specify, including information about how to specify transitions\.

States can have multiple incoming transitions from other states\.

The process repeats until it reaches a terminal state \(a state with `"Type": Succeed`, `"Type": Fail`, or `"End": true`\), or a runtime error occurs\.

The following rules apply to states within a state machine:
+ States can occur in any order within the enclosing block, but the order in which they're listed doesn't affect the order in which they're run\. That order is determined by the contents of the states\.
+ Within a state machine, there can be only one state designated as the `start` state, which is designated by the value of the `StartAt` field in the top\-level structure\.
+ Depending on your state machine logic—for example, if your state machine has multiple branches of execution—you may have more than one `end` state\.
+ If your state machine consists of only one state, it can be both the `start` state and the `end` state\.