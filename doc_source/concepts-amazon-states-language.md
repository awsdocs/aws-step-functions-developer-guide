# Amazon States Language<a name="concepts-amazon-states-language"></a>

The Amazon States Language is a JSON\-based, structured language used to define your state machine, a collection of [states](concepts-states.md), that can do work \(`Task` states\), determine which states to transition to next \(`Choice` states\), stop an execution with an error \(`Fail` states\), and so on\. 

For more information, see the [Amazon States Language Specification](https://states-language.net/spec.html) and [Statelint](https://github.com/awslabs/statelint), a tool that validates Amazon States Language code\.

To create a state machine on the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) using Amazon States Language, see [Getting Started](getting-started.md)\.

## Example Amazon States Language Specification<a name="example-amazon-states-language-specification"></a>

```
{
  "Comment": "An example of the Amazon States Language using a choice state.",
  "StartAt": "FirstState",
  "States": {
    "FirstState": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:FUNCTION_NAME",
      "Next": "ChoiceState"
    },
    "ChoiceState": {
      "Type" : "Choice",
      "Choices": [
        {
          "Variable": "$.foo",
          "NumericEquals": 1,
          "Next": "FirstMatchState"
        },
        {
          "Variable": "$.foo",
          "NumericEquals": 2,
          "Next": "SecondMatchState"
        }
      ],
      "Default": "DefaultState"
    },

    "FirstMatchState": {
      "Type" : "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:OnFirstMatch",
      "Next": "NextState"
    },

    "SecondMatchState": {
      "Type" : "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:OnSecondMatch",
      "Next": "NextState"
    },

    "DefaultState": {
      "Type": "Fail",
      "Error": "DefaultStateError",
      "Cause": "No Matches!"
    },

    "NextState": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:FUNCTION_NAME",
      "End": true
    }
  }
}
```

**Topics**
+ [Example Amazon States Language Specification](#example-amazon-states-language-specification)
+ [State Machine Structure](amazon-states-language-state-machine-structure.md)
+ [Common State Fields](amazon-states-language-common-fields.md)