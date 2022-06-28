# AWS SDK service integrations<a name="supported-services-awssdk"></a>

AWS Step Functions integrates with AWS services, letting you call each service's API actions from your workflow\. You can use Step Functions' [AWS SDK integrations](#supported-services-awssdk) to call almost any AWS service's API actions from your state machine\. You can also use [Step Functions's Optimized integrations](connect-supported-services.md), each of which has been customized to provide special functionality for your workflow\.

Some services or SDKs may not be available as AWS SDK integrations, either temporarily or permanently\. Recently released services may not have SDK interactions available until a later update\. Some services require customized configuration, such as specifying a customer\-specific endpoint, which may be more suitable for an optimized integration\. Other SDKs are unsuitable for use in a workflow, such as those for streaming audio or video\. Finally, some services may be withheld until they pass certain internal validations performed by Step Functions\.

**Topics**
+ [Using AWS SDK service integrations](#use-awssdk-integ)
+ [Supported services](#supported-services-awssdk-list)
+ [Unsupported API actions for supported services](#unsupported-api-actions-list)
+ [Deprecated AWS SDK service integrations](#deprecated-aws-sdk-integ)

## Using AWS SDK service integrations<a name="use-awssdk-integ"></a>

To use AWS SDK integrations, you specify the service name and API call and, optionally, a [service integration pattern](connect-to-resource.md)\.

**Note**  
The API action will always be camel case, and parameter names will be Pascal case\. For example, you could use Step Functions' `startSyncExecution` API action and specify the parameter `StateMachineArn`\.

You can call AWS SDK services directly from the Amazon States Language in the `Resource` field of a task state\. To do this, use the following syntax:

`arn:aws:states:::aws-sdk:serviceName:apiAction.[serviceIntegrationPattern]`

For example, for Amazon EC2, you might use `arn:aws:states:::aws-sdk:ec2:describeInstances`\. This would return output as defined for the [Amazon EC2 describeInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html#API_DescribeInstances_ResponseElements) API call\. 

If an AWS SDK integration encounters an error, the resulting Error field will be composed of the service name and the error name, separated by a period character: `ServiceName.ErrorName`\. Both the service name and error name are in Pascal case\. You can also see the service name, in lowercase, in the Task state's Resource field\. You can find the potential error names in the target service's API reference documentation\. 

For example, you might use the `arn:aws:states:::aws-sdk:acmpca:deleteCertificateAuthority` AWS SDK integration\. The [AWS Certificate Manager Private Certificate Authority API Reference](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_DeleteCertificateAuthority.html#API_DeleteCertificateAuthority_Errors) indicates that the `DeleteCertificateAuthority` API action can result in a `ResourceNotFoundException`, for example\. To handle this error, you would thus specify the Error `AcmPca.ResourceNotFoundException` in your Task state's Retriers or Catchers\. For more information on error handling in Step Functions, see [Error handling in Step Functions](concepts-error-handling.md)\. 

Step Functions can't autogenerate IAM policies for AWS SDK integrations\. After you create your state machine, you will need to navigate to the IAM console and configure your role policies\. See [IAM Policies for integrated services](service-integration-iam-templates.md) for more information\.

See the [Gather Amazon S3 bucket info using AWS SDK service integrations](tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations.md) tutorial for an example of how to use AWS SDK integrations\.

## Supported AWS SDK service integrations<a name="supported-services-awssdk-list"></a>

The following table lists the AWS SDK service integrations supported by Step Functions\. The *`Task` state resource* column lists the syntax to call a specific API action when using the itegration for the service specified in the *Service name* column on the left\. The *Date supported* column provides information about the dates on which the API actions were supported for each of the services\. In addition, the *Exception prefix* column on the right, lists the exception prefixes for each service integration\. These exception prefixes are present in the exceptions that are generated when you erroneously perform an AWS SDK service integration with Step Functions\.


| Service name | `Task` state resource | Date supported | Exception prefix | 
| --- | --- | --- | --- | 
| AWS Account Management | arn:aws:states:::aws\-sdk:account:\[apiAction\] | April 19, 2022 | Account | 
| AWS Amplify | arn:aws:states:::aws\-sdk:amplify:\[apiAction\] | September 30, 2021 | Amplify | 
| AWS App Mesh | arn:aws:states:::aws\-sdk:appmesh:\[apiAction\] | September 30, 2021 | AppMesh | 
| AWS AppConfig | arn:aws:states:::aws\-sdk:appconfig:\[apiAction\] | September 30, 2021 | AppConfig | 
| AWS AppConfig Data |  arn:aws:states:::aws\-sdk:appconfigdata:\[apiAction\] | April 19, 2022 | AppConfigData | 
| AWS AppSync | arn:aws:states:::aws\-sdk:appsync:\[apiAction\] | September 30, 2021 | AppSync | 
|  AWS Application Discovery Service | arn:aws:states:::aws\-sdk:applicationdiscovery:\[apiAction\] [1](#UnsupportedFootnote1)  | September 30, 2021 | ApplicationDiscovery | 
| AWS Audit Manager | arn:aws:states:::aws\-sdk:auditmanager:\[apiAction\] | September 30, 2021 | AuditManager | 
| AWS Auto Scaling Plans | arn:aws:states:::aws\-sdk:autoscalingplans:\[apiAction\] | September 30, 2021 | AutoScalingPlans | 
| AWS Backup | arn:aws:states:::aws\-sdk:backup:\[apiAction\] | September 30, 2021 | Backup | 
| AWS Backup gateway | arn:aws:states:::aws\-sdk:backupgateway:\[apiAction\] | April 19, 2022 | BackupGateway | 
| AWS Batch | arn:aws:states:::aws\-sdk:batch:\[apiAction\] | September 30, 2021 | Batch | 
| AWS Budgets | arn:aws:states:::aws\-sdk:budgets:\[apiAction\] | September 30, 2021 | Budgets | 
| AWS Certificate Manager | arn:aws:states:::aws\-sdk:acm:\[apiAction\] | September 30, 2021 | Acm | 
| AWS Certificate Manager Private Certificate Authority | arn:aws:states:::aws\-sdk:acmpca:\[apiAction\] | September 30, 2021 | AcmPca | 
| AWS Cloud Map | arn:aws:states:::aws\-sdk:servicediscovery:\[apiAction\] | September 30, 2021 | ServiceDiscovery | 
| AWS Cloud9 | arn:aws:states:::aws\-sdk:cloud9:\[apiAction\] | September 30, 2021 | Cloud9 | 
| AWS CloudFormation | arn:aws:states:::aws\-sdk:cloudformation:\[apiAction\] | September 30, 2021 | CloudFormation | 
| AWS CloudHSM | arn:aws:states:::aws\-sdk:cloudhsm:\[apiAction\] | September 30, 2021 | CloudHsm | 
| AWS CloudHSM | arn:aws:states:::aws\-sdk:cloudhsmv2:\[apiAction\] | September 30, 2021 | CloudHsmV2 | 
| AWS CloudTrail | arn:aws:states:::aws\-sdk:cloudtrail:\[apiAction\] | September 30, 2021 | CloudTrail | 
| AWS Cloud Control |  arn:aws:states:::aws\-sdk:cloudcontrol:\[apiAction\]  | April 19, 2022 | CloudControl | 
| AWS CodeBuild | arn:aws:states:::aws\-sdk:codebuild:\[apiAction\] | September 30, 2021 | CodeBuild | 
| AWS CodeCommit | arn:aws:states:::aws\-sdk:codecommit:\[apiAction\] | September 30, 2021 | CodeCommit | 
|  AWS CodeDeploy | arn:aws:states:::aws\-sdk:codedeploy:\[apiAction\] [2](#UnsupportedFootnote2) | September 30, 2021 | CodeDeploy | 
| AWS CodePipeline | arn:aws:states:::aws\-sdk:codepipeline:\[apiAction\] | September 30, 2021 | CodePipeline | 
| AWS CodeStar | arn:aws:states:::aws\-sdk:codestar:\[apiAction\] | September 30, 2021 | CodeStar | 
| AWS CodeStar | arn:aws:states:::aws\-sdk:codestarnotifications:\[apiAction\] | September 30, 2021 | CodestarNotifications | 
| AWS CodeStar | arn:aws:states:::aws\-sdk:codestarconnections:\[apiAction\] | September 30, 2021 | CodeStarConnections | 
| AWS Compute Optimizer | arn:aws:states:::aws\-sdk:computeoptimizer:\[apiAction\] | September 30, 2021 | ComputeOptimizer | 
| AWS Config | arn:aws:states:::aws\-sdk:config:\[apiAction\] | September 30, 2021 | Config | 
| AWS Cost Explorer Service | arn:aws:states:::aws\-sdk:costexplorer:\[apiAction\] | September 30, 2021 | CostExplorer | 
| AWS Cost and Usage Report | arn:aws:states:::aws\-sdk:costandusagereport:\[apiAction\] | September 30, 2021 | CostAndUsageReport | 
| AWS Data Exchange | arn:aws:states:::aws\-sdk:dataexchange:\[apiAction\] | September 30, 2021 | DataExchange | 
| AWS Data Pipeline | arn:aws:states:::aws\-sdk:datapipeline:\[apiAction\] | September 30, 2021 | DataPipeline | 
| AWS DataSync | arn:aws:states:::aws\-sdk:datasync:\[apiAction\] | September 30, 2021 | DataSync | 
| AWS Database Migration Service | arn:aws:states:::aws\-sdk:databasemigration:\[apiAction\] | September 30, 2021 | DatabaseMigration | 
| AWS Device Farm | arn:aws:states:::aws\-sdk:devicefarm:\[apiAction\] | September 30, 2021 | DeviceFarm | 
|  AWS Direct Connect | arn:aws:states:::aws\-sdk:directconnect:\[apiAction\] [3](#UnsupportedFootnote3) | September 30, 2021 | DirectConnect | 
| AWS Directory Service | arn:aws:states:::aws\-sdk:directory:\[apiAction\] | September 30, 2021 | Directory | 
| AWS EC2 Instance Connect | arn:aws:states:::aws\-sdk:ec2instanceconnect:\[apiAction\] | September 30, 2021 | Ec2InstanceConnect | 
| AWS Elastic Beanstalk | arn:aws:states:::aws\-sdk:elasticbeanstalk:\[apiAction\] | September 30, 2021 | ElasticBeanstalk | 
| AWS Elemental MediaLive | arn:aws:states:::aws\-sdk:medialive:\[apiAction\] | September 30, 2021 | MediaLive | 
| AWS Elemental MediaPackage | arn:aws:states:::aws\-sdk:mediapackage:\[apiAction\] [4](#UnsupportedFootnote4) | September 30, 2021 | MediaPackage | 
| AWS Elemental MediaPackage VOD | arn:aws:states:::aws\-sdk:mediapackagevod:\[apiAction\] | September 30, 2021 | MediaPackageVod | 
| AWS Elemental MediaStore | arn:aws:states:::aws\-sdk:mediastore:\[apiAction\] | September 30, 2021 | MediaStore | 
| AWS Fault Injection Simulator | arn:aws:states:::aws\-sdk:fis:\[apiAction\] | September 30, 2021 | Fis | 
| AWS Firewall Manager | arn:aws:states:::aws\-sdk:fms:\[apiAction\] | September 30, 2021 | Fms | 
| AWS Glue | arn:aws:states:::aws\-sdk:glue:\[apiAction\] | September 30, 2021 | Glue | 
| AWS Glue DataBrew | arn:aws:states:::aws\-sdk:databrew:\[apiAction\] | September 30, 2021 | DataBrew | 
| AWS IoT Greengrass | arn:aws:states:::aws\-sdk:greengrass:\[apiAction\] | September 30, 2021 | Greengrass | 
| AWS Ground Station | arn:aws:states:::aws\-sdk:groundstation:\[apiAction\] | September 30, 2021 | GroundStation | 
| AWS Identity and Access Management | arn:aws:states:::aws\-sdk:iam:\[apiAction\] | September 30, 2021 | Iam | 
| AWS IoT | arn:aws:states:::aws\-sdk:iot:\[apiAction\] [5](#UnsupportedFootnote5) | September 30, 2021 | Iot | 
| AWS IoT 1\-Click | arn:aws:states:::aws\-sdk:iot1clickprojects:\[apiAction\] | September 30, 2021 | Iot1ClickProjects | 
| AWS IoT Analytics | arn:aws:states:::aws\-sdk:iotanalytics:\[apiAction\] | September 30, 2021 | IoTAnalytics | 
| AWS IoT Core Device Advisor  | arn:aws:states:::aws\-sdk:iotdeviceadvisor:\[apiAction\] [6](#UnsupportedFootnote6) | September 30, 2021 | IotDeviceAdvisor | 
| AWS IoT Events | arn:aws:states:::aws\-sdk:iotevents:\[apiAction\] | September 30, 2021 | IotEvents | 
| AWS IoT Events Data | arn:aws:states:::aws\-sdk:ioteventsdata:\[apiAction\] | September 30, 2021 | IotEventsData | 
| AWS IoT Fleet Hub | arn:aws:states:::aws\-sdk:iotfleethub:\[apiAction\] | September 30, 2021 | IoTFleetHub | 
| AWS IoT Greengrass Version 2 | arn:aws:states:::aws\-sdk:greengrassv2:\[apiAction\] | September 30, 2021 | GreengrassV2 | 
| AWS IoT Jobs Data Plane | arn:aws:states:::aws\-sdk:iotjobsdataplane:\[apiAction\] | September 30, 2021 | IotJobsDataPlane | 
| AWS IoT Secure Tunneling | arn:aws:states:::aws\-sdk:iotsecuretunneling:\[apiAction\] | September 30, 2021 | IoTSecureTunneling | 
| AWS IoT SiteWise | arn:aws:states:::aws\-sdk:iotsitewise:\[apiAction\] | September 30, 2021 | IoTSiteWise | 
| AWS IoT Things Graph | arn:aws:states:::aws\-sdk:iotthingsgraph:\[apiAction\] | September 30, 2021 | IoTThingsGraph | 
| AWS IoT Wireless | arn:aws:states:::aws\-sdk:iotwireless:\[apiAction\] | September 30, 2021 | IotWireless | 
| AWS Key Management Service | arn:aws:states:::aws\-sdk:kms:\[apiAction\] | September 30, 2021 | Kms | 
| AWS Lake Formation | arn:aws:states:::aws\-sdk:lakeformation:\[apiAction\] | September 30, 2021 | LakeFormation | 
| AWS Lambda  | arn:aws:states:::aws\-sdk:lambda:\[apiAction\] [7](#UnsupportedFootnote7) | September 30, 2021 | Lambda | 
| AWS License Manager | arn:aws:states:::aws\-sdk:licensemanager:\[apiAction\] | September 30, 2021 | LicenseManager | 
| AWS Marketplace | arn:aws:states:::aws\-sdk:marketplacecatalog:\[apiAction\] | September 30, 2021 | MarketplaceCatalog | 
| AWS Marketplace Commerce Analytics | arn:aws:states:::aws\-sdk:marketplacecommerceanalytics:\[apiAction\] | September 30, 2021 | MarketplaceCommerceAnalytics | 
| AWS Marketplace Entitlement Service | arn:aws:states:::aws\-sdk:marketplaceentitlement:\[apiAction\] | September 30, 2021 | MarketplaceEntitlement | 
| AWS Elemental MediaTailor | arn:aws:states:::aws\-sdk:mediatailor:\[apiAction\] | September 30, 2021 | MediaTailor | 
| AWS Migration Hub | arn:aws:states:::aws\-sdk:migrationhub:\[apiAction\] | September 30, 2021 | MigrationHub | 
| AWS Migration Hub Config | arn:aws:states:::aws\-sdk:migrationhubconfig:\[apiAction\] | September 30, 2021 | MigrationHubConfig | 
| AWS Migration Hub Strategy Recommendations |  arn:aws:states:::aws\-sdk:migrationhubstrategy:\[apiAction\]  | April 19, 2022 | MigrationHubStrategy | 
| AWS Mobile | arn:aws:states:::aws\-sdk:mobile:\[apiAction\] | September 30, 2021 |  | 
| AWS Network Firewall | arn:aws:states:::aws\-sdk:networkfirewall:\[apiAction\] | September 30, 2021 | NetworkFirewall | 
| AWS OpsWorks | arn:aws:states:::aws\-sdk:opsworks:\[apiAction\] | September 30, 2021 | OpsWorks | 
| AWS OpsWorks CM | arn:aws:states:::aws\-sdk:opsworkscm:\[apiAction\] | September 30, 2021 | OpsWorksCm | 
| AWS Organizations | arn:aws:states:::aws\-sdk:organizations:\[apiAction\] | September 30, 2021 | Organizations | 
| AWS Outposts | arn:aws:states:::aws\-sdk:outposts:\[apiAction\] | September 30, 2021 | Outposts | 
| AWS Panorama | arn:aws:states:::aws\-sdk:panorama:\[apiAction\] | April 19, 2022 | Panorama | 
| Amazon Relational Database Service Performance Insights | arn:aws:states:::aws\-sdk:pi:\[apiAction\] | September 30, 2021 | Pi | 
| AWS Price List | arn:aws:states:::aws\-sdk:pricing:\[apiAction\] | September 30, 2021 | Pricing | 
| Amazon Relational Database Service  | arn:aws:states:::aws\-sdk:rdsdata:\[apiAction\] [8](#UnsupportedFootnote8) | September 30, 2021 | RdsData | 
| AWS Resilience Hub | arn:aws:states:::aws\-sdk:resiliencehub:\[apiAction\] | April 19, 2022 | Resiliencehub | 
| AWS Resource Access Manager | arn:aws:states:::aws\-sdk:ram:\[apiAction\] | September 30, 2021 | Ram | 
| AWS Resource Groups | arn:aws:states:::aws\-sdk:resourcegroups:\[apiAction\] | September 30, 2021 | ResourceGroups | 
| AWS Resource Groups Tagging API | arn:aws:states:::aws\-sdk:resourcegroupstaggingapi:\[apiAction\] | September 30, 2021 | ResourceGroupsTaggingApi | 
| AWS RoboMaker | arn:aws:states:::aws\-sdk:robomaker:\[apiAction\] | September 30, 2021 | RoboMaker | 
| AWS Single Sign\-On | arn:aws:states:::aws\-sdk:identitystore:\[apiAction\] | September 30, 2021 | Identitystore | 
| AWS SSO OIDC | arn:aws:states:::aws\-sdk:ssooidc:\[apiAction\] | September 30, 2021 | SsoOidc | 
| AWS Secrets Manager | arn:aws:states:::aws\-sdk:secretsmanager:\[apiAction\] | September 30, 2021 | SecretsManager | 
| AWS Security Token Service | arn:aws:states:::aws\-sdk:sts:\[apiAction\] [9](#UnsupportedFootnote9) | September 30, 2021 | Sts | 
| AWS Security Hub | arn:aws:states:::aws\-sdk:securityhub:\[apiAction\] | September 30, 2021 | SecurityHub | 
| AWS Server Migration Service | arn:aws:states:::aws\-sdk:sms:\[apiAction\] | September 30, 2021 | Sms | 
| AWS Service Catalog | arn:aws:states:::aws\-sdk:servicecatalog:\[apiAction\] | September 30, 2021 | ServiceCatalog | 
| AWS Service Catalog App Registry | arn:aws:states:::aws\-sdk:servicecatalogappregistry:\[apiAction\] | September 30, 2021 | ServiceCatalogAppRegistry | 
| AWS Shield | arn:aws:states:::aws\-sdk:shield:\[apiAction\] [10](#UnsupportedFootnote10) | September 30, 2021 | Shield | 
| AWS Signer | arn:aws:states:::aws\-sdk:signer:\[apiAction\] | September 30, 2021 | Signer | 
| AWS Single Sign\-On | arn:aws:states:::aws\-sdk:sso:\[apiAction\] | September 30, 2021 | Sso | 
| AWS Single Sign\-On Admin | arn:aws:states:::aws\-sdk:ssoadmin:\[apiAction\] | September 30, 2021 | SsoAdmin | 
| AWS Step Functions | arn:aws:states:::aws\-sdk:sfn:\[apiAction\] | September 30, 2021 | Sfn | 
| AWS Storage Gateway | arn:aws:states:::aws\-sdk:storagegateway:\[apiAction\] | September 30, 2021 | StorageGateway | 
| AWS Support | arn:aws:states:::aws\-sdk:support:\[apiAction\] | September 30, 2021 | Support | 
| AWS Transfer Family | arn:aws:states:::aws\-sdk:transfer:\[apiAction\] | September 30, 2021 | Transfer | 
| AWS WAF | arn:aws:states:::aws\-sdk:waf:\[apiAction\] | September 30, 2021 | Waf | 
| AWS WAF Regional | arn:aws:states:::aws\-sdk:wafregional:\[apiAction\] | September 30, 2021 | WafRegional | 
| AWS WAFV2 | arn:aws:states:::aws\-sdk:wafv2:\[apiAction\] | September 30, 2021 | Wafv2 | 
| AWS Well\-Architected Tool | arn:aws:states:::aws\-sdk:wellarchitected:\[apiAction\] | September 30, 2021 | WellArchitected | 
| AWS X\-Ray | arn:aws:states:::aws\-sdk:xray:\[apiAction\] | September 30, 2021 | XRay | 
| AWS Marketplace Metering Service | arn:aws:states:::aws\-sdk:marketplacemetering:\[apiAction\] | September 30, 2021 | MarketplaceMetering | 
| AWS Serverless Application Repository | arn:aws:states:::aws\-sdk:serverlessapplicationrepository:\[apiAction\] | September 30, 2021 | ServerlessApplicationRepository | 
| AWS Identity and Access Management Access Analyzer | arn:aws:states:::aws\-sdk:accessanalyzer:\[apiAction\] | September 30, 2021 | AccessAnalyzer | 
| Alexa for Business | arn:aws:states:::aws\-sdk:alexaforbusiness:\[apiAction\] | September 30, 2021 | AlexaForBusiness | 
| Amazon API Gateway | arn:aws:states:::aws\-sdk:apigateway:\[apiAction\] | September 30, 2021 | ApiGateway | 
| Amazon API Gateway | arn:aws:states:::aws\-sdk:apigatewayv2:\[apiAction\] | September 30, 2021 | ApiGatewayV2 | 
| Amazon AppIntegrations | arn:aws:states:::aws\-sdk:appintegrations:\[apiAction\] | September 30, 2021 | AppIntegrations | 
| Amazon AppStream 2\.0 | arn:aws:states:::aws\-sdk:appstream:\[apiAction\] | September 30, 2021 | AppStream | 
| Amazon AppFlow | arn:aws:states:::aws\-sdk:appflow:\[apiAction\] | September 30, 2021 | Appflow | 
| Amazon Athena | arn:aws:states:::aws\-sdk:athena:\[apiAction\] | September 30, 2021 | Athena | 
| Amazon Augmented AI | arn:aws:states:::aws\-sdk:sagemakera2iruntime:\[apiAction\] | September 30, 2021 | SageMakerA2IRuntime | 
| Amazon Braket | arn:aws:states:::aws\-sdk:braket:\[apiAction\] | September 30, 2021 | Braket | 
| Amazon Chime | arn:aws:states:::aws\-sdk:chime:\[apiAction\] | September 30, 2021 | Chime | 
| Amazon Chime Meetings | arn:aws:states:::aws\-sdk:chimesdkmeetings:\[apiAction\] | April 19, 2022 | ChimeSdkMeetings | 
| Amazon Cloud Directory | arn:aws:states:::aws\-sdk:clouddirectory:\[apiAction\] | September 30, 2021 | CloudDirectory | 
| Amazon CloudFront | arn:aws:states:::aws\-sdk:cloudfront:\[apiAction\] | September 30, 2021 | CloudFront | 
| Amazon CloudSearch | arn:aws:states:::aws\-sdk:cloudsearch:\[apiAction\] | September 30, 2021 | CloudSearch | 
| Amazon CloudWatch | arn:aws:states:::aws\-sdk:cloudwatch:\[apiAction\] | September 30, 2021 | CloudWatch | 
| Amazon CloudWatch Application Insights | arn:aws:states:::aws\-sdk:applicationinsights:\[apiAction\] | September 30, 2021 | ApplicationInsights | 
| CloudWatch Evidently | arn:aws:states:::aws\-sdk:evidently:\[apiAction\] | April 19, 2022 | Evidently | 
| Amazon CloudWatch Logs | arn:aws:states:::aws\-sdk:cloudwatchlogs:\[apiAction\] | September 30, 2021 | CloudWatchLogs | 
| Amazon CloudWatch RUM | arn:aws:states:::aws\-sdk:rum:\[apiAction\] | April 19, 2022 | Rum | 
| Amazon CloudWatch Synthetics | arn:aws:states:::aws\-sdk:synthetics:\[apiAction\] | September 30, 2021 | Synthetics | 
| Amazon CodeGuru Profiler | arn:aws:states:::aws\-sdk:codeguruprofiler:\[apiAction\] | September 30, 2021 | CodeGuruProfiler | 
| Amazon CodeGuru Reviewer | arn:aws:states:::aws\-sdk:codegurureviewer:\[apiAction\] | September 30, 2021 | CodeGuruReviewer | 
| Amazon Cognito | arn:aws:states:::aws\-sdk:cognitoidentity:\[apiAction\] | September 30, 2021 | CognitoIdentity | 
| Amazon Cognito Identity Provider | arn:aws:states:::aws\-sdk:cognitoidentityprovider:\[apiAction\] | September 30, 2021 | CognitoIdentityProvider | 
| Amazon Cognito Sync | arn:aws:states:::aws\-sdk:cognitosync:\[apiAction\] | September 30, 2021 | CognitoSync | 
| Amazon Comprehend | arn:aws:states:::aws\-sdk:comprehend:\[apiAction\] | September 30, 2021 | Comprehend | 
| Amazon Comprehend Medical | arn:aws:states:::aws\-sdk:comprehendmedical:\[apiAction\] [11](#UnsupportedFootnote11) | September 30, 2021 | ComprehendMedical | 
| Amazon Connect Contact Lens | arn:aws:states:::aws\-sdk:connectcontactlens:\[apiAction\] | September 30, 2021 | ConnectContactLens | 
| Amazon Connect Participant Service | arn:aws:states:::aws\-sdk:connectparticipant:\[apiAction\] | September 30, 2021 | ConnectParticipant | 
| Amazon Connect | arn:aws:states:::aws\-sdk:connect:\[apiAction\] | September 30, 2021 | Connect | 
| Amazon Connect Voice ID | arn:aws:states:::aws\-sdk:voiceid:\[apiAction\] | April 19, 2022 | VoiceId | 
| Amazon Connect Wisdom | arn:aws:states:::aws\-sdk:wisdom:\[apiAction\] | April 19, 2022 | Wisdom | 
| Amazon Data Lifecycle Manager | arn:aws:states:::aws\-sdk:dlm:\[apiAction\] | September 30, 2021 | Dlm | 
| Amazon Detective | arn:aws:states:::aws\-sdk:detective:\[apiAction\] | September 30, 2021 | Detective | 
| Amazon DevOps Guru | arn:aws:states:::aws\-sdk:devopsguru:\[apiAction\] | September 30, 2021 | DevOpsGuru | 
| Amazon DocumentDB \(with MongoDB compatibility\) | arn:aws:states:::aws\-sdk:docdb:\[apiAction\] | September 30, 2021 | DocDb | 
| Amazon DynamoDB | arn:aws:states:::aws\-sdk:dynamodb:\[apiAction\] | September 30, 2021 | DynamoDb | 
| Amazon DynamoDB Streams | arn:aws:states:::aws\-sdk:dynamodbstreams:\[apiAction\] | September 30, 2021 | DynamoDbStreams | 
| Amazon EC2 Container Registry | arn:aws:states:::aws\-sdk:ecr:\[apiAction\] | September 30, 2021 | Ecr | 
| Amazon EC2 Container Service | arn:aws:states:::aws\-sdk:ecs:\[apiAction\] | September 30, 2021 | Ecs | 
| Amazon EC2 Systems Manager | arn:aws:states:::aws\-sdk:ssm:\[apiAction\] | September 30, 2021 | Ssm | 
| Amazon EMR | arn:aws:states:::aws\-sdk:emrcontainers:\[apiAction\] [12](#UnsupportedFootnote12) | September 30, 2021 | EmrContainers | 
| Amazon ElastiCache | arn:aws:states:::aws\-sdk:elasticache:\[apiAction\] | September 30, 2021 | ElastiCache | 
| Amazon Elastic Inference | arn:aws:states:::aws\-sdk:elasticinference:\[apiAction\] | September 30, 2021 | ElasticInference | 
| Amazon Elastic Block Store | arn:aws:states:::aws\-sdk:ebs:\[apiAction\] | September 30, 2021 | Ebs | 
| Amazon Elastic Compute Cloud | arn:aws:states:::aws\-sdk:ec2:\[apiAction\] | September 30, 2021 | Ec2 | 
| Amazon Elastic Container Registry Public | arn:aws:states:::aws\-sdk:ecrpublic:\[apiAction\] | September 30, 2021 | EcrPublic | 
| Amazon Elastic File System | arn:aws:states:::aws\-sdk:efs:\[apiAction\] [13](#UnsupportedFootnote13) | September 30, 2021 | Efs | 
| Amazon Elastic Kubernetes Service | arn:aws:states:::aws\-sdk:eks:\[apiAction\] | September 30, 2021 | Eks | 
| Amazon EMR | arn:aws:states:::aws\-sdk:emr:\[apiAction\] | September 30, 2021 | Emr | 
| Amazon Elastic Transcoder | arn:aws:states:::aws\-sdk:elastictranscoder:\[apiAction\] [14](#UnsupportedFootnote14) | September 30, 2021 | ElasticTranscoder | 
| Amazon OpenSearch Service | arn:aws:states:::aws\-sdk:elasticsearch:\[apiAction\] | September 30, 2021 | Elasticsearch | 
| Amazon OpenSearch Service | arn:aws:states:::aws\-sdk:opensearch:\[apiAction\] | April 19, 2022 | OpenSearch | 
| Amazon EventBridge | arn:aws:states:::aws\-sdk:eventbridge:\[apiAction\] | September 30, 2021 | EventBridge | 
| Amazon FSx | arn:aws:states:::aws\-sdk:fsx:\[apiAction\] | September 30, 2021 | FSx | 
| Amazon Forecast Query | arn:aws:states:::aws\-sdk:forecastquery:\[apiAction\] | September 30, 2021 | Forecastquery | 
| Amazon Forecast Service | arn:aws:states:::aws\-sdk:forecast:\[apiAction\] | September 30, 2021 | Forecast | 
| Amazon Fraud Detector | arn:aws:states:::aws\-sdk:frauddetector:\[apiAction\] | September 30, 2021 | FraudDetector | 
| Amazon GameLift | arn:aws:states:::aws\-sdk:gamelift:\[apiAction\] | September 30, 2021 | GameLift | 
| Amazon S3 Glacier | arn:aws:states:::aws\-sdk:glacier:\[apiAction\] | September 30, 2021 | Glacier | 
| Amazon GuardDuty | arn:aws:states:::aws\-sdk:guardduty:\[apiAction\] | September 30, 2021 | GuardDuty | 
| Amazon HealthLake | arn:aws:states:::aws\-sdk:healthlake:\[apiAction\] | September 30, 2021 | HealthLake | 
| Amazon Honeycode | arn:aws:states:::aws\-sdk:honeycode:\[apiAction\] | September 30, 2021 | Honeycode | 
| Amazon Inspector | arn:aws:states:::aws\-sdk:inspector:\[apiAction\] | September 30, 2021 | Inspector | 
| Amazon Inspector V2 | arn:aws:states:::aws\-sdk:inspector2:\[apiAction\] | April 19, 2022 | Inspector2 | 
| Amazon Interactive Video Service | arn:aws:states:::aws\-sdk:ivs:\[apiAction\] | September 30, 2021 | Ivs | 
| Amazon Kendra | arn:aws:states:::aws\-sdk:kendra:\[apiAction\] | September 30, 2021 | Kendra | 
| Amazon Kinesis | arn:aws:states:::aws\-sdk:kinesis:\[apiAction\] [15](#UnsupportedFootnote15) | September 30, 2021 | Kinesis | 
| Amazon Kinesis Analytics | arn:aws:states:::aws\-sdk:kinesisanalytics:\[apiAction\] | September 30, 2021 | KinesisAnalytics | 
| Amazon Kinesis Analytics V2 | arn:aws:states:::aws\-sdk:kinesisanalyticsv2:\[apiAction\] | September 30, 2021 | KinesisAnalyticsV2 | 
| Amazon Kinesis Firehose | arn:aws:states:::aws\-sdk:firehose:\[apiAction\] | September 30, 2021 | Firehose | 
| Amazon Kinesis Video Signaling Channels | arn:aws:states:::aws\-sdk:kinesisvideosignaling:\[apiAction\] | September 30, 2021 | KinesisVideoSignaling | 
| Amazon Kinesis Video Streams | arn:aws:states:::aws\-sdk:kinesisvideo:\[apiAction\] | September 30, 2021 | KinesisVideo | 
| Amazon Kinesis Video Streams Archived Media | arn:aws:states:::aws\-sdk:kinesisvideoarchivedmedia:\[apiAction\] | September 30, 2021 | KinesisVideoArchivedMedia | 
| Amazon Kinesis video stream | arn:aws:states:::aws\-sdk:kinesisvideomedia:\[apiAction\] | September 30, 2021 | KinesisVideoMedia | 
| Amazon Lex Model Building Service | arn:aws:states:::aws\-sdk:lexmodelbuilding:\[apiAction\] | September 30, 2021 | LexModelBuilding | 
| Amazon Lex Model Building Service V2 | arn:aws:states:::aws\-sdk:lexmodelsv2:\[apiAction\] | September 30, 2021 | LexModelsV2 | 
| Amazon Lex | arn:aws:states:::aws\-sdk:lexruntime:\[apiAction\] | September 30, 2021 | LexRuntime | 
| Amazon Lex Runtime V2  | arn:aws:states:::aws\-sdk:lexruntimev2:\[apiAction\] [16](#UnsupportedFootnote16) | September 30, 2021 | LexRuntimeV2 | 
| Amazon Lightsail | arn:aws:states:::aws\-sdk:lightsail:\[apiAction\] | September 30, 2021 | Lightsail | 
| Amazon Location Service | arn:aws:states:::aws\-sdk:location:\[apiAction\] | September 30, 2021 | Location | 
| Amazon Lookout for Vision | arn:aws:states:::aws\-sdk:lookoutvision:\[apiAction\] | September 30, 2021 | LookoutVision | 
| Amazon MQ | arn:aws:states:::aws\-sdk:mq:\[apiAction\] | September 30, 2021 | Mq | 
| Amazon Macie | arn:aws:states:::aws\-sdk:macie:\[apiAction\] | September 30, 2021 |  | 
| Amazon Macie 2 | arn:aws:states:::aws\-sdk:macie2:\[apiAction\] | September 30, 2021 | Macie2 | 
| Amazon Managed Blockchain | arn:aws:states:::aws\-sdk:managedblockchain:\[apiAction\] | September 30, 2021 | ManagedBlockchain | 
| Amazon Managed Grafana | arn:aws:states:::aws\-sdk:grafana:\[apiAction\] | April 19, 2022 | Grafana | 
| Amazon Managed Service for Prometheus | arn:aws:states:::aws\-sdk:amp:\[apiAction\] | September 30, 2021 | Amp | 
| Amazon Managed Streaming for Apache Kafka | arn:aws:states:::aws\-sdk:kafka:\[apiAction\] | September 30, 2021 | Kafka | 
| Amazon MSK Connect | arn:aws:states:::aws\-sdk:kafkaconnect:\[apiAction\] | April 19, 2022 | KafkaConnect | 
| Amazon Managed Workflows for Apache Airflow | arn:aws:states:::aws\-sdk:mwaa:\[apiAction\] | September 30, 2021 | Mwaa | 
| Amazon Mechanical Turk | arn:aws:states:::aws\-sdk:mturk:\[apiAction\] | September 30, 2021 | MTurk | 
| Amazon MemoryDB for Redis | arn:aws:states:::aws\-sdk:memorydb:\[apiAction\] | April 19, 2022 | MemoryDB | 
| Amazon Neptune | arn:aws:states:::aws\-sdk:neptune:\[apiAction\] | September 30, 2021 | Neptune | 
| Amazon Personalize | arn:aws:states:::aws\-sdk:personalize:\[apiAction\] | September 30, 2021 | Personalize | 
| Amazon Personalize Events | arn:aws:states:::aws\-sdk:personalizeevents:\[apiAction\] | September 30, 2021 | PersonalizeEvents | 
| Amazon Personalize Runtime | arn:aws:states:::aws\-sdk:personalizeruntime:\[apiAction\] | September 30, 2021 | PersonalizeRuntime | 
| Amazon Pinpoint | arn:aws:states:::aws\-sdk:pinpoint:\[apiAction\] | September 30, 2021 | Pinpoint | 
| Amazon Pinpoint Email Service | arn:aws:states:::aws\-sdk:pinpointemail:\[apiAction\] | September 30, 2021 | PinpointEmail | 
| Amazon Pinpoint SMS and Voice Service | arn:aws:states:::aws\-sdk:pinpointsmsvoice:\[apiAction\] | September 30, 2021 | PinpointSmsVoice | 
| Amazon Polly | arn:aws:states:::aws\-sdk:polly:\[apiAction\] | September 30, 2021 | Polly | 
| Amazon QLDB | arn:aws:states:::aws\-sdk:qldb:\[apiAction\] | September 30, 2021 | Qldb | 
| Amazon QLDB Session | arn:aws:states:::aws\-sdk:qldbsession:\[apiAction\] | September 30, 2021 | QldbSession | 
| Amazon QuickSight | arn:aws:states:::aws\-sdk:quicksight:\[apiAction\] | September 30, 2021 | QuickSight | 
| Amazon Redshift | arn:aws:states:::aws\-sdk:redshift:\[apiAction\] | September 30, 2021 | Redshift | 
| Amazon Redshift Data API | arn:aws:states:::aws\-sdk:redshiftdata:\[apiAction\] | September 30, 2021 | RedshiftData | 
| Amazon Rekognition | arn:aws:states:::aws\-sdk:rekognition:\[apiAction\] | September 30, 2021 | Rekognition | 
| Amazon Relational Database Service | arn:aws:states:::aws\-sdk:rds:\[apiAction\] | September 30, 2021 | Rds | 
| Amazon Route 53 | arn:aws:states:::aws\-sdk:route53:\[apiAction\] | September 30, 2021 | Route53 | 
| Amazon Route 53 Domains | arn:aws:states:::aws\-sdk:route53domains:\[apiAction\] | September 30, 2021 | Route53Domains | 
| Amazon Route 53 Resolver | arn:aws:states:::aws\-sdk:route53resolver:\[apiAction\] | September 30, 2021 | Route53Resolver | 
| Amazon S3 on Outposts | arn:aws:states:::aws\-sdk:s3outposts:\[apiAction\] | September 30, 2021 | S3Outposts | 
| Amazon SageMaker Feature Store Runtime | arn:aws:states:::aws\-sdk:sagemakerfeaturestoreruntime:\[apiAction\] | September 30, 2021 | SageMakerFeatureStoreRuntime | 
| Amazon SageMaker Runtime | arn:aws:states:::aws\-sdk:sagemakerruntime:\[apiAction\] | September 30, 2021 | SageMakerRuntime | 
| Amazon SageMaker | arn:aws:states:::aws\-sdk:sagemaker:\[apiAction\] | September 30, 2021 | SageMaker | 
| Amazon SageMaker Edge Manager | arn:aws:states:::aws\-sdk:sagemakeredge:\[apiAction\] | September 30, 2021 | SagemakerEdge | 
| Amazon Simple Email Service | arn:aws:states:::aws\-sdk:ses:\[apiAction\] | September 30, 2021 | Ses | 
| Amazon Simple Email Service V2 | arn:aws:states:::aws\-sdk:sesv2:\[apiAction\] | September 30, 2021 | SesV2 | 
| Amazon Simple Notification Service | arn:aws:states:::aws\-sdk:sns:\[apiAction\] | September 30, 2021 | Sns | 
| Amazon Simple Queue Service | arn:aws:states:::aws\-sdk:sqs:\[apiAction\] | September 30, 2021 | Sqs | 
| Amazon Simple Storage Service | arn:aws:states:::aws\-sdk:s3:\[apiAction\] [17](#UnsupportedFootnote17) | September 30, 2021 | S3 | 
| Amazon Simple Workflow Service | arn:aws:states:::aws\-sdk:swf:\[apiAction\] | September 30, 2021 | Swf | 
| Amazon Textract | arn:aws:states:::aws\-sdk:textract:\[apiAction\] | September 30, 2021 | Textract | 
| Amazon Transcribe | arn:aws:states:::aws\-sdk:transcribe:\[apiAction\] | September 30, 2021 | Transcribe | 
| Amazon Translate | arn:aws:states:::aws\-sdk:translate:\[apiAction\] | September 30, 2021 | Translate | 
| Amazon WorkDocs | arn:aws:states:::aws\-sdk:workdocs:\[apiAction\] | September 30, 2021 | WorkDocs | 
| Amazon WorkMail | arn:aws:states:::aws\-sdk:workmail:\[apiAction\] | September 30, 2021 | WorkMail | 
| Amazon WorkMail Message Flow | arn:aws:states:::aws\-sdk:workmailmessageflow:\[apiAction\] | September 30, 2021 | WorkMailMessageFlow | 
| Amazon WorkSpaces | arn:aws:states:::aws\-sdk:workspaces:\[apiAction\] | September 30, 2021 | WorkSpaces | 
| Amazon WorkSpaces Web | arn:aws:states:::aws\-sdk:workspacesweb:\[apiAction\] | April 19, 2022 | WorkSpacesWeb | 
| Amplify | arn:aws:states:::aws\-sdk:amplifybackend:\[apiAction\] | September 30, 2021 | AmplifyBackend | 
| Amplify UI Builder | arn:aws:states:::aws\-sdk:amplifyuibuilder:\[apiAction\] | April 19, 2022 | AmplifyUiBuilder | 
| Application Auto Scaling | arn:aws:states:::aws\-sdk:applicationautoscaling:\[apiAction\] | September 30, 2021 | ApplicationAutoScaling | 
| Amazon EC2 Auto Scaling | arn:aws:states:::aws\-sdk:autoscaling:\[apiAction\] | September 30, 2021 | Auto Scaling | 
| CodeArtifact | arn:aws:states:::aws\-sdk:codeartifact:\[apiAction\] | September 30, 2021 | Codeartifact | 
| DynamoDB Accelerator | arn:aws:states:::aws\-sdk:dax:\[apiAction\] | September 30, 2021 | Dax | 
| EC2 Image Builder | arn:aws:states:::aws\-sdk:imagebuilder:\[apiAction\] | September 30, 2021 | Imagebuilder | 
| Elastic Disaster Recovery | arn:aws:states:::aws\-sdk:drs:\[apiAction\] | April 19, 2022 | Drs | 
| Elastic Load Balancing | arn:aws:states:::aws\-sdk:elasticloadbalancing:\[apiAction\] | September 30, 2021 | ElasticLoadBalancing | 
| Elastic Load Balancing V2 | arn:aws:states:::aws\-sdk:elasticloadbalancingv2:\[apiAction\] | September 30, 2021 | ElasticLoadBalancingV2 | 
| MediaConnect | arn:aws:states:::aws\-sdk:mediaconnect:\[apiAction\] | September 30, 2021 | MediaConnect | 
| Network Manager | arn:aws:states:::aws\-sdk:networkmanager:\[apiAction\] | September 30, 2021 |  | 
| Amazon S3 Control | arn:aws:states:::aws\-sdk:s3control:\[apiAction\] [18](#UnsupportedFootnote18) | September 30, 2021 | S3Control | 
| Recycle Bin for Amazon EBS | arn:aws:states:::aws\-sdk:rbin:\[apiAction\] | April 19, 2022 | Rbin | 
| Savings Plans | arn:aws:states:::aws\-sdk:savingsplans:\[apiAction\] | September 30, 2021 | Savingsplans | 
| Amazon EventBridge Schema Registry | arn:aws:states:::aws\-sdk:schemas:\[apiAction\] | September 30, 2021 | Schemas | 
| Service Quotas | arn:aws:states:::aws\-sdk:servicequotas:\[apiAction\] | September 30, 2021 | ServiceQuotas | 
| AWS Snowball | arn:aws:states:::aws\-sdk:snowball:\[apiAction\] | September 30, 2021 | Snowball | 

**Note**  
Amazon OpenSearch Service is the successor of the Amazon Elasticsearch service, and we recommend that you use OpenSearch Service for your service integrations\.

 [1](#AppDisc-footnote1) For information about unsupported API actions for AWS Application Discovery Service integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\.

  [2](#CodeDep-footnote2) For information about unsupported API actions for AWS CodeDeploy integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [3](#DirConn-footnote3) For information about unsupported API actions for AWS Direct Connect integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [4](#EltMediaPkg-footnote4) For information about unsupported API action for AWS Elemental MediaPackage integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [5](#Iot-footnote5) For information about unsupported API actions for AWS IoT integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [6](#IotCoreDevAdv-footnote6) For information about unsupported API action for AWS IoT Core Device Advisor integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [7](#Lambda-footnote7) For information about unsupported API action for AWS Lambda integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [8](#RdsDb-footnote8) For information about unsupported API action for Amazon Relational Database Service integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [9](#Sts-footnote9) For information about unsupported API actions for AWS Security Token Service integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [10](#Shd-footnote10) For information about unsupported API action for AWS Shield integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [11](#CompMed-footnote11) For information about unsupported API action for Amazon Comprehend Medical integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [12](#Emr-footnote12) For information about unsupported API action for Amazon EMR integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [13](#Efs-footnote13) For information about unsupported API action for Amazon Elastic File System integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [14](#Els-trscdr-footnote14) For information about unsupported API action for Amazon Elastic Transcoder integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [15](#Kin-footnote15) For information about unsupported API action for Amazon Kinesis integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [16](#Lex2-footnote16) For information about unsupported API action for Amazon Lex Runtime V2 integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [17](#s3-footnote17) For information about unsupported API action for Amazon Simple Storage Service integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list)\. 

  [18](#s3Cont-footnote18) For information about unsupported API actions for Amazon S3 Control integration, see [Unsupported API actions for supported services](#unsupported-api-actions-list) 

## Unsupported API actions for supported services<a name="unsupported-api-actions-list"></a>

The following table lists the unsupported API actions for AWS SDK service integrations\. The right column contains the API actions that are currently not supported for the service listed in the left column\. 


| Service name | Unsupported API action | 
| --- | --- | 
| AWS Application Discovery Service |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/supported-services-awssdk.html)  | 
| AWS CodeDeploy |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/supported-services-awssdk.html)  | 
| Amazon Comprehend Medical | DetectEntities | 
| AWS Direct Connect |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/supported-services-awssdk.html)  | 
| Amazon Elastic File System | CreateTags | 
| Amazon Elastic Transcoder | TestRole | 
| Amazon EMR | DescribeJobFlows | 
| AWS IoT |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/supported-services-awssdk.html)  | 
| AWS IoT Core Device Advisor | ListTestCases | 
| Amazon Kinesis | SubscribeToShard | 
| AWS Lambda | InvokeAsync | 
| Amazon Lex Runtime V2 | StartConversation | 
| AWS Elemental MediaPackage | RotateChannelCredentials | 
| Amazon Relational Database Service | ExecuteSql | 
| Amazon Simple Storage Service | SelectObjectContent | 
| Amazon S3 Control | SelectObjectContent | 
| AWS Shield | DeleteSubscription | 
| AWS Security Token Service |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/supported-services-awssdk.html)  | 

## Deprecated AWS SDK service integrations<a name="deprecated-aws-sdk-integ"></a>

The following AWS SDK service integrations are now deprecated:
+ AWS Mobile
+ Amazon Macie