# Best practices for Step Functions<a name="sfn-best-practices"></a>

The following best practices for implementing AWS Step Functions workflows can help you optimize the performance of your implementations\.

**Topics**
+ [Use timeouts to avoid stuck executions](sfn-stuck-execution.md)
+ [Use Amazon S3 ARNs instead of passing large payloads](avoid-exec-failures.md)
+ [Avoid reaching the history quota](bp-history-limit.md)
+ [Handle Lambda service exceptions](bp-lambda-serviceexception.md)
+ [Avoid latency when polling for activity tasks](bp-activity-pollers.md)
+ [Choosing Standard or Express Workflows](bp-express.md)
+ [Amazon CloudWatch Logs resource policy size restrictions](bp-cwl.md)