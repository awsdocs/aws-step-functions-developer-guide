# Creating a Lambda State Machine for Step Functions Using the AWS CDK<a name="tutorial-lambda-state-machine-cdk"></a>

This tutorial shows you how to create a AWS Step Functions state machine containing an AWS Lambda function using the AWS Cloud Development Kit \(CDK\)\. The AWS CDK is an Infrastructure as Code \(IAC\) framework that lets you define AWS infrastructure using a full\-fledged programming language\. You can write an app in one of the CDK's supported languages containing one or more stacks\. Then, you can synthesize it to an AWS CloudFormation template and deploy it to your AWS account\. We'll use this method to define a AWS Step Functions state machine containing a AWS Lambda function, then use the AWS Management Console to initiate execution\. 

Before you begin this tutorial, you must set up your AWS CDK development environment as described in [Getting Started With the AWS CDK \- Prerequisites](https://docs.aws.amazon.com/cdk/latest/guide/getting_started.html#getting_started_prerequisites) Then, install the AWS CDK with the following command at the AWS CLI:

```
npm install -g aws-cdk
```

This tutorial produces the same result as [Creating a Lambda state machine for Step Functions using AWS CloudFormation](tutorial-lambda-state-machine-cloudformation.md)\. However, in this tutorial, the AWS CDK doesn't require you to create any IAM roles; the AWS CDK does it for you\. The AWS CDK version also includes a Succeed step to illustrate how to add additional steps to your state machine\.

**Topics**
+ [Step 1: Set Up Your AWS CDK Project](#lambda-state-machine-cdk-step-1)
+ [Step 2: Use the AWS CDK to Create a Lambda State Machine](#lambda-state-machine-cdk-step-2)
+ [Step 3: Start a State Machine Execution](#lambda-state-machine-cdk-step-3)
+ [Step 4: Clean Up](#lambda-state-machine-cdk-step-4)
+ [Next steps](#lambda-state-machine-cdk-next-steps)

## Step 1: Set Up Your AWS CDK Project<a name="lambda-state-machine-cdk-step-1"></a>

First, create a directory for your new AWS CDK app and initialize the project\.

**Note**  
Be sure to name the directory `step`\. The AWS CDK application template uses the name of the directory to generate names for source files and classes\. If you use a different name, your app will not match this tutorial\.

------
#### [ TypeScript ]

```
mkdir step
cd step
cdk init --language typescript
```

------
#### [ JavaScript ]

```
mkdir step
cd step
cdk init --language javascript
```

------
#### [ Python ]

```
mkdir step
cd step
cdk init --language python
```

After the project has been initialized, activate the project's virtual environment and install the AWS CDK's baseline dependencies\.

```
source .venv/bin/activate
python -m pip install -r requirements.txt
```

------
#### [ Java ]

```
mkdir step
cd step
cdk init --language java
```

------
#### [ C\# ]

```
mkdir step
cd step
cdk init --language csharp
```

------

Next, install the construct library modules for AWS Lambda and AWS Step Functions\.

------
#### [ TypeScript ]

```
npm install @aws-cdk/aws-lambda @aws-cdk/aws-stepfunctions @aws-cdk/aws-stepfunctions-tasks
```

------
#### [ JavaScript ]

```
npm install @aws-cdk/aws-lambda @aws-cdk/aws-stepfunctions @aws-cdk/aws-stepfunctions-tasks
```

------
#### [ Python ]

```
python -m pip install aws-cdk.aws-lambda aws-cdk.aws-stepfunctions
python -m pip install aws-cdk.aws-stepfunctions-tasks
```

------
#### [ Java ]

Edit the project's `pom.xml` file to add the following dependencies inside the existing `<dependencies>` container\.

```
        <dependency>
            <groupId>software.amazon.awscdk</groupId>
            <artifactId>stepfunctions</artifactId>
            <version>${cdk.version}</version>
        </dependency>
        <dependency>
            <groupId>software.amazon.awscdk</groupId>
            <artifactId>stepfunctions-tasks</artifactId>
            <version>${cdk.version}</version>
        </dependency>
        <dependency>
            <groupId>software.amazon.awscdk</groupId>
            <artifactId>lambda</artifactId>
            <version>${cdk.version}</version>
        </dependency>
```

Maven automatically installs these dependencies the next time you build your app\. To build, issue `mvn compile` or use your Java IDE's **Build** command\.

------
#### [ C\# ]

```
dotnet add src\Step package Amazon.CDK.AWS.Lambda
dotnet add src\Step package Amazon.CDK.AWS.Stepfunctions
dotnet add src\Step package Amazon.CDK.AWS.Stepfunctions.Tasks
```

You may also install the indicated packages using the Visual Studio NuGet GUI, available via **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**\.

------

Once you've installed these modules, you can use them in your AWS CDK app by importing the following packages:

------
#### [ TypeScript ]

```
@aws-cdk/aws-lambda
@aws-cdk/aws-stepfunctions
@aws-cdk/aws-stepfunctions-tasks
```

------
#### [ JavaScript ]

```
@aws-cdk/aws-lambda
@aws-cdk/aws-stepfunctions
@aws-cdk/aws-stepfunctions-tasks
```

------
#### [ Python ]

```
aws_cdk.aws_lambda
aws_cdk.aws_stepfunctions
aws_cdk.aws_stepfunctions_tasks
```

------
#### [ Java ]

```
software.amazon.awscdk.services.lambda
software.amazon.awscdk.services.stepfunctions
software.amazon.awscdk.services.stepfunctions.tasks
```

------
#### [ C\# ]

```
Amazon.CDK.AWS.Lambda
Amazon.CDK.AWS.StepFunctions
Amazon.CDK.AWS.StepFunctions.Tasks
```

------

## Step 2: Use the AWS CDK to Create a Lambda State Machine<a name="lambda-state-machine-cdk-step-2"></a>

First, we'll present the individual pieces of code that define the Lambda function and the Step Functions state machine\. Then, we'll explain how to put them together in your AWS CDK app\. Finally, you'll see how to synthesize and deploy these resources\.

### To create a Lambda function<a name="lambda-state-machine-cdk-create-function"></a>

The following AWS CDK code defines the Lambda function, providing its source code inline\.

------
#### [ TypeScript ]

```
const helloFunction = new lambda.Function(this, 'MyLambdaFunction', {
  code: lambda.Code.fromInline(`
      exports.handler = (event, context, callback) => {
        callback(null, "Hello World!");
      };
  `),
  runtime: lambda.Runtime.NODEJS_12_X,
  handler: "index.handler",
  timeout: cdk.Duration.seconds(25)
});
```

------
#### [ JavaScript ]

```
const helloFunction = new lambda.Function(this, 'MyLambdaFunction', {
  code: lambda.Code.fromInline(`
      exports.handler = (event, context, callback) => {
        callback(null, "Hello World!");
      };
  `),
  runtime: lambda.Runtime.NODEJS_12_X,
  handler: "index.handler",
  timeout: cdk.Duration.seconds(25)
});
```

------
#### [ Python ]

```
hello_function = lambda_.Function(self, "MyLambdaFunction",
                                  code=lambda_.Code.from_inline("""
                        exports.handler = (event, context, callback) => {
                          callback(null, "Hello World!");
                        }"""),
                                  runtime=lambda_.Runtime.NODEJS_12_X,
                                  handler="index.handler",
                                  timeout=cdk.Duration.seconds(25))
```

------
#### [ Java ]

```
Function helloFunction = Function.Builder.create(this, "MyLambdaFunction")
        .code(Code.fromInline(
                "exports.handler = (event, context, callback) => { callback(null, 'Hello World!' );"))
        .runtime(Runtime.NODEJS_12_X)
        .handler("index.handler")
        .timeout(Duration.seconds(25))
        .build();
```

------
#### [ C\# ]

```
var helloFunction = new Function(this, "MyLambdaFunction", new FunctionProps
{
    Code = Code.FromInline(@"`
      exports.handler = (event, context, callback) => {
        callback(null, 'Hello World!');
      }"),
    Runtime = Runtime.NODEJS_12_X,
    Handler = "index.handler",
    Timeout = Duration.Seconds(25)
});
```

------

You can see in this short example code:
+ The function's logical name, `MyLambdaFunction`\.
+ The source code for the function, embedded as a string in the source code of the AWS CDK app\.
+ Other function attributes, such as the runtime to be used \(Node 12\.x\), the function's entry point, and a timeout\.

### To create a Lambda state machine<a name="lambda-state-machine-cdk-create"></a>

Our state machine has two states: our Lambda function task, and a success state\. The function requires that we create a Step Functions [Task](amazon-states-language-task-state.md) that invokes our function\. This task state is used as the first step in our state machine The success state is added to the state machine using that task's `next()` method\. The following code both invokes the function `MyLambdaTask`, then uses the `next()` method to set a success state of `GreetedWorld`

------
#### [ TypeScript ]

```
const stateMachine = new sfn.StateMachine(this, 'MyStateMachine', {
  definition: new tasks.LambdaInvoke(this, "MyLambdaTask", {
    lambdaFunction: helloFunction
  }).next(new sfn.Succeed(this, "GreetedWorld"))
});
```

------
#### [ JavaScript ]

```
const stateMachine = new sfn.StateMachine(this, 'MyStateMachine', {
  definition: new tasks.LambdaInvoke(this, "MyLambdaTask", {
    lambdaFunction: helloFunction
  }).next(new sfn.Succeed(this, "GreetedWorld"))
});
```

------
#### [ Python ]

```
state_machine = sfn.StateMachine(self, "MyStateMachine",
                                 definition=tasks.LambdaInvoke(self, "MyLambdaTask",
                                     lambda_function=hello_function).next(
                                         sfn.Succeed(self, "GreetedWorld")))
```

------
#### [ Java ]

```
StateMachine stateMachine = StateMachine.Builder.create(this, "MyStateMachine")
        .definition(LambdaInvoke.Builder.create(this, "MyLambdaTask")
            .lambdaFunction(helloFunction)
            .build()
            .next(new Succeed(this, "GreetedWorld")))
        .build();
```

------
#### [ C\# ]

```
var stateMachine = new StateMachine(this, "MyStateMachine", new StateMachineProps {
    Definition = new LambdaInvoke(this, "MyLambdaTask", new LambdaInvokeProps
    {
        LambdaFunction = helloFunction
    }).next(new Succeed(this, "GreetedWorld"))
});
```

------

### To build and deploy the AWS CDK app<a name="lambda-state-machine-cdk-app"></a>

In your newly created AWS CDK project, edit the file that contains the stack's definition to look like the following example code\. You'll recognize the definitions of the Lambda function and the Step Functions state machine from previous sections\.

------
#### [ TypeScript ]

Update `lib/step-stack.ts` with the following code:

```
import * as cdk from '@aws-cdk/core';

import * as lambda from '@aws-cdk/aws-lambda';
import * as sfn from '@aws-cdk/aws-stepfunctions';
import * as tasks from '@aws-cdk/aws-stepfunctions-tasks';

export class StepStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    const helloFunction = new lambda.Function(this, 'MyLambdaFunction', {
      code: lambda.Code.fromInline(`
          exports.handler = (event, context, callback) => {
              callback(null, "Hello World!");
          };
      `),
      runtime: lambda.Runtime.NODEJS_12_X,
      handler: "index.handler",
      timeout: cdk.Duration.seconds(3)
    });

    const stateMachine = new sfn.StateMachine(this, 'MyStateMachine', {
      definition: new tasks.LambdaInvoke(this, "MyLambdaTask", {
        lambdaFunction: helloFunction
      }).next(new sfn.Succeed(this, "GreetedWorld"))
    });
  }
}
```

------
#### [ JavaScript ]

Update `lib/step-stack.js` with the following code\.

```
const cdk = require('@aws-cdk/core');

const lambda = require('@aws-cdk/aws-lambda');
const sfn = require('@aws-cdk/aws-stepfunctions');
const tasks = require('@aws-cdk/aws-stepfunctions-tasks');

class StepStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    const helloFunction = new lambda.Function(this, 'MyLambdaFunction', {
      code: lambda.Code.fromInline(`
          exports.handler = (event, context, callback) => {
              callback(null, "Hello World!");
          };
      `),
      runtime: lambda.Runtime.NODEJS_12_X,
      handler: "index.handler",
      timeout: cdk.Duration.seconds(25)
    });

    const stateMachine = new sfn.StateMachine(this, 'MyStateMachine', {
      definition: new tasks.LambdaInvoke(this, "MyLambdaTask", {
        lambdaFunction: helloFunction
      }).next(new sfn.Succeed(this, "GreetedWorld"))
    });
  }
}

