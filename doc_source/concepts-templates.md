# Templates<a name="concepts-templates"></a>

In the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/), you can choose one of the following state machine templates to automatically fill the **Code** pane\. Each of the templates is fully functional and you can use any blueprint as the template for your own state machine\.

**Note**  
Choosing any of the templates overwrites the contents of the **Code** pane\.
+ **Hello world** – A state machine with a `Pass` state\.
+ **Wait state** – A state machine that demonstrates different ways of injecting a `Wait` state into a running state machine:
  + By waiting for a number of seconds\.
  + By waiting for an absolute time \(timestamp\)\.
  + By specifying the `Wait` state's definition\.
  + By using the state's input data\.
+ **Retry failure** – A state machine that retries a task after the task fails\. This blueprint demonstrates how to handle multiple retries and various failure types\.
+ **Parallel** – A state machine that demonstrates how to execute two branches at the same time\.
+ **Catch failure** – A state machine that performs a different task after its primary task fails\. This blueprint demonstrates how to call different tasks depending on the failure type\.
+ **Choice state** – A state machine that makes a choice: It either runs a `Task` state from a set of `Task` states or runs a `Fail` state after the initial state is complete\.