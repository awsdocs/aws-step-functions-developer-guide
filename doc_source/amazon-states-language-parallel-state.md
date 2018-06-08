# Parallel<a name="amazon-states-language-parallel-state"></a>

The `Parallel` state \(`"Type": "Parallel"`\) can be used to create parallel branches of execution in your state machine\.

In addition to the [common state fields](amazon-states-language-common-fields.md), `Parallel` states introduce these additional fields:

** `Branches` \(Required\)**  
An array of objects that specify state machines to execute in parallel\. Each such state machine object must have fields named `States` and `StartAt` whose meanings are exactly like those in the top level of a state machine\.

** `ResultPath` \(Optional\)**  
Specifies where \(in the input\) to place the output of the branches\. The input is then filtered as prescribed by the `OutputPath` field \(if present\) before being used as the state's output\. For more information, see [Input and Output Processing](amazon-states-language-input-output-processing.md)\.

** `Retry` \(Optional\)**  
An array of objects, called Retriers that define a retry policy in case the state encounters runtime errors\. For more information, see [Retrying After an Error](amazon-states-language-errors.md#amazon-states-language-retrying-after-error)\.

** `Catch` \(Optional\)**  
An array of objects, called Catchers that define a fallback state which is executed in case the state encounters runtime errors and its retry policy has been exhausted or is not defined\. For more information, see [Fallback States](amazon-states-language-errors.md#amazon-states-language-fallback-states)\.

A `Parallel` state causes AWS Step Functions to execute each branch, starting with the state named in that branch's `StartAt` field, as concurrently as possible, and wait until all branches terminate \(reach a terminal state\) before processing the `Parallel` state's `Next` field\.

Here is an example:

```
{
  "Comment": "Parallel Example.",
  "StartAt": "LookupCustomerInfo",
  "States": {
    "LookupCustomerInfo": {
      "Type": "Parallel",
      "End": true,
      "Branches": [
        {
         "StartAt": "LookupAddress",
         "States": {
           "LookupAddress": {
             "Type": "Task",
             "Resource":
               "arn:aws:lambda:us-east-1:123456789012:function:AddressFinder",
             "End": true
           }
         }
       },
       {
         "StartAt": "LookupPhone",
         "States": {
           "LookupPhone": {
             "Type": "Task",
             "Resource":
               "arn:aws:lambda:us-east-1:123456789012:function:PhoneFinder",
             "End": true
           }
         }
       }
      ]
    }
  }
}
```

In this example, the `LookupAddress` and `LookupPhone` branches are executed in parallel\. Here is how the visual workflow looks in the Step Functions console:

![\[Parallel Workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/parallel-state.png)

Each branch must be self\-contained\. A state in one branch of a `Parallel` state must not have a `Next` field that targets a field outside of that branch, nor can any other state outside the branch transition into that branch\.

## Parallel State Output<a name="amazon-states-language-parallel-state-output"></a>

A `Parallel` state provides each branch with a copy of its own input data \(subject to modification by the `InputPath` field\)\. It generates output which is an array with one element for each branch containing the output from that branch\. There is no requirement that all elements be of the same type\. The output array can be inserted into the input data \(and the whole sent as the `Parallel` state's output\) by using a `ResultPath` field in the usual way \(see [Input and Output Processing](amazon-states-language-input-output-processing.md)\)\.

Here is another example:

```
{
  "Comment": "Parallel Example.",
  "StartAt": "FunWithMath",
  "States": {
    "FunWithMath": {
    "Type": "Parallel",
    "End": true,
    "Branches": [
      {
        "StartAt": "Add",
        "States": {
          "Add": {
            "Type": "Task",
            "Resource": "arn:aws:swf:us-east-1:123456789012:task:Add",
            "End": true
          }
        }
      },
      {
        "StartAt": "Subtract",
        "States": {
          "Subtract": {
            "Type": "Task",
            "Resource": "arn:aws:swf:us-east-1:123456789012:task:Subtract",
            "End": true
          }
        }
      }
    ]
   }
  }
}
```

If the `FunWithMath` state was given the array `[3, 2]` as input, then both the `Add` and `Subtract` states receive that array as input\. The output of `Add` would be `5`, that of `Subtract` would be `1`, and the output of the `Parallel` state would be an array:

```
[ 5, 1 ]
```

### Error Handling<a name="error-handling"></a>

If any branch fails, due to either an unhandled error or by transitioning to a `Fail` state, the entire `Parallel` state is considered to have failed and all its branches are stopped\. If the error is not handled by the `Parallel` state itself, Step Functions will stop the execution with an error\.