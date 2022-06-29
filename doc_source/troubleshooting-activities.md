# Troubleshooting activities<a name="troubleshooting-activities"></a>

## My state machine execution is stuck at an activity state\.<a name="troubleshooting-activities-stuck-state-machine"></a>

An activity task state doesn't start until you poll a task token by using the [GetActivityTask](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html) API action\. As a best practice, add a task level timeout in order to avoid a stuck execution\. For more information, see [Use timeouts to avoid stuck executions](sfn-stuck-execution.md)\. 

## My activity worker times out while waiting for a task token\.<a name="troubleshooting-activity-worker-times-out"></a>

Workers use the [GetActivityTask](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html) API action to retrieve a task with the specified activity ARN that is scheduled for execution by a running state machine\. `GetActivityTask` starts a long poll, so the service holds the HTTP connection open and responds as soon as a task becomes available\. The maximum time the service hold the request before responding is 60 seconds\. If no task is available within 60 seconds, the poll returns a `taskToken` with a null string\. To avoid this timeout, configure a client side socket [with a timeout of at least 65](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html) seconds in the AWS SDK or in the client you are using to make the API call\. 