# Development options<a name="development-options"></a>

You can implement your AWS Step Functions state machines in several ways, such as using the console, the SDKs, or a local version for testing and development\. 

**Topics**
+ [Step Functions console](#development-options-console)
+ [AWS SDKs](#development-options-aws-sdk)
+ [Standard and Express workflows](#development-options-standard-express)
+ [HTTPS service API](#development-options-service-api)
+ [Development environments](#development-options-environments)
+ [Endpoints](#development-options-endpoints)
+ [AWS CLI](#development-options-aws-cli)
+ [Step Functions Local](#development-options-local)
+ [AWS Toolkit for Visual Studio Code](#development-options-toolkit)
+ [AWS Serverless Application Model and Step Functions](#development-options-sam)
+ [Definition format support](#development-options-format)

## Step Functions console<a name="development-options-console"></a>

You can define a state machine using the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\. You can write complex state machines in the cloud without using a local development environment by using AWS Lambda to supply code for your tasks, and the Step Functions console to define your state machine using the Amazon States Language\.

The [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md) tutorial uses this technique to create a simple state machine, execute it, and view its results\.

### Data flow simulator<a name="development-options-console-simulator"></a>

 You can design, implement and debug workflows in the Step Functions console\. Knowing how JsonPath input and output processing work can help you understand how to control the flow of data through your state machine\. Use the [ data flow simulator in the Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/simulator) to learn how information flows from state to state and to understand how to filter and manipulate date\. This tool lets you simulate each of the [fields](concepts-input-output-filtering.md) that Step Functions uses to process data, such as `InputPath`, `Parameters`, `ResultSelector`, `OutputPath`, and `ResultPath`\.

## AWS SDKs<a name="development-options-aws-sdk"></a>

Step Functions is supported by the AWS SDKs for Java, \.NET, Ruby, PHP, Python \(Boto 3\), JavaScript, Go, and C\+\+\. These SDKs provide a convenient way to use the Step Functions HTTPS API actions in various programming languages\.

You can develop state machines, activities, or state machine starters using the API actions exposed by these SDK libraries\. You can also access visibility operations using these libraries to develop your own Step Functions monitoring and reporting tools\.

To use Step Functions with other AWS services, see the reference documentation for the current AWS SDKs and [Tools for Amazon Web Services](http://aws.amazon.com/tools/)\.

**Note**  
Step Functions supports only an HTTPS endpoint\.

## Standard and Express workflows<a name="development-options-standard-express"></a>

When you create a new state machine, you must select a `Type` of either **Standard** or **Express**\. In both cases, you define your state machine using the Amazon States Language\. Your state machine executions will behave differently, depending on which **Type** you select\. The **Type** you choose cannot be changed after your state machine is created\.

See [Logging using CloudWatch Logs](cw-logs.md) for more information\.

## HTTPS service API<a name="development-options-service-api"></a>

Step Functions provides service operations that are accessible through HTTPS requests\. You can use these operations to communicate directly with Step Functions and to develop your own libraries in any language that can communicate with Step Functions through HTTPS\.

You can develop state machines, workers, or state machine starters using the service API actions\. You can also access visibility operations through the API actions to develop your own monitoring and reporting tools\. 

For detailed information about API actions, see the [AWS Step Functions API Reference](https://docs.aws.amazon.com/step-functions/latest/apireference/)\.

## Development environments<a name="development-options-environments"></a>

You must set up a development environment that's appropriate to the programming language that you plan to use\.

For example, to develop for Step Functions using Java, you should install a Java development environment \(such as the AWS SDK for Java\) on each of your development workstations\. If you use Eclipse IDE for Java Developers, you should also install the AWS Toolkit for Eclipse\. This Eclipse plugin adds features that are useful for developing on AWS\.

If your programming language requires a runtime environment, you must set up the environment on each computer where these processes run\.

## Endpoints<a name="development-options-endpoints"></a>

To reduce latency and to store data in a location that meets your requirements, Step Functions provides endpoints in different AWS Regions\.

Each endpoint in Step Functions is completely independent\. A state machine or activity exists only within the Region where it was created\. Any state machines and activities that you create in one Region don't share any data or attributes with those created in another Region\. For example, you can register a state machine named `STATES-Flows-1` in two different Regions, but the two state machines won't share data or attributes with each other because they are completely independent from each other\.

For a list of Step Functions endpoints, see [Regions and Endpoints: AWS Step Functions](https://docs.aws.amazon.com/general/latest/gr/rande.html#step-functions_region) in the *AWS General Reference*\.

## AWS CLI<a name="development-options-aws-cli"></a>

You can access many Step Functions features from the AWS Command Line Interface \(AWS CLI\)\. The AWS CLI provides an alternative to using the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) or, in some cases, to programming using the Step Functions API actions\. For example, you can use the AWS CLI to create a state machine and then list your state machines\.

The Step Functions commands in the AWS CLI enable you to start and manage executions, poll for activities, record task heartbeats, and so on\. For a complete list of Step Functions commands and the descriptions of the available arguments and examples showing their use, see the *AWS CLI Command Reference*\.

The AWS CLI commands follow the Amazon States Language closely, so you can use the AWS CLI to learn about the Step Functions API actions\. You can also use your existing API knowledge to prototype code or perform Step Functions actions from the command line\.

## Step Functions Local<a name="development-options-local"></a>

For testing and development purposes, you can install and run Step Functions on your local machine\. With Step Functions Local, you can start an execution on any machine\. 

The local version of Step Functions can invoke AWS Lambda functions, both in AWS and running locally\. You can also coordinate other [supported AWS services](concepts-service-integrations.md)\. For more information, see [Setting Up Step Functions Local \(Downloadable Version\)](sfn-local.md)\.

## AWS Toolkit for Visual Studio Code<a name="development-options-toolkit"></a>

 You can use VS Code to interact with remote state machines, and develop state machines locally\. You can create or update state machines, list existing state machines, execute them, and download them\. VS Code also lets you create new state machines from templates, see a visualization of your state machine, and provides code snippets, code completion, and code validation\. 

For more information, see [the AWS Toolkit for Visual Studio Code User Guide](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/bulding-stepfunctions.html) 

## AWS Serverless Application Model and Step Functions<a name="development-options-sam"></a>

Step Functions is integrated with the AWS Serverless Application Model, which lets you integrate workflows with Lambda functions, APIs and events to create serverless applications\.

You can also use the AWS SAM CLI in conjunction with the AWS Toolkit for Visual Studio Code as part of an integrated experience\.

For more information, see [AWS Step Functions and AWS SAM](concepts-sam-sfn.md)\.

## Definition format support<a name="development-options-format"></a>

Step Functions offers a variety of tooling that lets you provide your state machine definitions in different formats\. An Amazon States Language \(ASL\) definition that specifies the details of your state machine can be provided in either string form or as a serialized object using JSON or YAML\. 

**Note**  
YAML allows single line comments\. Any YAML comments provided in the state machine definition portion of a template will not be carried forward into the created resource’s definition\. Instead, you can use the `Comment` property within the state machine definition\. For more information, see the [State Machine Structure](amazon-states-language-state-machine-structure.md) page\.

The following table shows which tools support ASL\-based definitions\. 


**Definition format support by tool**  

|  | JSON | YAML | Stringified Amazon States Language | 
| --- | --- | --- | --- | 
| [Step Functions Console](#development-options-console) | ✓ |  |  | 
| [HTTPS Service API](#development-options-service-api) |  |  | ✓ | 
| [AWS CLI](#development-options-aws-cli) |  |  | ✓ | 
| [Step Functions Local](#development-options-local) |  |  | ✓ | 
| [AWS Toolkit for Visual Studio Code](#development-options-toolkit) | ✓ | ✓ |  | 
| [AWS SAM](#development-options-sam) | ✓ | ✓ |  | 
| [AWS CloudFormation](tutorial-lambda-state-machine-cloudformation.md) | ✓ | ✓ | ✓ | 

**Note**  
AWS CloudFormation and AWS SAM also allow you to upload your state machine definitions to Amazon S3 in JSON or YAML format, and to provide the definition's Amazon S3 location in the template\. This can improve the readability of your templates when your state machine definition is complex\. For more information see the [AWS::StepFunctions::StateMachine S3Location](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-stepfunctions-statemachine-s3location.html) page\. 

 The following example AWS CloudFormation templates show how you can provide the same state machine definition using different input formats\. 

------
#### [ JSON with Definition ]

```
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "AWS Step Functions sample template.",
  "Resources": {
    "MyStateMachine": {
      "Type": "AWS::StepFunctions::StateMachine",
      "Properties": {
        "RoleArn": {
          "Fn::GetAtt": [ "StateMachineRole", "Arn" ]
        },
        "TracingConfiguration": {
          "Enabled": true
        },
        "Definition": {
          "StartAt": "HelloWorld",
          "States": {
            "HelloWorld": {
              "Type": "Pass",
              "End": true
            }
          }
        }
      }
    },
    "StateMachineRole": {
      "Type": "AWS::IAM::Role",
      "Properties": {
        "AssumeRolePolicyDocument": {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Action": [
                "sts:AssumeRole"
              ],
              "Effect": "Allow",
              "Principal": {
                "Service": [
                  "states.amazonaws.com"
                ]
              }
            }
          ]
        },
        "ManagedPolicyArns": [],
        "Policies": [
          {
            "PolicyName": "StateMachineRolePolicy",
            "PolicyDocument": {
              "Statement": [
                {
                  "Action": [
                    "lambda:InvokeFunction"
                  ],
                  "Resource": "*",
                  "Effect": "Allow"
                }
              ]
            }
          }
        ]
      }
    }
  },
  "Outputs": {
    "StateMachineArn": {
      "Value": {
        "Ref": "MyStateMachine"
      }
    }
  }
}
```

------
#### [ JSON with DefinitionString ]

```
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "AWS Step Functions sample template.",
  "Resources": {
    "MyStateMachine": {
      "Type": "AWS::StepFunctions::StateMachine",
      "Properties": {
        "RoleArn": {
          "Fn::GetAtt": [ "StateMachineRole", "Arn" ]
        },
        "TracingConfiguration": {
          "Enabled": true
        },
        "DefinitionString": "{\n  \"StartAt\": \"HelloWorld\",\n  \"States\": {\n    \"HelloWorld\": {\n      \"Type\": \"Pass\",\n      \"End\": true\n    }\n  }\n}"
      }
    },
    "StateMachineRole": {
      "Type": "AWS::IAM::Role",
      "Properties": {
        "AssumeRolePolicyDocument": {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Action": [
                "sts:AssumeRole"
              ],
              "Effect": "Allow",
              "Principal": {
                "Service": [
                  "states.amazonaws.com"
                ]
              }
            }
          ]
        },
        "ManagedPolicyArns": [],
        "Policies": [
          {
            "PolicyName": "StateMachineRolePolicy",
            "PolicyDocument": {
              "Statement": [
                {
                  "Action": [
                    "lambda:InvokeFunction"
                  ],
                  "Resource": "*",
                  "Effect": "Allow"
                }
              ]
            }
          }
        ]
      }
    }
  },
  "Outputs": {
    "StateMachineArn": {
      "Value": {
        "Ref": "MyStateMachine"
      }
    }
  }
}
```

------
#### [ YAML with Definition ]

```
AWSTemplateFormatVersion: 2010-09-09
Description: AWS Step Functions sample template.
Resources:
  MyStateMachine:
    Type: 'AWS::StepFunctions::StateMachine'
    Properties:
      RoleArn: !GetAtt
        - StateMachineRole
        - Arn
      TracingConfiguration:
        Enabled: true
      Definition:
        # This is a YAML comment. This will not be preserved in the state machine resource's definition.
        Comment: This is an ASL comment. This will be preserved in the state machine resource's definition.
        StartAt: HelloWorld
        States:
          HelloWorld:
            Type: Pass
            End: true
  StateMachineRole:
    Type: 'AWS::IAM::Role'
    Properties:
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Action:
              - 'sts:AssumeRole'
            Effect: Allow
            Principal:
              Service:
                - states.amazonaws.com
      ManagedPolicyArns: []
      Policies:
        - PolicyName: StateMachineRolePolicy
          PolicyDocument:
            Statement:
              - Action:
                  - 'lambda:InvokeFunction'
                Resource: "*"
                Effect: Allow

Outputs:
  StateMachineArn:
    Value:
      Ref: MyStateMachine
```

------
#### [ YAML with DefinitionString ]

```
AWSTemplateFormatVersion: 2010-09-09
Description: AWS Step Functions sample template.
Resources:
  MyStateMachine:
    Type: 'AWS::StepFunctions::StateMachine'
    Properties:
      RoleArn: !GetAtt
        - StateMachineRole
        - Arn
      TracingConfiguration:
        Enabled: true
      DefinitionString: |
        {
            "StartAt": "HelloWorld",
            "States": {
                "HelloWorld": {
                    "Type": "Pass",
                    "End": true
                }
            }
        }
  StateMachineRole:
    Type: 'AWS::IAM::Role'
    Properties:
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Action:
              - 'sts:AssumeRole'
            Effect: Allow
            Principal:
              Service:
                - states.amazonaws.com
      ManagedPolicyArns: []
      Policies:
        - PolicyName: StateMachineRolePolicy
          PolicyDocument:
            Statement:
              - Action:
                  - 'lambda:InvokeFunction'
                Resource: "*"
                Effect: Allow

Outputs:
  StateMachineArn:
    Value:
      Ref: MyStateMachine
```

------