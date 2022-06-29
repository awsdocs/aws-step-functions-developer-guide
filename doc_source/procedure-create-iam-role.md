# How AWS Step Functions Works with IAM<a name="procedure-create-iam-role"></a>

AWS Step Functions can execute code and access AWS resources \(such as invoking an AWS Lambda function\)\. To maintain security, you must grant Step Functions access to those resources by using an IAM role\.

The [Tutorials for Step Functions](tutorials.md) in this guide enable you to take advantage of automatically generated IAM roles that are valid for the AWS Region in which you create the state machine\. However, you can create your own IAM role for a state machine\.

When creating an IAM policy for your state machines to use, the policy should include the permissions that you would like the state machines to assume\. You can use an existing AWS managed policy that as an example or you can create a custom policy from scratch that meets your specific needs\. For more information, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*

To create your own IAM role for a state machine, follow the steps in this section\.

In this example, you create an IAM role with permission to invoke a Lambda function\.

## Create a role for Step Functions<a name="create-role-for-step-functions"></a>

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/home), and then choose **Roles**, **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS service**, select **Step Functions** from the list, and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, choose **Next: Review**\.

1. On the **Review** page, enter `StepFunctionsLambdaRole` for **Role Name**, and then choose **Create role**\.

   The IAM role appears in the list of roles\.

For more information about IAM permissions and policies, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

## Prevent cross\-service confused deputy issue<a name="prevent-cross-service-confused-deputy"></a>

A confused deputy is an entity \(a service or an account\) that is coerced by a different entity to perform an action\. In AWS, cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources in a way it should not otherwise have permission to access\. This type of impersonation can happen cross\-account and cross\-service\.

To prevent confused deputies, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. This section focuses on cross\-service confused deputy prevention specific to AWS Step Functions; however, you can learn more about this topic in the [confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html) section of the *IAM User Guide*\.

To limit the permissions IAM gives to Step Functions to access your resources, we recommend using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) global condition context keys in your resource policies\. Note that if you use both of these global condition context keys, and the `aws:SourceArn` value contains the AWS account ID, the `aws:SourceAccount` value and the AWS account in the `aws:SourceArn` value must use the same AWS account ID when used in the same policy statement\.

**Note**  
The most effective way to protect against the confused deputy problem is to use the `aws:SourceArn` global condition context key with the full ARN of the resource\. If you don’t know the full ARN, or if you're specifying multiple resources, use the `aws:SourceArn` global context condition key with wildcards \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:states:*:111122223333:*`\.

Here's an example of a *trusted policy* that shows how you can use `aws:SourceArn` and `aws:SourceAccount` with Step Functions to prevent the confused deputy issue\.

```
{
  "Version":"2012-10-17",
  "Statement":[
     {
        "Effect":"Allow",
        "Principal":{
           "Service":[
              "states.amazonaws.com"
           ]
        },
        "Action":"sts:AssumeRole",
        "Condition":{
           "ArnLike":{
              "aws:SourceArn":"arn:aws:states:us-east-1:111122223333:stateMachine:*"
           },
           "StringEquals":{
              "aws:SourceAccount":"111122223333"
           }
        }
     }
  ]
}
```

## Attach an Inline Policy<a name="attach-inline-policy"></a>

Step Functions can control other services directly in a `Task` state\. Attach inline policies to allow Step Functions to access the API actions of the services you need to control\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/home), choose **Roles**, search for your Step Functions role, and select that role\.

1. Select **Add inline policy**\.

1. Use the **Visual editor** or the **JSON** tab to create policies for your role\.

For more information about how AWS Step Functions can control other AWS services, see [Using AWS Step Functions with other services](concepts-service-integrations.md)\.

**Note**  
For examples of IAM policies created by the Step Functions console, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.