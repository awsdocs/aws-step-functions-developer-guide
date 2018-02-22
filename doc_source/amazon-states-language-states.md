# States<a name="amazon-states-language-states"></a>

States are top\-level elements within a state machine's `States` field, and can take a number of different roles in your state machine depending on their type\.

```
"FirstState" : {
   "Type" : "Task",
   ...
}
```

States are identified by their name, which must be unique within the state machine specification, but otherwise can be any valid string in JSON text format\. Each state also contains a number of fields with options that vary according to the contents of the state's required `Type` field\.

**Note**  
State machine names must be 1—80 characters in length, must be unique for your account and region, and must not contain any of the following:  
Whitespace
Whitespace characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.



## Common State Fields<a name="amazon-states-language-common-fields"></a>

** `Type` \(Required\)**  
The state's type\.

** `Next` **  
The name of the next state that will be run when the current state finishes\. Some state types, such as `Choice`, allow multiple transition states\.

** `End` **  
Designates this state as a terminal state \(it ends the execution\) if set to `true`\. There can be any number of terminal states per state machine\. Only one of `Next` or `End` can be used in a state\. Some state types, such as `Choice`, do not support or use the `End` field\.

** `Comment` \(Optional\)**  
Holds a human\-readable description of the state\.

** `InputPath` \(Optional\)**  
A [path](amazon-states-language-input-output-processing.md) that selects a portion of the state's input to be passed to the state's task for processing\. If omitted, it has the value `$` which designates the entire input\. For more information, see [Input and Output Processing](amazon-states-language-input-output-processing.md)\)\.

** `OutputPath` \(Optional\)**  
A [path](amazon-states-language-input-output-processing.md) that selects a portion of the state's input to be passed to the state's output\. If omitted, it has the value `$` which designates the entire input\. For more information, see [Input and Output Processing](amazon-states-language-input-output-processing.md)\.