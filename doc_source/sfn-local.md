# Setting Up Step Functions Local \(Downloadable Version\)<a name="sfn-local"></a>

The downloadable version of AWS Step Functions is provided as an executable \.jar file, and as a Docker image\. The Java application runs on Windows, Linux, macOS X, and other platforms that support Java\. In addition to Java, you need to install the AWS Command Line Interface\. For information on installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**Warning**  
The downloadable version of AWS Step Functions is only intended to be used for testing and shouldn't be used to process sensitive information\.

Follow these steps to set up and run Step Functions on your computer:

1. Download Step Functions using the following link:     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/sfn-local.html)

1. Extract the zip file\.

1. Test the download and view version information\.

   ```
   $ java -jar StepFunctionsLocal.jar -v
   Step Function Local
   Version: 1.0.0
   Build: 2019-01-21
   ```

1. \(Optional\) View a listing of available commands:

   ```
   $ java -jar StepFunctionsLocal.jar -h
   ```

1. To start Step Functions on your computer, open a command prompt window, navigate to the directory where you extracted `StepFunctionsLocal.jar` and type the following command:

   ```
   java -jar StepFunctionsLocal.jar
   ```

1. To access Step Functions running locally, use the `--endpoint-url` parameter\. For example, using the AWS Command Line Interface, you would specify Step Functions commands as:

   ```
   aws stepfunctions --endpoint-url http://localhost:8083 command
   ```

**Note**  
By default Step Functions Local uses a fake account and credentials, and the region is set to US East \(N\. Virginia\)\. To use Step Functions Local with AWS Lambda, or other supported services, you must configure your credentials and region\.

To configure and run Step Functions Local to work with AWS Lambda, AWS SAM CLI Local, or other supported services, see the following topics\. 

**Topics**
+ [Step Functions \(Downloadable Version\) on Your Computer](sfn-local-computer.md)
+ [Step Functions \(Downloadable Version\) and Docker](sfn-local-docker.md)
+ [Step Functions Local Configuration Options](sfn-local-config-options.md)
+ [Step Functions and AWS SAM CLI Local](sfn-local-lambda.md)