# Setting Configuration Options for Step Functions Local<a name="sfn-local-config-options"></a>

When you start AWS Step Functions Local  by using the JAR file, you can set configuration options by using the AWS Command Line Interface \(AWS CLI\), or by including them in the system environment\. For Docker, you must specify these options in a file that you reference when starting Step Functions Local\.

## Configuration Options<a name="sfn-local-config-options-table"></a>


| Option | Command Line | Environment | 
| --- | --- | --- | 
| Account | \-account, \-\-aws\-account | AWS\_ACCOUNT\_ID | 
| Region | \-region, \-\-aws\-region | AWS\_DEFAULT\_REGION | 
| Wait Time Scale | \-waitTimeScale, \-\-wait\-time\-scale | WAIT\_TIME\_SCALE | 
| Lambda Endpoint | \-lambdaEndpoint, \-\-lambda\-endpoint | LAMBDA\_ENDPOINT | 
| Batch Endpoint | \-batchEndpoint, \-\-batch\-endpoint | BATCH\_ENDPOINT | 
| DynamoDB Endpoint | \-dynamoDBEndpoint, \-\-dynamodb\-endpoint | DYNAMODB\_ENDPOINT | 
| ECS Endpoint  | \-ecsEndpoint, \-\-ecs\-endpoint | ECS\_ENDPOINT | 
| Glue Endpoint | \-glueEndpoint, \-\-glue\-endpoint | GLUE\_ENDPOINT | 
| SageMaker Endpoint | \-sageMakerEndpoint, \-\-sagemaker\-endpoint | SAGE\_MAKER\_ENDPOINT | 
| SQS Endpoint | \-sqsEndpoint, \-\-sqs\-endpoint | SQS\_ENDPOINT | 
| SNS Endpoint | \-snsEndpoint, \-\-sns\-endpoint | SNS\_ENDPOINT | 
| Step Functions Endpoint | \-stepFunctionsEndpoint, \-\-step\-functions\-endpoint | STEP\_FUNCTIONS\_ENDPOINT | 

## Credentials and configuration for Docker<a name="docker-credentials"></a>

To configure Step Functions Local for Docker, create the following file: `aws-stepfunctions-local-credentials.txt`\.

This file contains your credentials and other configuration options\. The following can be used as a template when creating the `aws-stepfunctions-local-credentials.txt` file\.

```
AWS_DEFAULT_REGION=AWS_REGION_OF_YOUR_AWS_RESOURCES
AWS_ACCESS_KEY_ID=YOUR_AWS_ACCESS_KEY
AWS_SECRET_ACCESS_KEY=YOUR_AWS_SECRET_KEY
WAIT_TIME_SCALE=VALUE
LAMBDA_ENDPOINT=VALUE
BATCH_ENDPOINT=VALUE
DYNAMODB_ENDPOINT=VALUE
ECS_ENDPOINT=VALUE
GLUE_ENDPOINT=VALUE
SAGE_MAKER_ENDPOINT=VALUE
SQS_ENDPOINT=VALUE
SNS_ENDPOINT=VALUE
STEP_FUNCTIONS_ENDPOINT=VALUE
```

Once you have configured your credentials and configuration options in `aws-stepfunctions-local-credentials.txt`, start Step Functions with the following command\.

```
docker run -p 8083:8083 --env-file aws-stepfunctions-local-credentials.txt amazon/aws-stepfunctions-local
```

**Note**  
 It is recommended to use the special DNS name `host.docker.internal`, which resolves to the internal IP address that the host uses, such as `http://host.docker.internal:8000`\. For more information, see Docker documentation for Mac and Windows at [Networking features in Docker Desktop for Mac](https://docs.docker.com/desktop/mac/networking/#use-cases-and-workaround) and [Networking features in Docker Desktop for Windows](https://docs.docker.com/desktop/windows/networking/) respectively\. 