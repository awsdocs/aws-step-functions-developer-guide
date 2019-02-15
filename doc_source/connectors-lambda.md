# Invoke Lambda with Step Functions<a name="connectors-lambda"></a>

It is simple and convenient to use a Lambda function for implementing task states\. Include the ARN of your Lambda function in the `Resource` field of a task state\. Step Functions will wait for the Lambda function to complete\. The output of the Lambda function will be the result of the task\.

**Note**  
For more information on managing state input, output and results, see [Input and Output Processing in Step Functions](concepts-input-output-filtering.md)\.

The following includes a `Task` state that invokes a Lambda function\.

```
  "States": {
    "HelloWorld": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
      "End": true
    }
  }
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for Integrated Services](connectors-iam-templates.md)\.