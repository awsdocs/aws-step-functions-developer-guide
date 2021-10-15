# Call a microservice running on Fargate using API Gateway integration<a name="sample-apigateway-ecs-workflow"></a>

This sample project demonstrates how to use Step Functions to make a call to API Gateway in order to interact with a service on AWS Fargate, and also to check whether the call succeeded\. This sample project creates the following:
+ An Amazon API Gateway HTTP API that is called by the state machine\.
+ An Amazon API Gateway Amazon VPC Link\.
+ An Amazon Virtual Private Cloud\.
+ An Application Load Balancer\.
+ A Fargate cluster\.
+ An Amazon SNS topic
+ Related AWS Identity and Access Management \(IAM\) roles
+ Several additional services that are required to enable these resources to work together\.

For more information about API Gateway and Step Functions service integrations, see the following:
+ [Using AWS Step Functions with other services](concepts-service-integrations.md)
+ [Call API Gateway with Step Functions](connect-api-gateway.md)

**Note**  
This sample project may incur charges\.  
For new AWS users, a free usage tier is available\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the Free Tier, see [Pricing](http://aws.amazon.com/step-functions/pricing)\.

## Create the State Machine and Provision Resources<a name="sample-apigateway-ecs-workflow-create"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) and choose **Create a state machine**\.

1. Choose **Sample Projects**, and then choose **Call a microservice with API Gateway**\.

   The state machine **Code** and **Visual Workflow** are displayed\.  
![\[Training model workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sample-apigateway-ecs-workflow.png)

1. Choose **Next**\.

   The **Deploy resources** page is displayed, listing the resources that will be created\. For this sample project, the resources include:
   + An API Gateway HTTP API
   + An API Gateway VpcLink
   + An Application Load Balancer
   + A Fargate cluster
   + A state machine
   + An Amazon SNS topic
   + Related IAM roles
   + Several additional services that are required to enable these resources to work together\.

1. Choose **Deploy Resources**\.
**Note**  
It can take up to 10 minutes for these resources and related IAM permissions to be created\. While the **Deploy resources** page is displayed, you can open the **Stack ID** link to see which resources are being provisioned\.

## Start a New Execution<a name="sample-apigateway-ecs-workflow-start-execution"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home)\.

1. On the **State machines** page, choose the **ApiGatewayECSStateMachine** state machine that was created by the sample project, and then choose **Start execution**\.

1. On the **New execution** page, enter an execution name \(optional\), and then choose **Start Execution**\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. \(Optional\) Go to the newly created state machine on the Step Functions **Dashboard**, and then choose **New execution**\.

1. When an execution is complete, you can select states on the **Visual workflow** and browse the **Input** and **Output** under **Step details**\.

## Example State Machine Code<a name="sample-apigateway-ecs-workflow-code-examples"></a>

The state machine in this sample project integrates with API Gateway by calling an API Gateway HTTP API that is connected to a service on Fargate\. This is hosted on a private subnet, and accessed through a private application load balancer\.

Browse through this example state machine to see how Step Functions interacts with API Gateway and returns results\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

```
{
  "Comment": "Calling APIGW HTTP Endpoint",
  "StartAt": "Call API",
  "States": {
    "Call API": {
      "Type": "Task",
      "Resource": "arn:<PARTITION>:states:::apigateway:invoke",
      "Parameters": {
        "ApiEndpoint": "<API_ENDPOINT>",
        "Method": "GET",
        "AuthType": "IAM_ROLE"
      },
      "Next": "Call Successful?"
    },
    "Call Successful?": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.StatusCode",
          "NumericEquals": 200,
          "Next": "Notify Success"
        }
      ],
      "Default": "Notify Failure"
    },
    "Notify Success": {
      "Type": "Task",
      "Resource": "arn:<PARTITION>:states:::sns:publish",
      "Parameters": {
        "Message": "Call was successful",
        "TopicArn": "<SNS_TOPIC_ARN>"
      },
      "End": true
    },
    "Notify Failure": {
      "Type": "Task",
      "Resource": "arn:<PARTITION>:states:::sns:publish",
      "Parameters": {
        "Message": "Call was not successful",
        "TopicArn": "<SNS_TOPIC_ARN<"
      },
      "End": true
    }
  }
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.

## IAM Example<a name="sample-apigateway-ecs-workflow-iam-example"></a>

These example AWS Identity and Access Management \(IAM\) policies generated by the sample project include the least privilege necessary to execute the state machine and related resources\. We recommend that you include only those permissions that are necessary in your IAM policies\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sns:Publish"
            ],
            "Resource": [
                "arn:aws:sns:us-east-1:111122223333:apigw-ecs-sample-2000-SNSTopic-444455556666"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "execute-api:Invoke"
            ],
            "Resource": [
                "arn:aws:execute-api:us-east-1:111122223333:444444444444/*/GET/*"
            ],
            "Effect": "Allow"
        }
    ]
}
```

```
{
    "Statement": [
        {
            "Action": [
                "ec2:AttachNetworkInterface",
                "ec2:CreateNetworkInterface",
                "ec2:CreateNetworkInterfacePermission",
                "ec2:DeleteNetworkInterface",
                "ec2:DeleteNetworkInterfacePermission",
                "ec2:Describe*",
                "ec2:DetachNetworkInterface",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
                "elasticloadbalancing:DeregisterTargets",
                "elasticloadbalancing:Describe*",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "elasticloadbalancing:RegisterTargets"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

```
{
    "Statement": [
        {
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

For information about how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.