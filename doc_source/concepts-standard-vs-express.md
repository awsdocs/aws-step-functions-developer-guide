# Standard vs\. Express Workflows<a name="concepts-standard-vs-express"></a>

When you create a state machine, you can select a **Type** of either **Standard** \(default\) or **Express**\. In both cases, you define your state machine using the Amazon States Language\. Your state machine executions will behave differently, depending on which **Type** you select\. The **Type** you choose cannot be changed after your state machine has been created\. 

Standard Workflows are ideal for long\-running, durable, and auditable workflows\. They can run for up to a year and you can retrieve the full execution history using the [Step Functions API](https://docs.aws.amazon.com/step-functions/latest/apireference), up to 90 days after your execution completes\. Standard Workflows employ an at\-most\-once model, where your tasks and states are never executed more than once unless you have specified `Retry` behavior in ASL\. This makes them suited to orchestrating non\-idempotent actions, such as starting an Amazon EMR cluster or processing payments\. Standard Workflows executions are billed according to the number of state transitions processed\. 

Express Workflows are ideal for high\-volume, event\-processing workloads such as IoT data ingestion, streaming data processing and transformation, and mobile application backends\. They can run for up to five minutes\. Express Workflows employ an at\-least\-once model, where there is a possibility that an execution might be run more than once\. This makes them ideal for orchestrating idempotent actions such as transforming input data and storing via PUT in Amazon DynamoDB\. Express Workflow executions are billed by the number of executions, the duration of execution, and the memory consumed\. 

Standard and Express Workflows can automatically start in response to events such as HTTP requests via Amazon API Gateway \(fully\-managed APIs at scale\), IoT Rules and over 140 event sources in Amazon EventBridge\.


**Standard vs Express Workflows**  

|  | Standard Workflows | Express Workflows: Synchronous and Asynchronous | 
| --- | --- | --- | 
| Maximum duration | 1 year\. | 5 minutes\. | 
| [Supported execution start rate](limits-overview.md) | Over 2,000 per second | Over 100,000 per second | 
| [Supported state transition rate](limits-overview.md) | Over 4,000 per second per account | Nearly unlimited | 
| [Pricing](http://aws.amazon.com/step-functions/pricing) | Priced per state transition\. A state transition is counted each time a step in your execution is completed\. | Priced by the number of executions you run, their duration, and memory consumption\.  | 
| Execution history | Executions can be listed and described with Step Functions APIs, and visually debugged through the console\. They can also be inspected in CloudWatch Logs by enabling logging on your state machine\. | Executions can be inspected in CloudWatch Logs by enabling logging on your state machine\. | 
| [Execution semantics](express-at-least-once-execution.md) | Exactly\-once workflow execution\. | *Asynchronous Express Workflows*: At\-least\-once workflow execution\. *Synchronous Express Workflows*: At\-most\-once workflow execution\. | 
| [Service integrations](concepts-service-integrations.md) | Supports all service integrations and patterns\. | Supports all service integrations\. Does not support Job\-run \(\.sync\) or Callback \(\.waitForTaskToken\) patterns\. | 
| Step Functions activities | Supports Step Functions activities\. | Does not support Step Functions activities\. | 