module.exports = { StepStack }
```

------
#### [ Python ]

Update `step/step_stack.py` with the following code\.

```
from aws_cdk import core as cdk

from aws_cdk import aws_lambda as lambda_
from aws_cdk import aws_stepfunctions as sfn
from aws_cdk import aws_stepfunctions_tasks as tasks

class StepStack(cdk.Stack):

    def __init__(self, scope: cdk.Construct, construct_id: str, **kwargs) -> None:
        super().__init__(scope, construct_id, **kwargs)

        hello_function = lambda_.Function(self, "MyLambdaFunction",
                                          code=lambda_.Code.from_inline("""
                                exports.handler = (event, context, callback) => {
                                    callback(null, "Hello World!");
                                }"""),
                                          runtime=lambda_.Runtime.NODEJS_12_X,
                                          handler="index.handler",
                                          timeout=cdk.Duration.seconds(25))

        state_machine = sfn.StateMachine(self, "MyStateMachine",
                                         definition=tasks.LambdaInvoke(self, "MyLambdaTask",
                                            lambda_function=hello_function).next(
                                            sfn.Succeed(self, "GreetedWorld")))
```

------
#### [ Java ]

Update `src/main/java/com.myorg/StepStack.java` with the following code\.

```
package com.myorg;

import software.amazon.awscdk.core.Construct;
import software.amazon.awscdk.core.Stack;
import software.amazon.awscdk.core.StackProps;
import software.amazon.awscdk.core.Duration;

