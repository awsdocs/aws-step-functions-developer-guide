# InputPath, ResultPath and OutputPath Examples<a name="input-output-example"></a>

Any state other than a `Fail` state can include `InputPath`, `ResultPath` or `OutputPath`\. These allow you to use a [JsonPath](https://github.com/json-path/JsonPath) to filter the JSON as it moves through your workflow\. 

You can also use the `Parameters` field to manipulate JSON as it moves through your workflow\. For information about using `Parameters`, see [InputPath, Parameters and ResultSelector](input-output-inputpath-params.md)\.

For example, start with the AWS Lambda function and state machine described in the [Creating a Step Functions State Machine That Uses Lambda](tutorial-creating-lambda-state-machine.md) tutorial\. Modify the state machine so that it includes the following `InputPath`, `ResultPath`, and `OutputPath`\.

```
{
  "Comment": "A Hello World example of the Amazon States Language using an AWS Lambda function",
  "StartAt": "HelloWorld",
  "States": {
    "HelloWorld": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
      "InputPath": "$.lambda",
      "ResultPath": "$.data.lambdaresult",
      "OutputPath": "$.data",
      "End": true
    }
  }
}
```

Start an execution using the following input\.

```
{
  "comment": "An input comment.",
  "data": {
    "val1": 23,
    "val2": 17
  },
  "extra": "foo",
  "lambda": {
    "who": "AWS Step Functions"
  }
}
```

Assume that the `comment` and `extra` nodes can be discarded, but that we want to include the output of the Lambda function, and preserve the information in the `data` node\.

In the updated state machine, the `Task` state is altered to process the input to the task\.

```
"InputPath": "$.lambda",
```

This line in the state machine definition limits the task input to only the `lambda` node from the state input\. The Lambda function receives only the JSON object `{"who": "AWS Step Functions"}` as input\. 

```
"ResultPath": "$.data.lambdaresult",
```

This `ResultPath` tells the state machine to insert the result of the Lambda function into a node named `lambdaresult`, as a child of the `data` node in the original state machine input\. Because we aren't performing any other manipulation on the original input and the result using `OutputPath`, the output of the state now includes the result of the Lambda function with the original input\.

```
{
  "comment": "An input comment.",
  "data": {
    "val1": 23,
    "val2": 17,
    "lambdaresult": "Hello, AWS Step Functions!"
  },
  "extra": "foo",
  "lambda": {
    "who": "AWS Step Functions"
  }
}
```

But, our goal was to preserve only the `data` node, and include the result of the Lambda function\. `OutputPath` filters this combined JSON before passing it to the state output\.

```
"OutputPath": "$.data",
```

This selects only the `data` node from the original input \(including the `lambdaresult` child inserted by `ResultPath`\) to be passed to the output\. The state output is filtered to the following\.

```
{
  "val1": 23,
  "val2": 17,
  "lambdaresult": "Hello, AWS Step Functions!"
}
```

In this `Task` state:

1. `InputPath` sends only the `lambda` node from the input to the Lambda function\.

1. `ResultPath` inserts the result as a child of the `data` node in the original input\.

1. `OutputPath` filters the state input \(which now includes the result of the Lambda function\) so that it passes only the `data` node to the state output\.

**Example to manipulate original state machine input, result, and final output using JsonPath**  
Consider the following state machine that verifies an insurance applicant's identity and address\.  
To view the complete example, see [How to use JSON Path in Step Functions](https://github.com/aws-samples/serverless-account-signup-service)\.

```
{
  "Comment": "Sample state machine to verify an applicant's ID and address",
  "StartAt": "Verify info",
  "States": {
    "Verify info": {
      "Type": "Parallel",
      "End": true,
      "Branches": [
        {
          "StartAt": "Verify identity",
          "States": {
            "Verify identity": {
              "Type": "Task",
              "Resource": "arn:aws:states:::lambda:invoke",
              "Parameters": {
                "Payload.$": "$",
                "FunctionName": "arn:aws:lambda:us-east-2:111122223333:function:check-identity:$LATEST"
              },
              "End": true
            }
          }
        },
        {
          "StartAt": "Verify address",
          "States": {
            "Verify address": {
              "Type": "Task",
              "Resource": "arn:aws:states:::lambda:invoke",
              "Parameters": {
                "Payload.$": "$",
                "FunctionName": "arn:aws:lambda:us-east-2:111122223333:function:check-address:$LATEST"
              },
              "End": true
            }
          }
        }
      ]
    }
  }
}
```
If you run this state machine using the following input, the execution fails because the Lambda functions that perform verification only expect the data that needs to be verified as input\. Therefore, you must specify the nodes that contain the information to be verified using an appropriate JsonPath\.  

```
{
  "data": {
    "firstname": "Jane",
    "lastname": "Doe",
    "identity": {
      "email": "jdoe@example.com",
      "ssn": "123-45-6789"
    },
    "address": {
      "street": "123 Main St",
      "city": "Columbus",
      "state": "OH",
      "zip": "43219"
    },
    "interests": [
      {
        "category": "home",
        "type": "own",
        "yearBuilt": 2004
      },
      {
        "category": "boat",
        "type": "snowmobile",
        "yearBuilt": 2020
      },
      {
        "category": "auto",
        "type": "RV",
        "yearBuilt": 2015
      },
    ]
  }
}
```
To specify the node that the `check-identity` Lambda function must use, use the `InputPath` field as follows:  

```
"InputPath": "$.data.address"
```
And to specify the node that the `check-address` Lambda function must use, use the `InputPath` field as follows:  

```
"InputPath": "$.data.address"
```
Now if you want to store the verification result within the original state machine input, use the `ResultPath` field as follows:  

```
"ResultPath": "$.results"
```
However, if you only need the identity and verification results and discard the original input, use the `OutputPath` field as follows:  

```
"OutputPath": "$.results"
```

For more information, see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.