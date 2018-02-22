# Creating IAM Roles for AWS Step Functions<a name="procedure-create-iam-role"></a>

AWS Step Functions can execute code and access AWS resources \(such as invoking an AWS Lambda function\)\. To maintain security, you must grant Step Functions access to those resources by using an IAM role\.

The [Tutorials](tutorials.md) in this guide enable you to take advantage of automatically generated IAM roles that are valid for the region in which you create the state machine\. To create your own IAM role for a state machine, follow the steps in this section\.

## Create a Role for Step Functions<a name="lambda-create-role"></a>

In this example, you create an IAM role with permission to invoke a Lambda function\.

### To create a role for Step Functions<a name="create-role-for-step-functions"></a>

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/home), and then choose **Roles**, **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS service**, select **Step Functions** from the list and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, choose **Next: Review**\.

1. On the **Review** page, type `StepFunctionsLambdaRole` for **Role Name**, and then choose **Create role**\.

   The IAM role appears in the list of roles\.

For more information about IAM permissions and policies, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.