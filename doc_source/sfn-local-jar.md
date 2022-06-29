# Setting Up Step Functions Local \(Downloadable Version\) \- Java Version<a name="sfn-local-jar"></a>

The downloadable version of AWS Step Functions is provided as an executable JAR file and as a Docker image\. The Java application runs on Windows, Linux, macOS, and other platforms that support Java\. In addition to Java, you need to install the AWS Command Line Interface \(AWS CLI\)\. For information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To set up and run Step Functions on your computer**

1. Download Step Functions using the following links\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/sfn-local-jar.html)

1. Extract the `.zip` file\.

1. Test the download and view version information\.

   ```
   $ java -jar StepFunctionsLocal.jar -v
   Step Function Local
   Version: 1.0.0
   Build: 2019-01-21
   ```

1. \(Optional\) View a listing of available commands\.

   ```
   $ java -jar StepFunctionsLocal.jar -h
   ```

1. To start Step Functions on your computer, open a command prompt, navigate to the directory where you extracted `StepFunctionsLocal.jar`, and enter the following command\.

   ```
   java -jar StepFunctionsLocal.jar
   ```

1. To access Step Functions running locally, use the `--endpoint-url` parameter\. For example, using the AWS CLI, you would specify Step Functions commands as follows:

   ```
   aws stepfunctions --endpoint-url http://localhost:8083 command
   ```

**Note**  
By default, Step Functions Local uses a local test account and credentials, and the AWS Region is set to US East \(N\. Virginia\)\. To use Step Functions Local with AWS Lambda, or other supported services, you must configure your credentials and Region\.  
If you use Express workflows with Step Functions Local, the execution history will be stored in a log file\. It is not logged to CloudWatch Logs\. The log file path will be based on the CloudWatch Logs log group ARN provided when you create the local state machine\. The log file will be stored in `/aws/states/log-group-name/${execution_arn}.log` relative to the location where you are running Step Functions Local\. For example, if the execution ARN is:  

```
arn:aws:states:us-east-1:123456789012:express:test:example-ExpressLogGroup-wJalrXUtnFEMI
```
the log file will be:  

```
aws/states/log-group-name/arn:aws:states:us-east-1:123456789012:express:test:example-ExpressLogGroup-wJalrXUtnFEMI.log
```