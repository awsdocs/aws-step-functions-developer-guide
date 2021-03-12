# Choosing Standard or Express Workflows<a name="bp-express"></a>

AWS Step Functions offers Standard Workflows as the default workflow type, with the option to choose Express Workflows\.

You can choose Standard Workflows when you need long\-running, durable, and auditable workflows, or Express Workflows for high\-volume, event processing workloads\. Your state machine executions will behave differently, depending on which `Type` you select\. The `Type` you choose cannot be changed after your state machine has been created\.

For detailed information on the differences between Standard and Express Workflows, see [Standard vs\. Express Workflows](concepts-standard-vs-express.md)\.