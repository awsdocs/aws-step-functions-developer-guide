# Manage AWS Step Functions Executions as an Integrated Service<a name="connect-stepfunctions"></a>

Step Functions integrates with its own API as a service integration\. This allows Step Functions to start a new execution of a state machine directly from the task state of a running execution\. When building new workflows, use [nested workflow executions](concepts-nested-workflows.md) to reduce the complexity of your main workflows and to reuse common processes\.

**How the Optimized Step Functions integration is different than the Step Functions AWS SDK integration**  
The [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern is available\.
Note that there are no optimizations for the [Request Response](connect-to-resource.md#connect-default) or [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration patterns\.

For more information, see the following:
+ [Start Executions from a Task](concepts-nested-workflows.md)
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

Supported Step Functions APIs and syntax:
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html)
  + [Request Syntax](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax)
  + Supported Parameters
    + [https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax)
    + [https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax)
    + [https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax)
  + [Response syntax](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_ResponseSyntax)

The following includes a `Task` state that starts an execution of another state machine and waits for it to complete\.

```
{  
   "Type":"Task",
   "Resource":"arn:aws:states:::states:startExecution.sync:2",
   "Parameters":{  
      "Input":{
        "Comment": "Hello world!"
       },
      "StateMachineArn":"arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
      "Name":"ExecutionName"
   },
   "End":true
}
```

The following includes a `Task` state that starts an execution of another state machine\.

```
{  
   "Type":"Task",
   "Resource":"arn:aws:states:::states:startExecution",
   "Parameters":{  
      "Input":{
        "Comment": "Hello world!"
       },
      "StateMachineArn":"arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
      "Name":"ExecutionName"
   },
   "End":true
}
```

The following includes a `Task` state that implements the [callback](connect-to-resource.md#connect-wait-token) service integration pattern\.

```
{ 
   "Type":"Task",
   "Resource":"arn:aws:states:::states:startExecution.waitForTaskToken",
   "Parameters":{ 
      "Input":{
        "Comment": "Hello world!",
        "token.$": "$$.Task.Token"
       },
      "StateMachineArn":"arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
      "Name":"ExecutionName"
   },
   "End":true
}
```

To associate a nested workflow execution with the parent execution that started it, pass a specially named parameter that includes the execution ID pulled from the [context object](input-output-contextobject.md)\. When starting a nested execution, use a parameter named `AWS_STEP_FUNCTIONS_STARTED_BY_EXECUTION_ID`\. Pass the execution ID by appending `.$` to the parameter name, and referencing the ID in the context object with `$$.Execution.Id`\. For more information, see [Accessing the Context Object](input-output-contextobject.md#contextobject-access)\.

```
{  
   "Type":"Task",
   "Resource":"arn:aws:states:::states:startExecution.sync",
   "Parameters":{  
      "Input":{
        "Comment": "Hello world!",
        "AWS_STEP_FUNCTIONS_STARTED_BY_EXECUTION_ID.$": "$$.Execution.Id"
       },
      "StateMachineArn":"arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
      "Name":"ExecutionName"
   },
   "End":true
}
```

 Nested state machines return the following: 


| Resource | Output | 
| --- | --- | 
| startExecution\.sync | String | 
| startExecution\.sync:2 | JSON | 

Both will wait for the nested state machine to complete, but they return different `Output` formats\. For example, if you create a Lambda function that returns the object `{ "MyKey": "MyValue" }`, you would get the following responses:

For startExecution\.sync:

```
{
   <other fields>
   "Output": "{ \"MyKey\": \"MyValue\" }" 
}
```

For startExecution\.sync:2:

```
{
   <other fields> 
   "Output": {
      "MyKey": "MyValue"
   }
}
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.