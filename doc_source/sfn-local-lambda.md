# Step Functions and AWS SAM CLI Local<a name="sfn-local-lambda"></a>

With both Step Functions and Lambda running on your local machine, you can test your state machine and Lambda functions without deploying your code to AWS\. 

For more information, see:
+ [Setting Up Step Functions Local \(Downloadable Version\)](sfn-local.md)
+ [Set Up AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html)

**Topics**
+ [Step 1: Set Up The AWS Serverless Application Model](#install-sam)
+ [Step 2: Test AWS SAM CLI Local](#test-local-lambda)
+ [Step 3: Start AWS SAM CLI Local](#start-local-lambda)
+ [Step 4: Start Step Functions Local](#start-stepfunctions-local)
+ [Step 5: Create a State Machine That References Your AWS SAM CLI Local Function](#create-local-statemachine)
+ [Step 6: Start an Execution of Your Local State Machine](#run-local-statemachine)

## Step 1: Set Up The AWS Serverless Application Model<a name="install-sam"></a>

AWS SAM CLI Local requires the AWS Command Line Interface, the AWS Serverless Application Model, and Docker to be installed\. 

1. Install the AWS SAM CLI\.

   For more information see: [Installing the AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)\.
**Note**  
Before installing the AWS SAM CLI, you will need to install the AWS CLI and Docker\. See the [Prerequisites](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) for installing the AWS SAM CLI\.

1. Go through the [AWS SAM Quick Start](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html) documentation\. Be sure to follow the steps to:

   1. [Initialize the Application](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html#gs-ex1-setup-local-app)

   1.  [Test the Application Locally](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html#gs-ex1-test-locally) 

   This will create a `sam-app` directory, and will build an environment that includes a Python\-based hello world Lambda function\.

## Step 2: Test AWS SAM CLI Local<a name="test-local-lambda"></a>

Now that you have installed AWS SAM and created the Hello World Lambda function, test that it works\. In the `sam-app` directory, type:

```
sam local start-api
```

This launches a local instance of your Lambda function\.

```
2019-01-31 16:40:27 Found credentials in shared credentials file: ~/.aws/credentials
2019-01-31 16:40:27 Mounting HelloWorldFunction at http://127.0.0.1:3000/hello [GET]
2019-01-31 16:40:27 You can now browse to the above endpoints to invoke your functions. You do not need to restart/reload SAM CLI while working on your functions changes will be reflected instantly/automatically. You only need to restart SAM CLI if you update your AWS SAM template
2019-01-31 16:40:27  * Running on http://127.0.0.1:3000/ (Press CTRL+C to quit)
```

Open a browser and enter:

```
http://127.0.0.1:3000/hello
```

This will show output from your function:

```
{"message": "hello world", "location": "72.21.198.66"}
```

Enter **CTRL\+C** to end the Lambda API\.

## Step 3: Start AWS SAM CLI Local<a name="start-local-lambda"></a>

Now that you've tested that the function works, start AWS SAM CLI Local\. In the `sam-app` directory, type:

```
sam local start-lambda
```

This will start AWS SAM CLI Local and will provide the endpoint to use:

```
2019-01-29 15:33:32 Found credentials in shared credentials file: ~/.aws/credentials
2019-01-29 15:33:32 Starting the Local Lambda Service. You can now invoke your Lambda Functions defined in your template through the endpoint.
2019-01-29 15:33:32  * Running on http://127.0.0.1:3001/ (Press CTRL+C to quit)
```

## Step 4: Start Step Functions Local<a name="start-stepfunctions-local"></a>

### Jar File<a name="start-local-jar"></a>

If using the `.jar` file version of Step Functions Local, start step functions specifying the Lambda endpoint\. In the directory where you extracted the `.jar` files, type:

```
java -jar StepFunctionsLocal.jar --lambda-endpoint http://localhost:3001
```

When Step Functions Local starts, it will check the environment, and then the credentials configured in your `~/.aws/credentials` file\. By default, it will start using a fake user ID, and will be listed as `region us-east-1`:

```
2019-01-29 15:38:06.324: Failed to load credentials from environment because Unable to load AWS credentials from environment variables (AWS_ACCESS_KEY_ID (or AWS_ACCESS_KEY) and AWS_SECRET_KEY (or AWS_SECRET_ACCESS_KEY))
2019-01-29 15:38:06.326: Loaded credentials from profile: default
2019-01-29 15:38:06.326: Starting server on port 8083 with account 123456789012, region us-east-1
```

### Docker<a name="start-local-docker"></a>

If using the Docker version of Step Functions Local, launch Step Functions with the following command\.

```
docker run -p 8083:8083 amazon/aws-stepfunctions-local
```

For information on installing the Docker version of Step Functions, see [Step Functions \(Downloadable Version\) and Docker](sfn-local-docker.md)\.

**Note**  
You can specify the endpoint through the command line or by setting environment variables if you launch Step Functions from the `.jar` file\. For the Docker version, you must specify the endpoints and credentials in a text file\. See [Step Functions Local Configuration Options](sfn-local-config-options.md)\.

## Step 5: Create a State Machine That References Your AWS SAM CLI Local Function<a name="create-local-statemachine"></a>

Once Step Functions Local is running, create a state machine that references the `HelloWorldFunction` that you initialized in [Step 1: Set Up The AWS Serverless Application Model](#install-sam)\.

```
aws stepfunctions --endpoint http://localhost:8083 create-state-machine --definition "{\
  \"Comment\": \"A Hello World example of the Amazon States Language using an AWS Lambda Local function\",\
  \"StartAt\": \"HelloWorld\",\
  \"States\": {\
    \"HelloWorld\": {\
      \"Type\": \"Task\",\
      \"Resource\": \"arn:aws:lambda:us-east-1:123456789012:function:HelloWorldFunction\",\
      \"End\": true\
    }\
  }\
}\
}}" --name "HelloWorld" --role-arn "arn:aws:iam::012345678901:role/DummyRole"
```

This will create a state machine and provide an ARN that you can use to start an execution:

```
{
    "creationDate": 1548805711.403, 
    "stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld"
}
```

## Step 6: Start an Execution of Your Local State Machine<a name="run-local-statemachine"></a>

Once you have created a state machine, start an execution referencing the endpoint and state machine ARN\.

```
aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld --name test
```

This will start an execution of your `HelloWorld` state machine and give it the name `test`\.

```
{
    "startDate": 1548810641.52, 
    "executionArn": "arn:aws:states:us-east-1:123456789012:execution:HelloWorld:test"
}
```

Now that Step Functions is running locally, you can interact with it using the AWS CLI\. For instance, to get information about this execution:

```
aws stepfunctions --endpoint http://localhost:8083 describe-execution --execution-arn arn:aws:states:us-east-1:123456789012:execution:HelloWorld:test
```

Calling `describe-execution` for an execution provides more complete details\. For example:

```
{
    "status": "SUCCEEDED", 
    "startDate": 1549056334.073, 
    "name": "test", 
    "executionArn": "arn:aws:states:us-east-1:123456789012:execution:HelloWorld:test", 
    "stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld", 
    "stopDate": 1549056351.276, 
    "output": "{\"statusCode\": 200, \"body\": \"{\\\"message\\\": \\\"hello world\\\", \\\"location\\\": \\\"72.21.198.64\\\"}\"}", 
    "input": "{}"
}
```