# Wait<a name="amazon-states-language-wait-state"></a>

A `Wait` state \(`"Type": "Wait"`\) delays the state machine from continuing for a specified time\. You can choose either a relative time, specified in seconds from when the state begins, or an absolute end\-time, specified as a timestamp\.

In addition to the [common state fields](amazon-states-language-common-fields.md), `Wait` states have one of the following fields:

** `Seconds` **  
A time, in seconds, to wait before beginning the state specified in the `Next` field\.

** `Timestamp` **  
An absolute time to wait until before beginning the state specified in the `Next` field\.  
Timestamps must conform to the RFC3339 profile of ISO 8601, with the further restrictions that an uppercase `T` must separate the date and time portions, and an uppercase `Z` must denote that a numeric time zone offset is not present, for example, `2016-08-18T17:33:00Z`\.

** `SecondsPath` **  
A time, in seconds, to wait before beginning the state specified in the `Next` field, specified using a [path](amazon-states-language-input-output-processing.md) from the state's input data\.

** `TimestampPath` **  
An absolute time to wait until before beginning the state specified in the `Next` field, specified using a [path](amazon-states-language-input-output-processing.md) from the state's input data\.

**Note**  
You must specify exactly one of `Seconds`, `Timestamp`, `SecondsPath`, or `TimestampPath`\.

For example, the following `Wait` state introduces a ten second delay into a state machine:

```
"wait_ten_seconds": {
  "Type": "Wait",
  "Seconds": 10,
  "Next": "NextState"
}
```

In the next example, the `Wait` state waits until an absolute time: March 14th, 2016, at 1:59 PM UTC\.

```
"wait_until" : {
  "Type": "Wait",
  "Timestamp": "2016-03-14T01:59:00Z",
  "Next": "NextState"
}
```

The wait duration does not have to be hard\-coded\. For example, given the following input data:

```
{
  "expirydate": "2016-03-14T01:59:00Z"
}
```

You can select the value of "expirydate" from the input using a reference [path](amazon-states-language-input-output-processing.md) to select it from the input data:

```
"wait_until" : {
    "Type": "Wait",
    "TimestampPath": "$.expirydate",
    "Next": "NextState"
}
```