import software.amazon.awscdk.services.lambda.Function;
import software.amazon.awscdk.services.lambda.Code;
import software.amazon.awscdk.services.lambda.Runtime;
import software.amazon.awscdk.services.stepfunctions.StateMachine;
import software.amazon.awscdk.services.stepfunctions.Succeed;
import software.amazon.awscdk.services.stepfunctions.tasks.LambdaInvoke;

public class StepStack extends Stack {
    public StepStack(final Construct scope, final String id) {
        this(scope, id, null);
    }

    public StepStack(final Construct scope, final String id, final StackProps props) {
        super(scope, id, props);

        Function helloFunction = Function.Builder.create(this, "MyLambdaFunction")
                .code(Code.fromInline(
                        "exports.handler = (event, context, callback) => { callback(null, 'Hello World!' );"))
                .runtime(Runtime.NODEJS_12_X)
                .handler("index.handler")
                .timeout(Duration.seconds(25))
                .build();

        StateMachine stateMachine = StateMachine.Builder.create(this, "MyStateMachine")
                .definition(LambdaInvoke.Builder.create(this, "MyLambdaTask")
                    .lambdaFunction(helloFunction)
                    .build()
                    .next(new Succeed(this, "GreetedWorld")))
                .build();
    }
}
```

------
#### [ C\# ]

Update `scr/Step/StepStack.cs` with the following code\.

```
using Amazon.CDK;
using Amazon.CDK.AWS.Lambda;
using Amazon.CDK.AWS.StepFunctions;
using Amazon.CDK.AWS.StepFunctions.Tasks;

