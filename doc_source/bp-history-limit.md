# Iterate a Count to Avoid Reaching the History Limit<a name="bp-history-limit"></a>

AWS Step Functions has a hard limit of 25,000 entries in the execution history\. To avoid reaching this limit for long\-running executions, implement a pattern that uses an AWS Lambda function to iterate a count, as shown\.

![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-create-iterate-workflow.png)![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[State machine workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

For a simple state machine, as shown in this example, there are three state machine transitions per iteration\. To safely keep your execution history below 25,000 entries, you could cap the iterations at 8,000, so that you start a new execution when the execution history is around 16,000\.

For more information, see the [Iterating a Loop Using Lambda](tutorial-create-iterate-pattern-section.md) tutorial\.