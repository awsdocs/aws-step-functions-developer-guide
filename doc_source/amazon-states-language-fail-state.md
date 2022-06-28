# Fail<a name="amazon-states-language-fail-state"></a>

A `Fail` state \(`"Type": "Fail"`\) stops the execution of the state machine and marks it as a failure, unless it is caught by a `Catch` block\.

The `Fail` state only allows the use of `Type` and `Comment` fields from the set of [common state fields](amazon-states-language-common-fields.md)\. In addition, the `Fail` state allows the following fields\.

** `Cause` \(Optional\)**  
A custom failure string that you can specify for operational or diagnostic purposes\.

** `Error` \(Optional\)**  
An error name that you can provide for operational or diagnostic purposes\.

**Note**  
You cannot specify values for the `Cause` and `Error` fields by appending `.$` to these fields, such as `Cause.$` and `Error.$`\.

Because `Fail` states always exit the state machine, they have no `Next` field and don't require an `End` field\.

The following is an example\.

```
"FailState": {
  "Type": "Fail",
  "Cause": "Invalid response.",
  "Error": "ErrorA"
}
```