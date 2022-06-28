# Use Amazon S3 ARNs instead of passing large payloads<a name="avoid-exec-failures"></a>

Executions that pass large payloads of data between states can be terminated\. If the data you are passing between states might grow to over 262,144 bytes, use Amazon Simple Storage Service \(Amazon S3\) to store the data, and parse the Amazon Resource Name \(ARN\) of the bucket in the `Payload` parameter to get the bucket name and key value\. Alternatively, adjust your implementation so that you pass smaller payloads in your executions\.

In the following example, a state machine passes input to an AWS Lambda function, which processes a JSON file in an Amazon S3 bucket\. After you run this state machine, the Lambda function reads the contents of the JSON file, and returns the file contents as output\.

**Create the Lambda function**  
The following Lambda function named `pass-large-payload` reads the contents of a JSON file stored in a specific Amazon S3 bucket\.

**Note**  
After you create this Lambda function, make sure you provide its IAM role the appropriate permission to read from an Amazon S3 bucket\. For example, attach the **AmazonS3ReadOnlyAccess** permission to the Lambda function's role\.

```
import json
import boto3
import io
import os

s3 = boto3.client('s3')

def lambda_handler(event, context):
    event = event['Input']
    final_json = str()
    
    s3 = boto3.resource('s3')
    bucket = event['bucket'].split(':')[-1]
    filename = event['key']
    directory = "/tmp/{}".format(filename)
    
    s3.Bucket(bucket).download_file(filename, directory)
    
    with open(directory, "r") as jsonfile:
    
        final_json = json.load(jsonfile)
    
    os.popen("rm -rf /tmp")
    
    return final_json
```

**Create the state machine**  
The following state machine invokes the Lambda function you previously created\.

```
{  
   "StartAt":"Invoke Lambda function",
   "States":{  
      "Invoke Lambda function":{  
         "Type":"Task",
         "Resource":"arn:aws:states:::lambda:invoke",
         "Parameters":{  
            "FunctionName":"arn:aws:lambda:us-east-2:123456789012:function:pass-large-payload",
            "Payload":{  
               "Input.$":"$"
            }
         },
         "OutputPath": "$.Payload",
         "End":true
      }
   }
}
```

Rather than pass a large amount of data in the input, you could save that data in an Amazon S3 bucket, and pass the Amazon Resource Name \(ARN\) of the bucket in the `Payload` parameter to get the bucket name and key value\. Your Lambda function can then use that ARN to access the data directly\. The following is example input for the state machine execution, where the data is stored in `data.json` in an Amazon S3 bucket named `large-payload-json`\.

```
{
  "key": "data.json",
  "bucket": "arn:aws:s3:::large-payload-json"
}
```