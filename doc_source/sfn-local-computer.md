# Step Functions \(Downloadable Version\) on Your Computer<a name="sfn-local-computer"></a>

## Run a HelloWorld State Machine Locally<a name="sfn-local-heloworld"></a>

Once you have run Step Functions locally with the AWS CLI, you can start a state machine execution\.

1. Create a state machine from the AWS CLI by escaping the state machine definition\.

   ```
   aws stepfunctions --endpoint-url http://localhost:8083 create-state-machine --definition "{\
     \"Comment\": \"A Hello World example of the Amazon States Language using a Pass state\",\
     \"StartAt\": \"HelloWorld\",\
     \"States\": {\
       \"HelloWorld\": {\
         \"Type\": \"Pass\",\
         \"End\": true\
       }\
     }}" --name "HelloWorld" --role-arn "arn:aws:iam::012345678901:role/DummyRole"
   ```
**Note**  
The `role-arn` is not used for Step Functions Local, but you must have it included with the proper syntax\. You can use the ARN from the above example\. 

   If you successfully create the state machine, Step Functions will respond with the creation date and the state machine ARN:

   ```
   {
       "creationDate": 1548454198.202, 
       "stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld"
   }
   ```

1. Start an execution using the ARN of the state machine you created\.

   ```
   aws stepfunctions --endpoint-url http://localhost:8083 start-execution --state-machine-arn arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld
   ```

## Step Functions Local with AWS SAM CLI Local<a name="with-lambda-local"></a>

You can use the local version of Step Functions along with a local version of AWS Lambda\. To configure this, you must install and configure the AWS Serverless Application Model \(AWS SAM\)\.

For information on configuring and running AWS SAM, see:
+ [Set Up AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html)
+ [Start AWS SAM CLI Local](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-local-start-lambda.html)

Once Lambda is running on your local system, you can start Step Functions Local\. From the directory where you extracted your Step Functions local jar files, start Step Functions Local, configuring the local Lambda endpoint:

```
java -jar StepFunctionsLocal.jar --lambda-endpoint http://127.0.0.1:3001 command
```

For more information is running Step Functions Local with AWS Lambda, see [Step Functions and AWS SAM CLI Local](sfn-local-lambda.md)