# Invoke Lambda with Step Functions<a name="connect-lambda"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

**How the Optimized Lambda integration is different than the Lambda AWS SDK integration**  
The `Payload` field of the response is parsed from escaped Json to Json\.
If the response contains the field `FunctionError`, the task fails\.

For more information about managing state input, output, and results, see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.

Supported AWS Lambda APIs:
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

When you invoke a Lambda function, the execution will wait for the function to complete\. However, it is possible to call Lambda asynchronously using the `InvocationType` parameter, as seen in the following example:

```
{

  "Comment": "A Hello World example of the Amazon States Language using Pass states",
  "StartAt": "Hello",
  "States": {
    "Hello": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "echo",
        "InvocationType": "Event"
      },
      "End": true
    }
  }
}
```

 When the `Task` result is returned, the function output is nested inside a dictionary of metadata\. For example:

```
{

   "ExecutedVersion":"$LATEST",
   "Payload":"FUNCTION OUTPUT",
   "SdkHttpMetadata":{
      "HttpHeaders":{
         "Connection":"keep-alive",
         "Content-Length":"4",
         "Content-Type":"application/json",
         "Date":"Fri, 26 Mar 2021 07:42:02 GMT",
         "X-Amz-Executed-Version":"$LATEST",
         "x-amzn-Remapped-Content-Length":"0",
         "x-amzn-RequestId":"0101aa0101-1111-111a-aa55-1010aaa1010",
         "X-Amzn-Trace-Id":"root=1-1a1a000a2a2-fe0101aa10ab;sampled=0"
      },
      "HttpStatusCode":200
   },
   "SdkResponseMetadata":{
      "RequestId":"6b3bebdb-9251-453a-ae45-512d9e2bf4d3"
   },
   "StatusCode":200
}
```

Alternatively, you can invoke a Lambda function by specifying a function ARN directly in the "Resource" field\. When you invoke a Lambda function in this way, you can't specify `.waitForTaskToken`, and the task result contains only the function output\.

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

You can invoke a specific Lambda function version or alias by specifying those options in the ARN in the `Resource` field\. See the following in the Lambda documentation:
+ [AWS Lambda versioning](https://docs.aws.amazon.com/lambda/latest/dg/versioning-intro.html)
+ [AWS Lambda aliases](https://docs.aws.amazon.com/lambda/latest/dg/aliases-intro.html)

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.