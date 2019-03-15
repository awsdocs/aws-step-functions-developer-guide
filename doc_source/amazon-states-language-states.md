# States<a name="amazon-states-language-states"></a>

States are top\-level elements within a state machine's `States` field, and can have different roles in your state machine, depending on their type\.

```
"FirstState" : {
   "Type" : "Task",
   ...
}
```

States are identified by their names, which must be unique within the state machine specification, but otherwise can be any valid string in JSON text format\. Each state also contains fields with options that vary according to the contents of the state's required `Type` field\.

**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

**Topics**
+ [Common State Fields](amazon-states-language-common-fields.md)
+ [Pass](amazon-states-language-pass-state.md)
+ [Task](amazon-states-language-task-state.md)
+ [Choice](amazon-states-language-choice-state.md)
+ [Wait](amazon-states-language-wait-state.md)
+ [Succeed](amazon-states-language-succeed-state.md)
+ [Fail](amazon-states-language-fail-state.md)
+ [Parallel](amazon-states-language-parallel-state.md)