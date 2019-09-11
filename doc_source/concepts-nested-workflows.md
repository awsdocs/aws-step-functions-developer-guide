# Start Workflow Executions From a Task State<a name="concepts-nested-workflows"></a>

AWS Step Functions can start workflow executions directly from a `Task` state of a state machine\. This allows you to break your workflows into smaller state machines, and to start executions of these other state machines\. By starting these new workflow executions you can:
+ Separate higher level workflow from lower level, task\-specific workflows\.
+ Avoid repetitive elements by calling a separate state machine multiple times\.
+ Create a library of modular reusable workflows for faster development\.
+ Reduce complexity and make it easier to edit and troubleshoot state machines\.

Step Functions can start these workflow executions by calling its own API as an [integrated service](concepts-service-integrations.md)\. Simply call the `StartExecution` API action from your `Task` state and pass the necessary parameters\. You can call the Step Functions API using any of the [service integration patterns](connect-to-resource.md)\. To start a new execution of a state machine, use a `Task` state similar to the following\.

```
{  
   "Type":"Task",
   "Resource":"arn:aws:states:::states:startExecution",
   "Parameters":{  
      "StateMachineArn":"arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
      "Input":{  
         "Comment":"Hello world!"
      },
      "Retry":[  
         {  
            "ErrorEquals":[  
               "StepFunctions.ExecutionLimitExceeded"
            ]
         }
      ],
      "End":true
   }
}
```

This `Task` state will start a new execution of the `HelloWorld` state machine, and will pass the JSON comment as input\.

**Note**  
The `StartExecution` API action limits can limit the number of executions that you can start\. Use the `Retry` on `StepFunctions.ExecutionLimitExceeded` to ensure your execution is started\. See the following\.  
[Limits Related to API Action Throttling](limits.md#service-limits-api-action-throttling)
[Error Handling](concepts-error-handling.md)

## Associate Workflow Executions<a name="nested-execution-startid"></a>

To associate a started workflow execution with the execution that started it, pass the execution ID from the context object to the execution input\. You can access the ID from the context object from your `Task` state in a running execution\. Pass the execution ID by appending `.$` to the parameter name, and referencing the ID in the context object with `$$.Execution.Id`\.

```
"AWS_STEP_FUNCTIONS_STARTED_BY_EXECUTION_ID.$": "$$.Execution.Id"
```

You can use a special parameter named `AWS_STEP_FUNCTIONS_STARTED_BY_EXECUTION_ID` when you start an execution\. If included, this association provides links in the **Step details** section of the Step Functions console\. When provided, you can easily trace the executions of your workflows from starting executions to their started workflow executions\. Using the previous example, associate the execution ID with the started execution of the `HelloWorld` state machine as follows\.

```
{  
   "Type":"Task",
   "Resource":"arn:aws:states:::states:startExecution",
   "Parameters":{  
      "StateMachineArn":"arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
      "Input": {
        "Comment": "Hello world!",
        "AWS_STEP_FUNCTIONS_STARTED_BY_EXECUTION_ID.$": "$$.Execution.Id"
       }
   },
   "End":true
}
```

For more information, see:
+ [Service Integrations](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)
+ [Accessing the Context Object](input-output-contextobject.md#contextobject-access)
+ [AWS Step Functions](connect-stepfunctions.md)