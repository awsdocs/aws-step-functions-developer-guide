# Avoid Latency When Polling for Activity Tasks<a name="bp-activity-pollers"></a>

The `[GetActivityTask](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html)` API is designed to provide a [https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html#StepFunctions-GetActivityTask-response-taskToken](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html#StepFunctions-GetActivityTask-response-taskToken) *exactly\-once*\. If a `taskToken` is dropped while communicating with an activity worker, a number of `GetActivityTask` requests can be blocked for 60 seconds waiting for a response until `GetActivityTask` times out\. If you only have a small number of polls waiting for a response, it is possible that all requests will queue up behind the blocked request and stop\. However, if you have a large number of outstanding polls for each activity ARN, and some percentage of your requests are stuck waiting, there will be many more that can still get a `taskToken` and begin to process work\. 

For production systems, we recommend at least 100 open polls per activity ARN's at each point in time\. If one poll gets blocked, and a portion of those polls queue up behind it, there are still many more requests that will receive a `taskToken` to process work while the `GetActivityTask` request is blocked\. 

To avoid these kinds of latency problems when polling for tasks:
+ Implement your pollers as separate threads from the work in your activity worker implementation\.
+ Have at least 100 open polls per activity ARN at each point in time\.
**Note**  
Scaling to 100 open polls per ARN can be expensive\. For example, 100 Lambda functions polling per ARN is 100 times more expensive than having a single Lambda function with 100 polling threads\. To both reduce latency *and* minimize cost, use a language that has asynchronous I/O, and implement multiple polling threads per worker\. For an example activity worker where the poller threads are separate from the work threads, see [Example Activity Worker in Ruby](example-ruby-activity-worker.md)\. 

For more information on activities and activity workers see [Activities](concepts-activities.md)\.