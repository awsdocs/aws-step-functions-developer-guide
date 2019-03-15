# Fail<a name="amazon-states-language-fail-state"></a>

A `Fail` state \(`"Type": "Fail"`\) stops the execution of the state machine and marks it as a failure\.

The `Fail` state only allows the use of `Type` and `Comment` fields from the set of [common state fields](amazon-states-language-common-fields.md)\. In addition, the `Fail` state allows the following fields\.

** `Cause` \(Optional\)**  
Provides a custom failure string that can be used for operational or diagnostic purposes\.

** `Error` \(Optional\)**  
Provides an error name that can be used for error handling \(`Retry/Catch`\), operational, or diagnostic purposes\.

Because `Fail` states always exit the state machine, they have no `Next` field and don't require an `End` field\.

For example:

```
"FailState": {
  "Type": "Fail",
  "Cause": "Invalid response.",
  "Error": "ErrorA"
}
```