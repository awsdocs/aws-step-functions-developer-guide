# Prerequisites for Getting Started with AWS Step Functions<a name="sfn-prerequisites"></a>

Before you get started with AWS Step Functions for the first time, complete the prerequisites that are listed on this page\.

**Topics**
+ [Create an Amazon Web Services Account](#sfn-creating-aws-account)
+ [Create an IAM User](#sfn-creating-iam-user)
+ [Create an access key for your IAM user](#sfn-getting-access-key-id-secret-access-key)

## Create an AWS account<a name="sfn-creating-aws-account"></a>

When you sign up with AWS, you gain access to all of the services that AWS offers, including Step Functions\. You 're only charged for the products and services that you use, and you can get started with AWS for free\. For more information, see [AWS Free Tier](https://aws.amazon.com/free/)\.

If you already have an AWS account, skip to the next prerequisite\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Create an IAM user and group<a name="sfn-creating-iam-user"></a>

 To access any AWS service, you must provide credentials\. These credentials determine whether you have the correct permissions to use the resources that an AWS service contains\.

When you first create an Amazon Web Services \(AWS\) account, you begin with a single sign\-in identity\. That identity has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user*\. When you sign in, enter the email address and password that you used to create the account\.

**Important**  
We strongly recommend that you do not use the root user for your everyday tasks, even the administrative ones\. Instead, adhere to the [best practice of using the root user only to create your first IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users)\. Then securely lock away the root user credentials and use them to perform only a few account and service management tasks\. To view the tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

For more information about root user and IAM user credentials, see [AWS Account Root User Credentials and IAM User Credentials\.](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html)\.

**To create an IAM user with administrator permissions**

1.  With your root user credentials, sign in to [IAM console](https://console.aws.amazon.com/iam/) and create an IAM user with administrator permissions\. For instructions, see [Creating Your First IAM User and Administrators Group in the IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\.

1.  Using your new IAM user credentials, sign in to the AWS Management Console\. For more information, see [How Users Sign in to Your Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_how-users-sign-in.html)\.

For more information about IAM, see the following:
+ [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/)
+ [Getting Started](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
+ [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)

## Create an access key for your IAM user<a name="sfn-getting-access-key-id-secret-access-key"></a>

An access key is made up of two parts, an *access key ID* and a *secret access key*\. Both keys sign programmatic requests over AWS\. Similar to a username and password, both keys are used together\.

**To create an access key for your IAM user**
+ Sign in to the [IAM console](https://console.aws.amazon.com/iam/), and create an access key for your IAM user with administrator permissions\. For instructions, see [Programmatic Access](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)\.
**Important**  
Protect your AWS account\. Never send or share your credentials with anyone outside of your organization\. No one who legitimately represents Amazon will ever ask you for your credentials\.