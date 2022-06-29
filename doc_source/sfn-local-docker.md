# Setting Up Step Functions Local \(Downloadable Version\) and Docker<a name="sfn-local-docker"></a>

The Step Functions Local Docker image enables you to get started with Step Functions Local quickly by using a Docker image with all the needed dependencies\. The Docker image enables you to include Step Functions Local in your containerized builds and as part of your continuous integration testing\.

To get the Docker image for Step Functions Local, see [https://hub\.docker\.com/r/amazon/aws\-stepfunctions\-local](https://hub.docker.com/r/amazon/aws-stepfunctions-local), or enter the following Docker `pull` command\.

```
docker pull amazon/aws-stepfunctions-local
```

To start the downloadable version of Step Functions on Docker, run the following Docker `run` command

```
docker run -p 8083:8083 amazon/aws-stepfunctions-local
```

To interact with AWS Lambda or other supported services, you need to configure your credentials and other configuration options first\. For more information, see the following topics:
+ [Setting Configuration Options for Step Functions Local](sfn-local-config-options.md)
+ [Credentials and configuration for Docker](sfn-local-config-options.md#docker-credentials)