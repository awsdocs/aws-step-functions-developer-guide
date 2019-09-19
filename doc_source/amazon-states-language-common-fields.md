# Common State Fields<a name="amazon-states-language-common-fields"></a>

** `Type` \(Required\)**  
The state's type\.

** `Next` **  
The name of the next state that is run when the current state finishes\. Some state types, such as `Choice`, allow multiple transition states\.

** `End` **  
Designates this state as a terminal state \(ends the execution\) if set to `true`\. There can be any number of terminal states per state machine\. Only one of `Next` or `End` can be used in a state\. Some state types, such as `Choice`, don't support or use the `End` field\.

** `Comment` \(Optional\)**  
Holds a human\-readable description of the state\.

** `InputPath` \(Optional\)**  
A [path](concepts-input-output-filtering.md) that selects a portion of the state's input to be passed to the state's task for processing\. If omitted, it has the value `$` which designates the entire input\. For more information, see [Input and Output Processing](concepts-input-output-filtering.md)\)\.

** `OutputPath` \(Optional\)**  
A [path](concepts-input-output-filtering.md) that selects a portion of the state's input to be passed to the state's output\. If omitted, it has the value `$` which designates the entire input\. For more information, see [Input and Output Processing](concepts-input-output-filtering.md)\.