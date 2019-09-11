# Invoke Lambda with Step Functions<a name="connect-lambda"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information, see:
+ [Service Integrations](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

For more information about managing state input, output and results, see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.

Supported APIs:
+ [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html)
  + [Request Syntax](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters)
  + Supported Parameters
    + [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters)
    + [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters)
    + [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters)
    + [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters)
    + [https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestParameters)
  + [Response syntax](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_ResponseSyntax)

The following includes a `Task` state that invokes a Lambda function\.

```
{  
   "StartAt":"CallLambda",
   "States":{  
      "CallLambda":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::lambda:invoke",
         "Parameters":{  
            "FunctionName":"MyFunction"
         },
         "End":true
      }
   }
}
```

The following includes a `Task` state that implements the [callback](connect-to-resource.md#connect-wait-token) service integration pattern\.

```
{  
   "StartAt":"GetManualReview",
   "States":{  
      "GetManualReview":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::lambda:invoke.waitForTaskToken",
         "Parameters":{  
            "FunctionName":"get-model-review-decision",
            "Payload":{  
               "model.$":"$.new_model",
               "token.$":"$$.Task.Token"
            },
            "Qualifier":"prod-v1"
         },
         "End":true
      }
   }
}
```

To simply invoke a Lambda function, you can also call the resource ARN directly in the `"Resource"` string\.

**Note**  
When you invoke Lambda using an ARN directly from the `"Resource"` field, you cannot specify `.waitForTaskToken`, and the state input is passed to Lambda as the payload\. The output of the Lambda function is the result\.  
To have a Lambda task wait for a task token, see the previous callback pattern example\.

```
{  
   "StartAt":"CallFunction",
   "States":{  
      "CallFunction": {  
         "Type":"Task",
         "Resource":"arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
         "End": true
      }
   }
}
```

**Note**  
You can invoke a specific Lambda function version or alias by specifying those options in the Amazon Resource Name in the `Resource` field\. See the following in the Lambda documentation\.  
[AWS Lambda versioning](https://docs.aws.amazon.com/lambda/latest/dg/versioning-intro.html)
[AWS Lambda aliases](https://docs.aws.amazon.com/lambda/latest/dg/aliases-intro.html)

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](service-integration-iam-templates.md)\.