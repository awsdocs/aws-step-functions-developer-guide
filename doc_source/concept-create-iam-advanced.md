# Creating Granular IAM Permissions for Non\-Admin Users<a name="concept-create-iam-advanced"></a>

The default managed policies in IAM, such as `ReadOnly`, don't fully cover all types of Step Functions permissions\. This section describes these different types of permissions and provides some example configurations\. 

AWS Step Functions has four categories of permissions\. Depending on what access you want to provide to a user, you can control access by using permissions in these categories\. 

[Service\-Level Permissions](#concept-create-iam-advanced-service)  
Apply to components of the API that don't act on a specific resource\.

[State Machine\-Level Permissions](#concept-create-iam-advanced-state)  
Apply to all API components that act on a specific state machine\.

[Execution\-Level Permissions](#concept-create-iam-advanced-execution)  
Apply to all API components that act on a specific execution\.

[Activity\-Level Permissions](#concept-create-iam-advanced-activity)  
Apply to all API components that act on a specific activity or on a particular instance of an activity\.

## Service\-Level Permissions<a name="concept-create-iam-advanced-service"></a>

This permission level applies to all API actions that don't act on a specific resource\. These include `[CreateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateStateMachine.html)`, `[CreateActivity](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateActivity.html)`, `[ListStateMachines](https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListStateMachines.html)`, and `[ListActivities](https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListActivities.html)`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "states:ListStateMachines",
        "states:ListActivities",
        "states:CreateStateMachine",
        "states:CreateActivity"
      ],
      "Resource": [ 
        "arn:aws:states:*:*:*" 
      ]
    },
    {
      "Effect": "Allow",
      "Action": [ 
        "iam:PassRole"
      ],
      "Resource": [
        "arn:aws:iam:::role/my-execution-role"
      ]
    }
  ]
}
```

## State Machine\-Level Permissions<a name="concept-create-iam-advanced-state"></a>

This permission level applies to all API actions that act on a specific state machine\. These API operations require the Amazon Resource Name \(ARN\) of the state machine as part of the request, such as `[DeleteStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DeleteStateMachine.html)`, `[DescribeStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeStateMachine.html)`, `[StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html)`, and `[ListExecutions](https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListExecutions.html)`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "states:DescribeStateMachine",
        "states:StartExecution",
        "states:DeleteStateMachine",
        "states:ListExecutions",
        "states:UpdateStateMachine"
      ],
      "Resource": [ 
        "arn:aws:states:*:*:stateMachine:StateMachinePrefix*" 
      ]
    }
  ]
}
```

## Execution\-Level Permissions<a name="concept-create-iam-advanced-execution"></a>

This permission level applies to all the API actions that act on a specific execution\. These API operations require the ARN of the execution as part of the request, such as `[DescribeExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html)`, `[GetExecutionHistory](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetExecutionHistory.html)`, and `[StopExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StopExecution.html)`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "states:DescribeExecution",
        "states:DescribeStateMachineForExecution",
        "states:GetExecutionHistory",
        "states:StopExecution"
      ],
      "Resource": [ 
        "arn:aws:states:*:*:execution:*:ExecutionPrefix*"
      ]
    }
  ]
}
```

## Activity\-Level Permissions<a name="concept-create-iam-advanced-activity"></a>

This permission level applies to all the API actions that act on a specific activity or on a particular instance of it\. These API operations require the ARN of the activity or the token of the instance as part of the request, such as `[DeleteActivity](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DeleteActivity.html)`, `[DescribeActivity](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeActivity.html)`, `[GetActivityTask](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetActivityTask.html)`, `[SendTaskSuccess](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskSuccess.html)`, `[SendTaskFailure](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskFailure.html)`, and `[SendTaskHeartbeat](https://docs.aws.amazon.com/step-functions/latest/apireference/API_SendTaskHeartbeat.html)`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "states:DescribeActivity",
        "states:DeleteActivity",
        "states:GetActivityTask",
        "states:SendTaskSuccess",
        "states:SendTaskFailure",
        "states:SendTaskHeartbeat"
      ],
      "Resource": [
        "arn:aws:states:*:*:activity:ActivityPrefix*"
      ]
    }
  ]
}
```