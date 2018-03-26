# Common State Fields<a name="amazon-states-language-common-fields"></a>

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