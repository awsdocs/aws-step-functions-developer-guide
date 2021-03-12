# Amazon VPC Endpoints for Step Functions<a name="vpc-endpoints"></a>

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a connection between your Amazon VPC and AWS Step Functions workflows\. You can use this connection with your Step Functions workflows without crossing the public internet\.

Amazon VPC lets you launch AWS resources in a custom virtual network\. You can use a VPC to control your network settings, such as the IP address range, subnets, route tables, and network gateways\. For more information about VPCs, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

To connect your Amazon VPC to Step Functions, you must first define an *interface VPC endpoint*, which lets you connect your VPC to other AWS services\. The endpoint provides reliable, scalable connectivity, without requiring an internet gateway, network address translation \(NAT\) instance, or VPN connection\. For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.

## Creating the Endpoint<a name="vpc-endpoint-create"></a>

You can create an AWS Step Functions endpoint in your VPC using the AWS Management Console, the AWS Command Line Interface \(AWS CLI\), an AWS SDK, the AWS Step Functions API, or AWS CloudFormation\.

For information about creating and configuring an endpoint using the Amazon VPC console or the AWS CLI, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide\.*

**Note**  
 When you create an endpoint, specify Step Functions as the service that you want your VPC to connect to\. In the Amazon VPC console, service names vary based on the AWS Region\. For example, if you choose US East \(N\. Virginia\), the service name is **com\.amazonaws\.us\-east\-1\.states**\. 

For information about creating and configuring an endpoint using AWS CloudFormation, see the [AWS::EC2::VPCEndpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpcendpoint.html) resource in the *AWS CloudFormation User Guide*\.

## Amazon VPC Endpoint Policies<a name="vpc-endpoint-policy"></a>

To control connectivity access to Step Functions you can attach an AWS Identity and Access Management \(IAM\) endpoint policy while creating an Amazon VPC endpoint\. You can create complex IAM rules by attaching multiple endpoint policies\. For more information, see:
+  [Amazon Virtual Private Cloud Endpoint Policies for Step Functions](vpc-iam.md) 
+  [Creating Granular IAM Permissions for Non\-Admin Users](concept-create-iam-advanced.md) 
+  [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) 