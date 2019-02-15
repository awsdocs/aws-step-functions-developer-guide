# Step Functions \(Downloadable Version\) and Docker<a name="sfn-local-docker"></a>

The Step Functions Local Docker image enables you to get started with Step Functions Local quickly by using a docker image with all the needed dependencies\. The Docker image enables you to include Step Functions local in your containerized builds, and as part of your continuous integration testing\.

To get the Docker image for Step Functions Local, visit [https://hub\.docker\.com/r/amazon/aws\-stepfunctions\-local](https://hub.docker.com/r/amazon/aws-stepfunctions-local), or type the Docker pull command:

```
docker pull amazon/aws-stepfunctions-local
```

To start the downloadable version of Step Functions on Docker, run:

```
docker run -p 8083:8083 amazon/aws-stepfunctions-local
```

In order to interact with AWS Lambda or other supported services you need to configure your credentials and other configuration options first\. See:
+ [Step Functions Local Configuration Options](sfn-local-config-options.md)
+ [Credentials and Configuration for Docker](sfn-local-config-options.md#docker-credentials)