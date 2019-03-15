# Creating a Step Functions API Using API Gateway<a name="tutorial-api-gateway"></a>

You can use Amazon API Gateway to associate your AWS Step Functions APIs with methods in an API Gateway API\. When an HTTPS request is sent to an API method, API Gateway invokes your Step Functions API actions\.

This tutorial shows you how to create an API that uses one resource and the `POST` method to communicate with the `StartExecution` API action\. You'll use the IAM console to create a role for API Gateway\. Then, you'll use the API Gateway console to create an API Gateway API, create a resource and method, and map the method to the `StartExecution` API action\. Finally, you'll deploy and test your API\. For more information about this API action, see [StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) in the *AWS Step Functions API Reference*\.

**Note**  
Although Amazon API Gateway can start a Step Functions execution by calling `[StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html)`, you must call `[DescribeExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeExecution.html)` to get the result\.

**Topics**
+ [Step 1: Create an IAM Role for API Gateway](#api-gateway-step-1)
+ [Step 2: Create your API Gateway API](#api-gateway-step-2)
+ [Step 3: Test and Deploy the API Gateway API](#api-gateway-step-3)

## Step 1: Create an IAM Role for API Gateway<a name="api-gateway-step-1"></a>

Before you create your API Gateway API, you need to give API Gateway permission to call Step Functions API actions\.

### <a name="api-gateway-procedure-create-iam-role"></a>

1. Log in to the [IAM console](https://console.aws.amazon.com/iam/home) and choose **Roles**, **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS service**, select **API Gateway** from the list and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, choose **Next: Review**\.

1. On the **Review** page, enter `APIGatewayToStepFunctions` for **Role name** and then choose **Create role**\.

   The IAM role appears in the list of roles\.

1. Choose the name of your role and note the **Role ARN**, for example:

   ```
   arn:aws:iam::123456789012:role/APIGatewayToStepFunctions
   ```

### Attach a policy to the IAM role<a name="api-gateway-attach-policy-to-role"></a>

1. On the **Roles** page, search for your role \(`APIGatewayToStepFunctions`\) and then choose the role\.

1. On the **Permissions** tab, choose **Attach Policy**\.

1. On the **Attach Policy** page, search for `AWSStepFunctionsFullAccess`, choose the policy, and then choose **Attach Policy**\.

## Step 2: Create your API Gateway API<a name="api-gateway-step-2"></a>

After you create your IAM role, you can create your custom API Gateway API\.

### Create the API<a name="api-gateway-create-api"></a>

1. Navigate to the [Amazon API Gateway console](https://console.aws.amazon.com/apigateway/) and choose **Get Started**\.

1. On the **Create new API** page, choose **New API**\.

1. In the **Settings** section, enter `StartExecutionAPI` for the **API name**, and then choose **Create API**\.

### Create a resource<a name="api-gateway-create-resource"></a>

1. On the **Resources** page of ***StartExecutionAPI***, choose **Actions**, **Create Resource**\.

1. On the **New Child Resource** page, enter `execution` for **Resource Name**, and then choose **Create Resource**\.

### Create a POST Method<a name="api-gateway-create-method"></a>

1. On the **/execution Methods** page, choose **Actions**, **Create Method**\.

1. From the list, choose `POST`, and then select the check mark\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-api-gateway-create-method.png)

### Configure the method<a name="api-gateway-configure-method"></a>

On the **/execution \- POST \- Setup** page, configure the integration point for your method\.

1. For **Integration Type**, choose **AWS Service**\.

1. For **AWS Region**, choose a Region from the list\.
**Note**  
For Regions that currently support Step Functions, see [Supported Regions](welcome.md#supported-regions)\.

1. For **AWS Service**, choose **Step Functions** from the list\.

1. For **HTTP Method**, choose **POST** from the list\.
**Note**  
All Step Functions API actions use the HTTP `POST` method\.

1. For **Action Type**, choose **Use action name**\.

1. For **Action**, enter `StartExecution`\.

1. For **Execution Role**, enter [the role ARN of the IAM role that you created earlier](#api-gateway-procedure-create-iam-role), for example:

   ```
   arn:aws:iam::123456789012:role/APIGatewayToStepFunctions
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-api-gateway-configure-method.png)

1. Choose **Save**\.

   The visual mapping between API Gateway and Step Functions is displayed on the **/execution \- POST \- Method Execution** page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-api-gateway-mapping.png)

## Step 3: Test and Deploy the API Gateway API<a name="api-gateway-step-3"></a>

Once you have created the API, test and deploy it\. 

### Test the communication between API Gateway and Step Functions<a name="api-gateway-test-api"></a>

1. On the **/execution \- POST \- Method Execution** page, choose **Test**\.

1. On the **/execution \- POST \- Method Test** page, copy the following request parameters into the **Request Body** section using the ARN of an existing state machine \(or [create a new state machine](getting-started.md)\), and then choose **Test**\.

   ```
   {
      "input": "{}",
      "name": "MyExecution",
      "stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld"
   }
   ```
**Note**  
For more information, see the `StartExecution` [Request Syntax](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html#API_StartExecution_RequestSyntax) in the *AWS Step Functions API Reference*\.  
If you don't want to include the ARN of your state machine in the body of your API Gateway call, you can configure a body\-mapping template, for example:  

   ```
   {
       "input": "$util.escapeJavaScript($input.json('$'))",
       "stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld"
   }
   ```
This approach enables you to have different state machines based on your development stages \(for example, `dev`, `test`, and `prod`\)\. To release an update, you need to change only the stage variable, for example:  

   ```
   {
       "input": "$util.escapeJavaScript($input.json('$'))",
       "stateMachineArn": "$util.escapeJavaScript($stageVariables.get(arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld))"
   }
   ```

1. The execution starts and the execution ARN and its epoch date are displayed under **Response Body**\.

   ```
   {
      "executionArn": "arn:aws:states:us-east-1:123456789012:execution:HelloWorld:MyExecution",
      "startDate": 1486768956.878
   }
   ```
**Note**  
You can view the execution by choosing your state machine on the [AWS Step Functions console](https://console.aws.amazon.com/states/)\.

### Deploy your API<a name="api-gateway-deploy-api"></a>

1. On the **Resources** page of ***StartExecutionAPI***, choose **Actions**, **Deploy API**\.

1. In the **Deploy API** dialog box, select **\[New Stage\]** from the **Deployment stage** list, enter `alpha` for **Stage name**, and then choose **Deploy**\.

### To test your deployment<a name="api-gateway-testing-deployment"></a>

1. On the **Stages** page of ***StartExecutionAPI***, expand **alpha**, **/**, **/execution**, **POST**\.

1. On the **alpha \- POST \- /execution** page, note the **Invoke URL**, for example:

   ```
   https://a1b2c3d4e5.execute-api.us-east-1.amazonaws.com/alpha/execution
   ```

1. From the command line, run the `curl` command using the ARN of your state machine, and then invoke the URL of your deployment, for example:

   ```
   curl -X POST -d '{"input": "{}","name": "MyExecution","stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld"}' https://a1b2c3d4e5.execute-api.us-east-1.amazonaws.com/alpha/execution
   ```

   The execution ARN and its epoch date are returned, for example:

   ```
   {"executionArn":"arn:aws:states:us-east-1:123456789012:execution:HelloWorld:MyExecution","startDate":1.486772644911E9}
   ```