# Use Amazon S3 ARNs instead of passing large payloads<a name="avoid-exec-failures"></a>

Executions that pass large payloads of data between states can be terminated\. If the data you are passing between states might grow to over 262,144 bytes, use Amazon Simple Storage Service \(Amazon S3\) to store the data, and pass the Amazon Resource Name \(ARN\) instead of the raw data\. Alternatively, adjust your implementation so that you pass smaller payloads in your executions\.

For example, a state machine can pass input to an AWS Lambda function, like in the following example\.

```
{  
   "StartAt":"Invoke Lambda function",
   "States":{  
      "Invoke Lambda function":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::lambda:invoke",
         "Parameters":{  
            "FunctionName":"arn:aws:lambda:us-east-2:123456789012:function:MyFunctionName",
            "Payload":{  
               "Input.$":"$"
            }
         },
         "End":true
      }
   }
}
```

Rather than pass a large amount of data in the input, you could save that data in an Amazon S3 bucket, and pass the ARN of that bucket in the `Payload` parameter\. Your Lambda function can then use that ARN to access the data directly\. The following is example input for a state machine execution, where the data is stored in `data.json` in an Amazon S3 bucket\.

```
{
  "Data": "arn:aws:s3:::MyBucket/data.json"
}
```

For more information, see the following:
+ [Quotas for Standard Workflows](limits.md)
+ [Quotas for Express Workflows](express-limits.md)
+ [Amazon Simple Storage Service Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/)
+ [Amazon Resource Names \(ARNs\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)