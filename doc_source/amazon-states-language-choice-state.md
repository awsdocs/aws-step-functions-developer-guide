# Choice<a name="amazon-states-language-choice-state"></a>

A `Choice` state \(`"Type": "Choice"`\) adds branching logic to a state machine\.

In addition to the [common state fields](amazon-states-language-common-fields.md), `Choice` states introduce the following additional fields\.

** `Choices` \(Required\)**  
An array of [Choice Rules](#amazon-states-language-choice-state-rules) that determines which state the state machine transitions to next\.

** `Default` \(Optional, Recommended\)**  
The name of the state to transition to if none of the transitions in `Choices` is taken\.

**Important**  
 `Choice` states don't support the `End` field\. In addition, they use `Next` only inside their `Choices` field\.

## Choice State Example<a name="choice-state-example"></a>

The following is an example of a `Choice` state and other states that it transitions to\.

**Note**  
You must specify the `$.type` field\. If the state input doesn't contain the `$.type` field, the execution fails and an error is displayed in the execution history\.

```
"ChoiceStateX": {
  "Type": "Choice",
  "Choices": [
    {
        "Not": {
          "Variable": "$.type",
          "StringEquals": "Private"
        },
        "Next": "Public"
    },
    {
      "Variable": "$.value",
      "NumericEquals": 0,
      "Next": "ValueIsZero"
    },
    {
      "And": [
        {
          "Variable": "$.value",
          "NumericGreaterThanEquals": 20
        },
        {
          "Variable": "$.value",
          "NumericLessThan": 30
        }
      ],
      "Next": "ValueInTwenties"
    }
  ],
  "Default": "DefaultState"
},

"Public": {
  "Type" : "Task",
  "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Foo",
  "Next": "NextState"
},

"ValueIsZero": {
  "Type" : "Task",
  "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Zero",
  "Next": "NextState"
},

"ValueInTwenties": {
  "Type" : "Task",
  "Resource": "arn:aws:lambda:us-east-1:123456789012:function:Bar",
  "Next": "NextState"
},

"DefaultState": {
  "Type": "Fail",
  "Cause": "No Matches!"
}
```

In this example, the state machine starts with the following input value\.

```
{
  "type": "Private",
  "value": 22
}
```

Step Functions transitions to the `ValueInTwenties` state, based on the `value` field\.

If there are no matches for the `Choice` state's `Choices`, the state provided in the `Default` field runs instead\. If the `Default` state isn't specified, the execution fails with an error\.

## Choice Rules<a name="amazon-states-language-choice-state-rules"></a>

A `Choice` state must have a `Choices` field whose value is a non\-empty array, and whose every element is an object called a Choice Rule\. A Choice Rule contains the following:
+ A **comparison** – Two fields that specify an input variable to compare, the type of comparison, and the value to compare the variable to\.
+ A **`Next` field** – The value of this field must match a state name in the state machine\.

The following example checks whether the numerical value is equal to `1`\.

```
{
  "Variable": "$.foo",
  "NumericEquals": 1,
  "Next": "FirstMatchState"
}
```

The following example checks whether the string is equal to `MyString`\.

```
{
  "Variable": "$.foo",
  "StringEquals": "MyString",
  "Next": "FirstMatchState"
}
```

The following example checks whether the string is greater than `MyStringABC`\.

```
{
  "Variable": "$.foo",
  "StringGreaterThan": "MyStringABC",
  "Next": "FirstMatchState"
}
```

The following example checks whether the timestamp is equal to `2001-01-01T12:00:00Z`\.

```
{
  "Variable": "$.foo",
  "TimestampEquals": "2001-01-01T12:00:00Z",
  "Next": "FirstMatchState"
}
```

Step Functions examines each of the Choice Rules in the order listed in the `Choices` field\. Then it transitions to the state specified in the `Next` field of the first Choice Rule in which the variable matches the value according to the comparison operator\.

The following comparison operators are supported:
+ `And`
+ `BooleanEquals`
+ `Not`
+ `NumericEquals`
+ `NumericGreaterThan`
+ `NumericGreaterThanEquals`
+ `NumericLessThan`
+ `NumericLessThanEquals`
+ `Or`
+ `StringEquals`
+ `StringGreaterThan`
+ `StringGreaterThanEquals`
+ `StringLessThan`
+ `StringLessThanEquals`
+ `TimestampEquals`
+ `TimestampGreaterThan`
+ `TimestampGreaterThanEquals`
+ `TimestampLessThan`
+ `TimestampLessThanEquals`

For each of these operators, the corresponding value must be of the appropriate type: string, number, Boolean, or timestamp\. Step Functions doesn't attempt to match a numeric field to a string value\. However, because timestamp fields are logically strings, it's possible that a field considered to be a timestamp can be matched by a `StringEquals` comparator\.

**Note**  
For interoperability, don't assume that numeric comparisons work with values outside the magnitude or precision that the [IEEE 754\-2008 `binary64` data type](https://en.wikipedia.org/wiki/IEEE_754#Basic_and_interchange_formats) represents\. In particular, integers outside of the range `[-253+1, 253-1]` might fail to compare in the expected way\.  
Timestamps \(for example, `2016-08-18T17:33:00Z`\) must conform to [RFC3339 profile ISO 8601](https://www.ietf.org/rfc/rfc3339.txt), with further restrictions:  
An uppercase `T` must separate the date and time portions\.
An uppercase `Z` must denote that a numeric time zone offset isn't present\.
To understand the behavior of string comparisons, see the [Java `compareTo` documentation](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#compareTo-java.lang.String-)\.  
The values of the `And` and `Or` operators must be non\-empty arrays of Choice Rules that must not themselves contain `Next` fields\. Likewise, the value of a `Not` operator must be a single Choice Rule that must not contain `Next` fields\.  
You can create complex, nested Choice Rules using `And`, `Not`, and `Or`\. However, the `Next` field can appear only in a top\-level Choice Rule\.