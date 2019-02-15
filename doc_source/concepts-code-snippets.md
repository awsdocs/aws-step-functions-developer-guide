# Code Snippets<a name="concepts-code-snippets"></a>

Code snippets are a way to easily configure the options for a new state in your state machine definition\. When you edit or create a state machine, the top of the code pane will include a **Generate code snippet** menu\. Selecting an option from the **Generate code snippet** menu will open a window to configure parameters specific to that state, and will generate Amazon States Language code based on the options you choose\.

For instance, if you choose the **AWS Batch: Manage a job** code snippet, you can configure:
+ **Batch job name** — You can either specify the job name, or specify it at runtime using a path\.
+ **Batch job definition** — You can select the ARN of an existing an existing AWS Batch job in your account, enter the job definition, or choose to specify it at runtime using a path\.
+ **Batch job queue** — You can select the ARN of an existing an existing AWS Batch job queue in your account, enter the job queue definition, or choose to specify it at runtime using a path\.
+ **Run synchronously** — Selecting this option will configure Step Functions to wait until the AWS Batch job completes before continuing to the next state\.

**Note**  
For more information on specifying service parameters by using input to your state machine execution, see [Pass State Input as Parameters Using Paths](connectors-parameters.md#connectors-parameters-path)\.

Once you have configured your AWS Batch options you can specify error handling options for your state, such as `Retry`, `Catch`, and `TimeoutSeconds.` For more information, see [Errors](amazon-states-language-errors.md) in the Amazon States Language section\.

To learn more about Step Functions service integrations, see:
+ [AWS Service Integrations](concepts-connectors.md)
+ [Supported AWS Service Integrations for Step Functions](connectors-supported-services.md)
+ [Using Code Snippets](tutorial-code-snippet.md)