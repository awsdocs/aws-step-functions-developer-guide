# Creating a Lambda State Machine Using AWS CloudFormation<a name="tutorial-lambda-state-machine-cloudformation"></a>

This tutorial shows you how to create a basic AWS Lambda function using AWS CloudFormation\. You use the AWS CloudFormation console and a YAML *template* to create the *stack* \(IAM roles, the Lambda function, and the state machine\)\. Then you use the AWS Step Functions console to start the state machine execution\. 

For more information, see [Working with CloudFormation Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) and the `[AWS::StepFunctions::StateMachine](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-stepfunctions-statemachine.html)` resource in the *AWS CloudFormation User Guide*\.

**Topics**
+ [Step 1: Set Up Your AWS CloudFormation Template](#lambda-state-machine-cfn-step-1)
+ [Step 2: Use the AWS CloudFormation Template to Create a Lambda State Machine](#lambda-state-machine-cfn-step-2)
+ [Step 3: Start a State Machine Execution](#lambda-state-machine-cfn-step-3)

## Step 1: Set Up Your AWS CloudFormation Template<a name="lambda-state-machine-cfn-step-1"></a>

Before you use the [example templates](#lambda-state-machine-cfn-step-2), you should understand their parts\.

**Topics**
+ [To create an IAM role for Lambda](#lambda-state-machine-cfn-procedure-create-iam-role)
+ [To create a Lambda function](#lambda-state-machine-cfn-create-function)
+ [To create an IAM role for the state machine execution](#lambda-state-machine-cfn-create-role)
+ [To create a Lambda state machine](#lambda-state-machine-cfn-create)

### To create an IAM role for Lambda<a name="lambda-state-machine-cfn-procedure-create-iam-role"></a>

Define the trust policy associated with the IAM role for the Lambda function\.

------
#### [ YAML ]

```
LambdaExecutionRole:
  Type: "AWS::IAM::Role"
  Properties:
    AssumeRolePolicyDocument:
      Version: "2012-10-17"
      Statement:
        - Effect: Allow
          Principal:
            Service: lambda.amazonaws.com
          Action: "sts:AssumeRole"
```

------
#### [ JSON ]

```
          "LambdaExecutionRole": {
            "Type": "AWS::IAM::Role",
            "Properties": {
                "AssumeRolePolicyDocument": {
                    "Version": "2012-10-17",
                    "Statement": [
                        {
                            "Effect": "Allow",
                            "Principal": {
                                "Service": "lambda.amazonaws.com"
                            },
                            "Action": "sts:AssumeRole"
                        }
                    ]
                }
            }
```

------

### To create a Lambda function<a name="lambda-state-machine-cfn-create-function"></a>

Define the following properties of the Lambda function that prints the message `Hello World`\.

**Important**  
Ensure that your Lambda function is under the same AWS account and AWS Region as your state machine\.

------
#### [ YAML ]

```
MyLambdaFunction:
  Type: "AWS::Lambda::Function"
  Properties:
    Handler: "index.handler"
    Role: !GetAtt [ LambdaExecutionRole, Arn ]
    Code:
      ZipFile: |
        exports.handler = (event, context, callback) => {
            callback(null, "Hello World!");
        };
    Runtime: "nodejs8.10"
    Timeout: "25"
```

------
#### [ JSON ]

```
        "MyLambdaFunction": {
            "Type": "AWS::Lambda::Function",
            "Properties": {
                "Handler": "index.handler",
                "Role": {
                    "Fn::GetAtt": [
                        "LambdaExecutionRole",
                        "Arn"
                    ]
                },
                "Code": {
                    "ZipFile": "exports.handler = (event, context, callback) => {\n    callback(null, \"Hello World!\");\n};\n"
                },
                "Runtime": "nodejs8.10",
                "Timeout": "25"
            }
        },
```

------

### To create an IAM role for the state machine execution<a name="lambda-state-machine-cfn-create-role"></a>

Define the trust policy associated with the IAM role for the state machine execution\.

------
#### [ YAML ]

```
StatesExecutionRole:
  Type: "AWS::IAM::Role"
  Properties:
    AssumeRolePolicyDocument:
      Version: "2012-10-17"
      Statement:
        - Effect: "Allow"
          Principal:
            Service:
              - !Sub states.${AWS::Region}.amazonaws.com
          Action: "sts:AssumeRole"
    Path: "/"
    Policies:
      - PolicyName: StatesExecutionPolicy
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Action:
                - "lambda:InvokeFunction"
              Resource: "*"
```

------
#### [ JSON ]

```
        "StatesExecutionRole": {
            "Type": "AWS::IAM::Role",
            "Properties": {
                "AssumeRolePolicyDocument": {
                    "Version": "2012-10-17",
                    "Statement": [
                        {
                            "Effect": "Allow",
                            "Principal": {
                                "Service": [
                                    {
                                        "Fn::Sub": "states.${AWS::Region}.amazonaws.com"
                                    }
                                ]
                            },
                            "Action": "sts:AssumeRole"
                        }
                    ]
                },
                "Path": "/",
                "Policies": [
                    {
                        "PolicyName": "StatesExecutionPolicy",
                        "PolicyDocument": {
                            "Version": "2012-10-17",
                            "Statement": [
                                {
                                    "Effect": "Allow",
                                    "Action": [
                                        "lambda:InvokeFunction"
                                    ],
                                    "Resource": "*"
                                }
                            ]
                        }
                    }
                ]
            }
        },
```

------

### To create a Lambda state machine<a name="lambda-state-machine-cfn-create"></a>

Define the Lambda state machine\.

------
#### [ YAML ]

```
MyStateMachine:
  Type: "AWS::StepFunctions::StateMachine"
  Properties:
    DefinitionString:
      !Sub
        - |-
          {
            "Comment": "A Hello World example using an AWS Lambda function",
            "StartAt": "HelloWorld",
            "States": {
              "HelloWorld": {
                "Type": "Task",
                "Resource": "${lambdaArn}",
                "End": true
              }
            }
          }
        - {lambdaArn: !GetAtt [ MyLambdaFunction, Arn ]}
    RoleArn: !GetAtt [ StatesExecutionRole, Arn ]
```

------
#### [ JSON ]

```
        "MyStateMachine": {
            "Type": "AWS::StepFunctions::StateMachine",
            "Properties": {
                "DefinitionString": {
                    "Fn::Sub": [
                        "{\n  \"Comment\": \"A Hello World example using an AWS Lambda function\",\n  \"StartAt\": \"HelloWorld\",\n  \"States\": {\n    \"HelloWorld\": {\n      \"Type\": \"Task\",\n      \"Resource\": \"${lambdaArn}\",\n      \"End\": true\n    }\n  }\n}",
                        {
                            "lambdaArn": {
                                "Fn::GetAtt": [
                                    "MyLambdaFunction",
                                    "Arn"
                                ]
                            }
                        }
                    ]
                },
                "RoleArn": {
                    "Fn::GetAtt": [
                        "StatesExecutionRole",
                        "Arn"
                    ]
                }
            }
        }
```

------

## Step 2: Use the AWS CloudFormation Template to Create a Lambda State Machine<a name="lambda-state-machine-cfn-step-2"></a>

After you understand the parts of the AWS CloudFormation template, you put them together and use the template to create an AWS CloudFormation stack\.

### To create the Lambda state machine<a name="to-create-the-lam-state-machine"></a>

1. Copy the following example data to a file named `MyStateMachine.yaml` for the YAML example, or `MyStateMachine.json` for JSON\.

------
#### [ YAML ]

   ```
   AWSTemplateFormatVersion: "2010-09-09"
   Description: "An example template with an IAM role for a Lambda state machine."
   Resources:
     LambdaExecutionRole:
       Type: "AWS::IAM::Role"
       Properties:
         AssumeRolePolicyDocument:
           Version: "2012-10-17"
           Statement:
             - Effect: Allow
               Principal:
                 Service: lambda.amazonaws.com
               Action: "sts:AssumeRole"
   
     MyLambdaFunction:
       Type: "AWS::Lambda::Function"
       Properties:
         Handler: "index.handler"
         Role: !GetAtt [ LambdaExecutionRole, Arn ]
         Code:
           ZipFile: |
             exports.handler = (event, context, callback) => {
                 callback(null, "Hello World!");
             };
         Runtime: "nodejs8.10"
         Timeout: "25"
   
     StatesExecutionRole:
       Type: "AWS::IAM::Role"
       Properties:
         AssumeRolePolicyDocument:
           Version: "2012-10-17"
           Statement:
             - Effect: "Allow"
               Principal:
                 Service:
                   - !Sub states.${AWS::Region}.amazonaws.com
               Action: "sts:AssumeRole"
         Path: "/"
         Policies:
           - PolicyName: StatesExecutionPolicy
             PolicyDocument:
               Version: "2012-10-17"
               Statement:
                 - Effect: Allow
                   Action:
                     - "lambda:InvokeFunction"
                   Resource: "*"
   
     MyStateMachine:
       Type: "AWS::StepFunctions::StateMachine"
       Properties:
         DefinitionString:
           !Sub
             - |-
               {
                 "Comment": "A Hello World example using an AWS Lambda function",
                 "StartAt": "HelloWorld",
                 "States": {
                   "HelloWorld": {
                     "Type": "Task",
                     "Resource": "${lambdaArn}",
                     "End": true
                   }
                 }
               }
             - {lambdaArn: !GetAtt [ MyLambdaFunction, Arn ]}
         RoleArn: !GetAtt [ StatesExecutionRole, Arn ]
   ```

------
#### [ JSON ]

   ```
   {
       "AWSTemplateFormatVersion": "2010-09-09",
       "Description": "An example template with an IAM role for a Lambda state machine.",
       "Resources": {
           "LambdaExecutionRole": {
               "Type": "AWS::IAM::Role",
               "Properties": {
                   "AssumeRolePolicyDocument": {
                       "Version": "2012-10-17",
                       "Statement": [
                           {
                               "Effect": "Allow",
                               "Principal": {
                                   "Service": "lambda.amazonaws.com"
                               },
                               "Action": "sts:AssumeRole"
                           }
                       ]
                   }
               }
           },
           "MyLambdaFunction": {
               "Type": "AWS::Lambda::Function",
               "Properties": {
                   "Handler": "index.handler",
                   "Role": {
                       "Fn::GetAtt": [
                           "LambdaExecutionRole",
                           "Arn"
                       ]
                   },
                   "Code": {
                       "ZipFile": "exports.handler = (event, context, callback) => {\n    callback(null, \"Hello World!\");\n};\n"
                   },
                   "Runtime": "nodejs8.10",
                   "Timeout": "25"
               }
           },
           "StatesExecutionRole": {
               "Type": "AWS::IAM::Role",
               "Properties": {
                   "AssumeRolePolicyDocument": {
                       "Version": "2012-10-17",
                       "Statement": [
                           {
                               "Effect": "Allow",
                               "Principal": {
                                   "Service": [
                                       {
                                           "Fn::Sub": "states.${AWS::Region}.amazonaws.com"
                                       }
                                   ]
                               },
                               "Action": "sts:AssumeRole"
                           }
                       ]
                   },
                   "Path": "/",
                   "Policies": [
                       {
                           "PolicyName": "StatesExecutionPolicy",
                           "PolicyDocument": {
                               "Version": "2012-10-17",
                               "Statement": [
                                   {
                                       "Effect": "Allow",
                                       "Action": [
                                           "lambda:InvokeFunction"
                                       ],
                                       "Resource": "*"
                                   }
                               ]
                           }
                       }
                   ]
               }
           },
           "MyStateMachine": {
               "Type": "AWS::StepFunctions::StateMachine",
               "Properties": {
                   "DefinitionString": {
                       "Fn::Sub": [
                           "{\n  \"Comment\": \"A Hello World example using an AWS Lambda function\",\n  \"StartAt\": \"HelloWorld\",\n  \"States\": {\n    \"HelloWorld\": {\n      \"Type\": \"Task\",\n      \"Resource\": \"${lambdaArn}\",\n      \"End\": true\n    }\n  }\n}",
                           {
                               "lambdaArn": {
                                   "Fn::GetAtt": [
                                       "MyLambdaFunction",
                                       "Arn"
                                   ]
                               }
                           }
                       ]
                   },
                   "RoleArn": {
                       "Fn::GetAtt": [
                           "StatesExecutionRole",
                           "Arn"
                       ]
                   }
               }
           }
       }
   }
   ```

------

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home) and choose **Create Stack**\.

1. On the **Select Template** page, select **Upload a template to Amazon S3**\. Choose your `MyStateMachine` file, and then choose **Next**\.

1. On the **Specify Details** page, for **Stack name**, enter `MyStateMachine`, and then choose **Next**\.

1. On the **Options** page, choose **Next**\.

1. On the **Review** page, choose **I acknowledge that AWS CloudFormation might create IAM resources\.** and then choose **Create**\.

   AWS CloudFormation begins to create the `MyStateMachine` stack and displays the **CREATE\_IN\_PROGRESS** status\. When the process is complete, AWS CloudFormation displays the **CREATE\_COMPLETE** status\.

1. \(Optional\) To display the resources in your stack, select the stack and choose the **Resources** tab\.  
![\[Display resources\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-lambda-state-machine-display-resources.png)![\[Display resources\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Display resources\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

## Step 3: Start a State Machine Execution<a name="lambda-state-machine-cfn-step-3"></a>

After you create your Lambda state machine, you start an execution\.

### To start the state machine execution<a name="to-start-the-state-machine-execution"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose the name of the state machine that you created using AWS CloudFormation\.

1. On the ***MyStateMachine\-ABCDEFGHIJ1K*** page, choose **New execution**\.

   The **New execution** page is displayed\.

1. \(Optional\) To help identify your execution, you can specify an ID for it in the **Enter an execution name** box\. If you don't enter an ID, Step Functions generates a unique ID automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. \(Optional\) In the **Execution Details**, review the **Execution Status** and the **Started** and **Closed** timestamps\.

1. To view the results of your execution, choose **Output**\.