namespace Step
{
    public class StepStack : Stack
    {
        internal StepStack(Construct scope, string id, IStackProps props = null) : base(scope, id, props)
        {
            var helloFunction = new Function(this, "MyLambdaFunction", new FunctionProps
            {
                Code = Code.FromInline(@"`
                  exports.handler = (event, context, callback) => {
                    callback(null, 'Hello World!');
                  }"),
                Runtime = Runtime.NODEJS_12_X,
                Handler = "index.handler",
                Timeout = Duration.Seconds(25)
            });

            var stateMachine = new StateMachine(this, "MyStateMachine", new StateMachineProps {
                Definition = new LambdaInvoke(this, "MyLambdaTask", new LambdaInvokeProps
                {
                    LambdaFunction = helloFunction
                }).Next(new Succeed(this, "GreetedWorld"))
            });

        }
    }
}
```

------

Save the source file\.Then, run the `cdk synth` command in the app's main directory\. The AWS CDK runs the app and synthesizes an AWS CloudFormation template from it\. The AWS CDK then displays the template\.

To actually deploy the Lambda function and the Step Functions state machine to your AWS account, issue `cdk deploy`\. You'll be asked to approve the IAM policies the AWS CDK has generated\.

## Step 3: Start a State Machine Execution<a name="lambda-state-machine-cdk-step-3"></a>

After you create your Lambda state machine, you can start an execution\.

### To start the state machine execution<a name="to-start-the-state-machine-execution"></a>

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose the name of the state machine that you created using the AWS CDK\.

1. On the ***MyStateMachine\-ABCDEFGHIJ1K*** page, choose **New execution**\.

   The **New execution** page is displayed\.

1. \(Optional\) To identify your execution, you can specify a name for it in the **Name** box\. By default, Step Functions generates a unique execution name automatically\.
**Note**  
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. Choose **Start Execution**\.

   A new execution of your state machine starts, and a new page showing your running execution is displayed\.

1. \(Optional\) In the **Execution Details**, review the **Execution Status** and the **Started** and **Closed** timestamps\.

1. To view the results of your execution, choose **Output**\.

## Step 4: Clean Up<a name="lambda-state-machine-cdk-step-4"></a>

After you've tested your state machine, we recommend that you remove both your state machine and the related Lambda function to free up resources in your AWS account\. Run the `cdk destroy` command in your app's main directory to remove your state machine\.

## Next steps<a name="lambda-state-machine-cdk-next-steps"></a>

To learn more about developing AWS infrastructure using the AWS CDK, see the [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/)\.

For information about writing AWS CDK apps in your language of choice, see:

------
#### [ TypeScript ]

[Working with the AWS CDK in TypeScript](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-typescript.html)

------
#### [ JavaScript ]

[Working with the AWS CDK in JavaScript](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-javascript.html)

------
#### [ Python ]

[Working with the AWS CDK in Python](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-python.html)

------
#### [ Java ]

[Working with the AWS CDK in Java](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-java.html)

------
#### [ C\# ]

[Working with the AWS CDK in C\#](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-csharp.html)

------

For more information on the AWS Construct Library modules used in this tutorial, see the AWS CDK API Reference overviews below\.
+ [aws\-lambda](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-lambda-readme.html)
+ [aws\-stepfunctions](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-stepfunctions-readme.html)
+ [aws\-stepfunctions\-tasks](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-stepfunctions-tasks-readme.html)