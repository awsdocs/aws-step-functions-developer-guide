# Log levels<a name="cloudwatch-log-level"></a>

You can choose from `OFF`, `ALL`, `ERROR`, or `FATAL`\. No event types log when set to `OFF` and all event types do when set to `ALL`\. For `ERROR` and `FATAL`, see the following table\.


| Event Type | `ALL` | `ERROR` | `FATAL` | `OFF` | 
| --- | --- | --- | --- | --- | 
|  ChoiceStateEntered  | ✓ |  |  |  | 
|  ChoiceStateExited  | ✓ |  |  |  | 
|  ExecutionAborted  | ✓ |  ✓  |  ✓  |  | 
|  ExecutionFailed  | ✓ |  ✓  |  ✓  |  | 
|  ExecutionStarted  | ✓ |  |  |  | 
|  ExecutionSucceeded  | ✓ |  |  |  | 
|  ExecutionTimedOut  | ✓ |  ✓  |  ✓  |  | 
|  FailStateEntered  | ✓ | ✓ |   |  | 
|  LambdaFunctionFailed  | ✓ |  ✓  |  |  | 
| LambdaFunctionScheduled | ✓ |  |  |  | 
|  LambdaFunctionScheduleFailed  | ✓ |  ✓  |  |  | 
|  LambdaFunctionStarted  | ✓ |  |  |  | 
|  LambdaFunctionStartFailed  | ✓ |  ✓  |  |  | 
|  LambdaFunctionSucceeded  | ✓ |  |  |  | 
|  LambdaFunctionTimedOut  | ✓ |  ✓  |  |  | 
|  MapIterationAborted  | ✓ |  ✓  |  |  | 
|  MapIterationFailed  | ✓ |  ✓  |  |  | 
|  MapIterationStarted  | ✓ |  |  |  | 
|  MapIterationSucceeded  | ✓ |  |  |  | 
|  MapStateAborted  | ✓ |  ✓  |  |  | 
|  MapStateEntered  | ✓ |  |  |  | 
|  MapStateExited  | ✓ |  |  |  | 
|  MapStateFailed  | ✓ |  ✓  |  |  | 
|  MapStateStarted  | ✓ |  |  |  | 
|  MapStateSucceeded  | ✓ |  |  |  | 
|  ParallelStateAborted  | ✓ |  ✓  |  |  | 
|  ParallelStateEntered  | ✓ |  |  |  | 
|  ParallelStateExited  | ✓ |  |  |  | 
| ParallelStateFailed | ✓ |  ✓  |  |  | 
|  ParallelStateStarted  | ✓ |  |  |  | 
|  ParallelStateSucceeded  | ✓ |  |  |  | 
|  PassStateEntered  | ✓ |  |  |  | 
|  PassStateExited  | ✓ |  |  |  | 
|  SucceedStateEntered  | ✓ |  |  |  | 
|  SucceedStateExited  | ✓ |  |  |  | 
|  TaskFailed  | ✓ |  ✓  |  |  | 
|  TaskScheduled  | ✓ |  |  |  | 
| TaskStarted | ✓ |  |  |  | 
|  TaskStartFailed  | ✓ |  ✓  |  |  | 
|  TaskStateAborted  | ✓ |  ✓  |  |  | 
|  TaskStateEntered  | ✓ |  |  |  | 
| TaskStateExited | ✓ |  |  |  | 
| TaskSubmitFailed | ✓ | ✓ |  |  | 
| TaskSubmitted | ✓ |  |  |  | 
| TaskSucceeded | ✓ |  |  |  | 
| TaskTimedOut | ✓ | ✓ |  |  | 
| WaitStateAborted | ✓ | ✓ |  |  | 
| WaitStateEntered | ✓ |  |  |  | 
| WaitStateExited | ✓ |  |  |  | 