# Development Options<a name="development-options"></a>

You can implement your Step Functions state machines in a number of ways\.

## Step Functions Console<a name="development-options-console"></a>

You can define a state machine using the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\. You can write complex state machines in the cloud without using a local development enviroment by taking advantage of Lambda to supply code for your tasks and the Step Functions console to define your state machine using Amazon States Language\.

The [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial uses this technique to create a simple state machine, execute it, and view its results\.

## AWS SDKs<a name="development-options-aws-sdk"></a>

Step Functions is supported by SDKs for Java, \.NET, Ruby, PHP, Python \(boto 3\), JavaScript, Go, and C\+\+, providing a convenient way to use the Step Functions HTTPS API actions in various programming languages\.

You can develop state machines, activities, or state machine starters using the API actions exposed by these libraries\. You can also access visibility operations using these libraries to develop your own Step Functions monitoring and reporting tools\.

To use Step Functions with other AWS services, see the reference documentation for the current AWS SDKs and [Tools for Amazon Web Services](http://aws.amazon.com/tools/)\.

**Note**  
Step Functions supports only an HTTPS endpoint\.

## HTTPS Service API<a name="development-options-service-api"></a>

Step Functions provides service operations accessible through HTTPS requests\. You can use these operations to communicate directly with Step Functions and to develop your own libraries in any language that can communicate with Step Functions through HTTPS\.

You can develop state machines, workers, or state machine starters using the service API actions\. You can also access visibility operations through the API actions to develop your own monitoring and reporting tools\. For detailed information on API actions, see the *[AWS Step Functions API Reference](https://docs.aws.amazon.com/step-functions/latest/apireference/)*\.

## Development Environments<a name="development-options-environments"></a>

You must set up a development environment appropriate to the programming language that you plan to use\. For example, if you intend to develop for Step Functions with Java, you should install a Java development environment \(such as the SDK for Java\) on each of your development workstations\. If you use Eclipse IDE for Java Development, you should also install the Toolkit for Eclipse\. This Eclipse plug\-in adds features useful for AWS development\.

If your programming language requires a run\-time environment, you must set up the environment on each computer where these processes run\.

## Endpoints<a name="development-options-endpoints"></a>

To reduce latency and to store data in a location that meets your requirements, Step Functions provides endpoints in different regions\.

Each endpoint in Step Functions is completely independent: A state machine or activity exists only within the region where it was created\. Any state machines and activities that you create in one region don't share any data or attributes with those created in another region\. For example, you can register a state machine named `STATES-Flows-1` in two different regions, but the two state machines won't share data or attributes with each other, being completely independent from each other\.

For a list of Step Functions endpoints, see [Regions and Endpoints: AWS Step Functions](https://docs.aws.amazon.com/general/latest/gr/rande.html#step-functions_region) in the *Amazon Web Services General Reference*\.

## AWS CLI<a name="development-options-aws-cli"></a>

You can access many Step Functions features from the AWS CLI\. The AWS CLI provides an alternative to using the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) or, in some cases, to program using the AWS Step Functions API actions\. For example, you can use the AWS CLI to create a new state machine and then list your state machines\.

The Step Functions commands in AWS CLI allow you to start and manage executions, poll for activities, record task heartbeats, and so on\. For a complete list of Step Functions commands and the descriptions of the available arguments and examples showing their use, see the *AWS CLI Command Reference*\.

The AWS CLI commands follow the Amazon States Language closely, so you can use the AWS CLI to learn about the Step Functions API actions\. You can also use your existing API knowledge to prototype code or perform Step Functions actions from the command line\.