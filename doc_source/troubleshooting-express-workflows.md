# Troubleshooting Express Workflows<a name="troubleshooting-express-workflows"></a>

## My application times out before receiving a response from a `[StartSyncExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartSyncExecution.html)` API call\.<a name="troubleshooting-express-workflows-timeouts"></a>

Configure a client side socket timeout in the AWS SDK or client you are using to make the API call\. To receive a response, the timeout must have a value higher than the duration of Express Workflow executions\.

## I'm unable to see the execution history in order to troubleshoot Express Workflow failures\.<a name="troubleshooting-express-workflows-no-execution-history"></a>

Express Workflows do not have an execution history\. Instead, you need to enable CloudWatch logging\. Then you can use CloudWatch Logs Insights queries to better understand your Express Workflow executions\. 

To list executions based on duration:

```
fields ispresent(execution_arn) as exec_arn
| filter exec_arn 
| filter type in ["ExecutionStarted", "ExecutionSucceeded", "ExecutionFailed", "ExecutionAborted", "ExecutionTimedOut"]
| stats latest(type) as status, 
  tomillis(earliest(event_timestamp)) as UTC_starttime, 
  tomillis(latest(event_timestamp)) as UTC_endtime, 
  latest(event_timestamp) - earliest(event_timestamp) as duration_in_ms  by execution_arn
| sort duration desc
```

To list failed and cancelled executions:

```
fields ispresent(execution_arn) as isRes | filter type in ["ExecutionFailed", "ExecutionAborted", "ExecutionTimedOut"]
```