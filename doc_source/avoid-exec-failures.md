# Use ARNs Instead of Passing Large Payloads<a name="avoid-exec-failures"></a>

Executions that pass large payloads of data between states can be terminated\. If the data you are passing between states might grow to over 32 KB, use Amazon Simple Storage Service \(Amazon S3\) to store the data, and pass the Amazon Resource Name \(ARN\) instead of the raw data\. Alternatively, adjust your implementation so that you pass smaller payloads in your executions\.

For instance, a state machine can pass input to a Lambda function, like in the following example\.

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

Rather than pass a large amount of data in the input, you could save that data in an Amazon S3 bucket, and pass the Amazon Resource Name of that bucket in the `Payload` parameter\. Your Lambda function can then use that ARN to access the data directly\. The following is example input for an state machine execution, where the data is stored in `data.json` in an Amazon S3 bucket\.

```
{
  "Data": "arn:aws:s3:::MyBucket/data.json"
}
```

For more information, see:
+ [Limits](limits.md)
+ [Amazon Simple Storage Service Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/)
+ [Amazon Resource Names \(ARNs\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)