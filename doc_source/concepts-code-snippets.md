# Code Snippets<a name="concepts-code-snippets"></a>

In AWS Step Functions, code snippets are a way to easily configure the options for a new state in your state machine definition\. When you edit or create a state machine, the top of the code pane includes a **Generate code snippet** menu\. Selecting an option from the **Generate code snippet** menu opens a window to configure parameters specific to that state, and generates Amazon States Language code based on the options you choose\.

For example, if you choose the **AWS Batch: Manage a job** code snippet, you can configure the following:
+ **Batch job name** – You can either specify the job name, or specify it at runtime using a path\.
+ **Batch job definition** – You can select the ARN of an existing AWS Batch job in your account, enter the job definition, or choose to specify it at runtime using a path\.
+ **Batch job queue** – You can select the ARN of an existing AWS Batch job queue in your account, enter the job queue definition, or choose to specify it at runtime using a path\.
+ **Run synchronously** – Selecting this option configures Step Functions to wait until the AWS Batch job completes before continuing to the next state\.

**Note**  
For more information about specifying service parameters, see [Pass Parameters to a Service API](connect-parameters.md)\.

After you configure your AWS Batch options, you can specify error handling options for your state, such as `Retry`, `Catch`, and `TimeoutSeconds.` 

For more information, see [Error Names](concepts-error-handling.md#error-handling-error-representation) in Amazon States Language\.

To learn more about Step Functions service integrations, see:
+ [AWS Service Integrations](concepts-service-integrations.md)
+ [Supported AWS Service Integrations for Step Functions](connect-supported-services.md)
+ [Using Code Snippets](tutorial-code-snippet.md)