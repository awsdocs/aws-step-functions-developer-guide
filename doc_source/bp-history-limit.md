# Avoid Reaching the history quota<a name="bp-history-limit"></a>

AWS Step Functions has a hard quota of 25,000 entries in the execution history\. To avoid reaching this quota for long\-running executions, implement a pattern that uses an AWS Lambda function that can start a new execution of your state machine to split ongoing work across multiple workflow executions\.

For more information, see the [Continuing as a New Execution](tutorial-continue-new.md) tutorial\.