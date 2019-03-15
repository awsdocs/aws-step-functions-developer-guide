# Succeed<a name="amazon-states-language-succeed-state"></a>

A `Succeed` state \(`"Type": "Succeed"`\) stops an execution successfully\. The `Succeed` state is a useful target for `Choice` state branches that don't do anything but stop the execution\.

Because `Succeed` states are terminal states, they have no `Next` field, and don't need an `End` field, for example:

```
"SuccessState": {
  "Type": "Succeed"
}
```