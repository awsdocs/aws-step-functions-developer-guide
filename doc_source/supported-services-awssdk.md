# AWS SDK service integrations<a name="supported-services-awssdk"></a>

AWS Step Functions integrates with AWS services, letting you call each service's API actions from your workflow\. You can use Step Functions' [AWS SDK integrations](#supported-services-awssdk) to call any of the over two hundred AWS services directly from your state machine, giving you access to over nine thousand API actions\. Or you can use [Step Functions's Optimized integrations](connect-supported-services.md), each of which has been customized to provide special functionality for your workflow\.

Some services or SDKs may not be available as AWS SDK integrations, either temporarily or permanently, for a variety of reasons\. Very new services may not be available immediately\. Some services require customized configuration, such as specifying a customer\-specific endpoint, which may be more suitable for an optimized integration\. Some SDKs are unsuitable for use in a workflow, such as those for streaming audio or video\. Finally, some services may be withheld until they pass certain internal validations performed by Step Functions\.

To use AWS SDK integrations, you specify the service name and API call, and, optionally, a [service integration pattern](connect-to-resource.md)\. Note that the API action will always be camel case, and parameter names will be Pascal case\. For example, you could use Step Functions' `startSyncExecution` API action and specify the parameter `StateMachineArn`\. You can call AWS SDK services directly from the Amazon States Language in the `Resource` field of a task state\. To do this, use the following syntax:

`arn:aws:states:::aws-sdk:serviceName:apiAction.[serviceIntegrationPattern]`

For example, for Amazon EC2, you might use `arn:aws:states:::aws-sdk:ec2:describeInstances`\. This would return output as defined for the [Amazon EC2 describeInstances](Amazon EC2 API ReferenceAPI_DescribeInstances.html#API_DescribeInstances_ResponseElements) API call\. 

Step Functions can't autogenerate IAM policies for AWS SDK integrations\. After you create your state machine, you will need to navigate to the IAM console and configure your role policies\. See [IAM Policies for integrated services](service-integration-iam-templates.md) for more information\.

See the [Gather Amazon S3 bucket info using AWS SDK service integrations](tutorial-gather-s3-bucket-info-using-aws-sdk-service-integrations.md) tutorial for an example of how to use AWS SDK integrations\.

## Supported services<a name="supported-services-awssdk-list"></a>


**Supported services**  

| Service | `Task` state resource | 
| --- | --- | 
| AWS Amplify | arn:aws:states:::aws\-sdk:amplify:\[apiAction\] | 
| AWS App Mesh | arn:aws:states:::aws\-sdk:appmesh:\[apiAction\] | 
| AWS AppConfig | arn:aws:states:::aws\-sdk:appconfig:\[apiAction\] | 
| AWS AppSync | arn:aws:states:::aws\-sdk:appsync:\[apiAction\] | 
| AWS Application Discovery Service | arn:aws:states:::aws\-sdk:applicationdiscovery:\[apiAction\] | 
| AWS Audit Manager | arn:aws:states:::aws\-sdk:auditmanager:\[apiAction\] | 
| AWS Auto Scaling Plans | arn:aws:states:::aws\-sdk:autoscalingplans:\[apiAction\] | 
| AWS Backup | arn:aws:states:::aws\-sdk:backup:\[apiAction\] | 
| AWS Batch | arn:aws:states:::aws\-sdk:batch:\[apiAction\] | 
| AWS Budgets | arn:aws:states:::aws\-sdk:budgets:\[apiAction\] | 
| AWS Certificate Manager | arn:aws:states:::aws\-sdk:acm:\[apiAction\] | 
| AWS Certificate Manager Private Certificate Authority | arn:aws:states:::aws\-sdk:acmpca:\[apiAction\] | 
| AWS Cloud Map | arn:aws:states:::aws\-sdk:servicediscovery:\[apiAction\] | 
| AWS Cloud9 | arn:aws:states:::aws\-sdk:cloud9:\[apiAction\] | 
| AWS CloudFormation | arn:aws:states:::aws\-sdk:cloudformation:\[apiAction\] | 
| AWS CloudHSM | arn:aws:states:::aws\-sdk:cloudhsm:\[apiAction\] | 
| AWS CloudHSM | arn:aws:states:::aws\-sdk:cloudhsmv2:\[apiAction\] | 
| AWS CloudTrail | arn:aws:states:::aws\-sdk:cloudtrail:\[apiAction\] | 
| AWS CodeBuild | arn:aws:states:::aws\-sdk:codebuild:\[apiAction\] | 
| AWS CodeCommit | arn:aws:states:::aws\-sdk:codecommit:\[apiAction\] | 
| AWS CodeDeploy | arn:aws:states:::aws\-sdk:codedeploy:\[apiAction\] | 
| AWS CodePipeline | arn:aws:states:::aws\-sdk:codepipeline:\[apiAction\] | 
| AWS CodeStar | arn:aws:states:::aws\-sdk:codestar:\[apiAction\] | 
| AWS CodeStar | arn:aws:states:::aws\-sdk:codestarnotifications:\[apiAction\] | 
| AWS CodeStar | arn:aws:states:::aws\-sdk:codestarconnections:\[apiAction\] | 
| AWS Compute Optimizer | arn:aws:states:::aws\-sdk:computeoptimizer:\[apiAction\] | 
| AWS Config | arn:aws:states:::aws\-sdk:config:\[apiAction\] | 
| AWS Cost Explorer Service | arn:aws:states:::aws\-sdk:costexplorer:\[apiAction\] | 
| AWS Cost and Usage Report | arn:aws:states:::aws\-sdk:costandusagereport:\[apiAction\] | 
| AWS Data Exchange | arn:aws:states:::aws\-sdk:dataexchange:\[apiAction\] | 
| AWS Data Pipeline | arn:aws:states:::aws\-sdk:datapipeline:\[apiAction\] | 
| AWS DataSync | arn:aws:states:::aws\-sdk:datasync:\[apiAction\] | 
| AWS Database Migration Service | arn:aws:states:::aws\-sdk:databasemigration:\[apiAction\] | 
| AWS Device Farm | arn:aws:states:::aws\-sdk:devicefarm:\[apiAction\] | 
| AWS Direct Connect | arn:aws:states:::aws\-sdk:directconnect:\[apiAction\] | 
| AWS Directory Service | arn:aws:states:::aws\-sdk:directory:\[apiAction\] | 
| AWS EC2 Instance Connect | arn:aws:states:::aws\-sdk:ec2instanceconnect:\[apiAction\] | 
| AWS Elastic Beanstalk | arn:aws:states:::aws\-sdk:elasticbeanstalk:\[apiAction\] | 
| AWS Elemental MediaLive | arn:aws:states:::aws\-sdk:medialive:\[apiAction\] | 
| AWS Elemental MediaPackage | arn:aws:states:::aws\-sdk:mediapackage:\[apiAction\] | 
| AWS Elemental MediaPackage VOD | arn:aws:states:::aws\-sdk:mediapackagevod:\[apiAction\] | 
| AWS Elemental MediaStore | arn:aws:states:::aws\-sdk:mediastore:\[apiAction\] | 
| AWS Fault Injection Simulator | arn:aws:states:::aws\-sdk:fis:\[apiAction\] | 
| AWS Firewall Manager | arn:aws:states:::aws\-sdk:fms:\[apiAction\] | 
| AWS Glue | arn:aws:states:::aws\-sdk:glue:\[apiAction\] | 
| AWS Glue DataBrew | arn:aws:states:::aws\-sdk:databrew:\[apiAction\] | 
| AWS IoT Greengrass | arn:aws:states:::aws\-sdk:greengrass:\[apiAction\] | 
| AWS Ground Station | arn:aws:states:::aws\-sdk:groundstation:\[apiAction\] | 
| AWS Identity and Access Management | arn:aws:states:::aws\-sdk:iam:\[apiAction\] | 
| AWS IoT | arn:aws:states:::aws\-sdk:iot:\[apiAction\] | 
| AWS IoT 1\-Click | arn:aws:states:::aws\-sdk:iot1clickprojects:\[apiAction\] | 
| AWS IoT Analytics | arn:aws:states:::aws\-sdk:iotanalytics:\[apiAction\] | 
| AWS IoT Core Device Advisor | arn:aws:states:::aws\-sdk:iotdeviceadvisor:\[apiAction\] | 
| AWS IoT Events | arn:aws:states:::aws\-sdk:iotevents:\[apiAction\] | 
| AWS IoT Events Data | arn:aws:states:::aws\-sdk:ioteventsdata:\[apiAction\] | 
| AWS IoT Fleet Hub | arn:aws:states:::aws\-sdk:iotfleethub:\[apiAction\] | 
| AWS IoT Greengrass Version 2 | arn:aws:states:::aws\-sdk:greengrassv2:\[apiAction\] | 
| AWS IoT Jobs Data Plane | arn:aws:states:::aws\-sdk:iotjobsdataplane:\[apiAction\] | 
| AWS IoT Secure Tunneling | arn:aws:states:::aws\-sdk:iotsecuretunneling:\[apiAction\] | 
| AWS IoT SiteWise | arn:aws:states:::aws\-sdk:iotsitewise:\[apiAction\] | 
| AWS IoT Things Graph | arn:aws:states:::aws\-sdk:iotthingsgraph:\[apiAction\] | 
| AWS IoT Wireless | arn:aws:states:::aws\-sdk:iotwireless:\[apiAction\] | 
| AWS Key Management Service | arn:aws:states:::aws\-sdk:kms:\[apiAction\] | 
| AWS Lake Formation | arn:aws:states:::aws\-sdk:lakeformation:\[apiAction\] | 
| AWS Lambda | arn:aws:states:::aws\-sdk:lambda:\[apiAction\] | 
| AWS License Manager | arn:aws:states:::aws\-sdk:licensemanager:\[apiAction\] | 
| AWS Marketplace | arn:aws:states:::aws\-sdk:marketplacecatalog:\[apiAction\] | 
| AWS Marketplace Commerce Analytics | arn:aws:states:::aws\-sdk:marketplacecommerceanalytics:\[apiAction\] | 
| AWS Marketplace Entitlement Service | arn:aws:states:::aws\-sdk:marketplaceentitlement:\[apiAction\] | 
| AWS Elemental MediaTailor | arn:aws:states:::aws\-sdk:mediatailor:\[apiAction\] | 
| AWS Migration Hub | arn:aws:states:::aws\-sdk:migrationhub:\[apiAction\] | 
| AWS Migration Hub Config | arn:aws:states:::aws\-sdk:migrationhubconfig:\[apiAction\] | 
| AWS Mobile | arn:aws:states:::aws\-sdk:mobile:\[apiAction\] | 
| AWS Network Firewall | arn:aws:states:::aws\-sdk:networkfirewall:\[apiAction\] | 
| AWS OpsWorks | arn:aws:states:::aws\-sdk:opsworks:\[apiAction\] | 
| AWS OpsWorks CM | arn:aws:states:::aws\-sdk:opsworkscm:\[apiAction\] | 
| AWS Organizations | arn:aws:states:::aws\-sdk:organizations:\[apiAction\] | 
| AWS Outposts | arn:aws:states:::aws\-sdk:outposts:\[apiAction\] | 
| AWS Performance Insights | arn:aws:states:::aws\-sdk:pi:\[apiAction\] | 
| AWS Price List | arn:aws:states:::aws\-sdk:pricing:\[apiAction\] | 
| AWS RDS DataService | arn:aws:states:::aws\-sdk:rdsdata:\[apiAction\] | 
| AWS Resource Access Manager | arn:aws:states:::aws\-sdk:ram:\[apiAction\] | 
| AWS Resource Groups | arn:aws:states:::aws\-sdk:resourcegroups:\[apiAction\] | 
| AWS Resource Groups Tagging API | arn:aws:states:::aws\-sdk:resourcegroupstaggingapi:\[apiAction\] | 
| AWS RoboMaker | arn:aws:states:::aws\-sdk:robomaker:\[apiAction\] | 
| AWS Single Sign\-On | arn:aws:states:::aws\-sdk:identitystore:\[apiAction\] | 
| AWS SSO OIDC | arn:aws:states:::aws\-sdk:ssooidc:\[apiAction\] | 
| AWS Secrets Manager | arn:aws:states:::aws\-sdk:secretsmanager:\[apiAction\] | 
| AWS Security Token Service | arn:aws:states:::aws\-sdk:sts:\[apiAction\] | 
| AWS Security Hub | arn:aws:states:::aws\-sdk:securityhub:\[apiAction\] | 
| AWS Server Migration Service | arn:aws:states:::aws\-sdk:sms:\[apiAction\] | 
| AWS Service Catalog | arn:aws:states:::aws\-sdk:servicecatalog:\[apiAction\] | 
| AWS Service Catalog App Registry | arn:aws:states:::aws\-sdk:servicecatalogappregistry:\[apiAction\] | 
| AWS Shield | arn:aws:states:::aws\-sdk:shield:\[apiAction\] | 
| AWS Signer | arn:aws:states:::aws\-sdk:signer:\[apiAction\] | 
| AWS Single Sign\-On | arn:aws:states:::aws\-sdk:sso:\[apiAction\] | 
| AWS Single Sign\-On Admin | arn:aws:states:::aws\-sdk:ssoadmin:\[apiAction\] | 
| AWS Step Functions | arn:aws:states:::aws\-sdk:sfn:\[apiAction\] | 
| AWS Storage Gateway | arn:aws:states:::aws\-sdk:storagegateway:\[apiAction\] | 
| AWS Support | arn:aws:states:::aws\-sdk:support:\[apiAction\] | 
| AWS Transfer Family | arn:aws:states:::aws\-sdk:transfer:\[apiAction\] | 
| AWS WAF | arn:aws:states:::aws\-sdk:waf:\[apiAction\] | 
| AWS WAF Regional | arn:aws:states:::aws\-sdk:wafregional:\[apiAction\] | 
| AWS WAFV2 | arn:aws:states:::aws\-sdk:wafv2:\[apiAction\] | 
| AWS Well\-Architected Tool | arn:aws:states:::aws\-sdk:wellarchitected:\[apiAction\] | 
| AWS X\-Ray | arn:aws:states:::aws\-sdk:xray:\[apiAction\] | 
| AWS Marketplace Metering Service | arn:aws:states:::aws\-sdk:marketplacemetering:\[apiAction\] | 
| AWS Serverless Application Repository | arn:aws:states:::aws\-sdk:serverlessapplicationrepository:\[apiAction\] | 
| AWS Identity and Access Management Access Analyzer | arn:aws:states:::aws\-sdk:accessanalyzer:\[apiAction\] | 
| Alexa for Business | arn:aws:states:::aws\-sdk:alexaforbusiness:\[apiAction\] | 
| Amazon API Gateway | arn:aws:states:::aws\-sdk:apigateway:\[apiAction\] | 
| Amazon API Gateway | arn:aws:states:::aws\-sdk:apigatewayv2:\[apiAction\] | 
| Amazon AppIntegrations | arn:aws:states:::aws\-sdk:appintegrations:\[apiAction\] | 
| Amazon AppStream | arn:aws:states:::aws\-sdk:appstream:\[apiAction\] | 
| Amazon AppFlow | arn:aws:states:::aws\-sdk:appflow:\[apiAction\] | 
| Amazon Athena | arn:aws:states:::aws\-sdk:athena:\[apiAction\] | 
| Amazon Augmented AI | arn:aws:states:::aws\-sdk:sagemakera2iruntime:\[apiAction\] | 
| Amazon Braket | arn:aws:states:::aws\-sdk:braket:\[apiAction\] | 
| Amazon Chime | arn:aws:states:::aws\-sdk:chime:\[apiAction\] | 
| Amazon Cloud Directory | arn:aws:states:::aws\-sdk:clouddirectory:\[apiAction\] | 
| Amazon CloudFront | arn:aws:states:::aws\-sdk:cloudfront:\[apiAction\] | 
| Amazon CloudSearch | arn:aws:states:::aws\-sdk:cloudsearch:\[apiAction\] | 
| Amazon CloudWatch | arn:aws:states:::aws\-sdk:cloudwatch:\[apiAction\] | 
| Amazon CloudWatch Application Insights | arn:aws:states:::aws\-sdk:applicationinsights:\[apiAction\] | 
| Amazon CloudWatch Logs | arn:aws:states:::aws\-sdk:cloudwatchlogs:\[apiAction\] | 
| Amazon CloudWatch Synthetics | arn:aws:states:::aws\-sdk:synthetics:\[apiAction\] | 
| Amazon CodeGuru Profiler | arn:aws:states:::aws\-sdk:codeguruprofiler:\[apiAction\] | 
| Amazon CodeGuru Reviewer | arn:aws:states:::aws\-sdk:codegurureviewer:\[apiAction\] | 
| Amazon Cognito | arn:aws:states:::aws\-sdk:cognitoidentity:\[apiAction\] | 
| Amazon Cognito Identity Provider | arn:aws:states:::aws\-sdk:cognitoidentityprovider:\[apiAction\] | 
| Amazon Cognito Sync | arn:aws:states:::aws\-sdk:cognitosync:\[apiAction\] | 
| Amazon Comprehend | arn:aws:states:::aws\-sdk:comprehend:\[apiAction\] | 
| Amazon Comprehend Medical | arn:aws:states:::aws\-sdk:comprehendmedical:\[apiAction\] | 
| Amazon Connect Contact Lens | arn:aws:states:::aws\-sdk:connectcontactlens:\[apiAction\] | 
| Amazon Connect Participant Service | arn:aws:states:::aws\-sdk:connectparticipant:\[apiAction\] | 
| Amazon Connect | arn:aws:states:::aws\-sdk:connect:\[apiAction\] | 
| Amazon Data Lifecycle Manager | arn:aws:states:::aws\-sdk:dlm:\[apiAction\] | 
| Amazon Detective | arn:aws:states:::aws\-sdk:detective:\[apiAction\] | 
| Amazon DevOps Guru | arn:aws:states:::aws\-sdk:devopsguru:\[apiAction\] | 
| Amazon DocumentDB \(with MongoDB compatibility\) | arn:aws:states:::aws\-sdk:docdb:\[apiAction\] | 
| Amazon DynamoDB | arn:aws:states:::aws\-sdk:dynamodb:\[apiAction\] | 
| Amazon DynamoDB Streams | arn:aws:states:::aws\-sdk:dynamodbstreams:\[apiAction\] | 
| Amazon EC2 Container Registry | arn:aws:states:::aws\-sdk:ecr:\[apiAction\] | 
| Amazon EC2 Container Service | arn:aws:states:::aws\-sdk:ecs:\[apiAction\] | 
| Amazon EC2 Systems Manager | arn:aws:states:::aws\-sdk:ssm:\[apiAction\] | 
| Amazon EMR | arn:aws:states:::aws\-sdk:emrcontainers:\[apiAction\] | 
| Amazon ElastiCache | arn:aws:states:::aws\-sdk:elasticache:\[apiAction\] | 
| Amazon Elastic Inference | arn:aws:states:::aws\-sdk:elasticinference:\[apiAction\] | 
| Amazon Elastic Block Store | arn:aws:states:::aws\-sdk:ebs:\[apiAction\] | 
| Amazon Elastic Compute Cloud | arn:aws:states:::aws\-sdk:ec2:\[apiAction\] | 
| Amazon Elastic Container Registry Public | arn:aws:states:::aws\-sdk:ecrpublic:\[apiAction\] | 
| Amazon Elastic File System | arn:aws:states:::aws\-sdk:efs:\[apiAction\] | 
| Amazon Elastic Kubernetes Service | arn:aws:states:::aws\-sdk:eks:\[apiAction\] | 
| Amazon EMR | arn:aws:states:::aws\-sdk:emr:\[apiAction\] | 
| Amazon Elastic Transcoder | arn:aws:states:::aws\-sdk:elastictranscoder:\[apiAction\] | 
| Amazon OpenSearch Service | arn:aws:states:::aws\-sdk:elasticsearch:\[apiAction\] | 
| Amazon EventBridge | arn:aws:states:::aws\-sdk:eventbridge:\[apiAction\] | 
| Amazon FSx | arn:aws:states:::aws\-sdk:fsx:\[apiAction\] | 
| Amazon Forecast Query Service | arn:aws:states:::aws\-sdk:forecastquery:\[apiAction\] | 
| Amazon Forecast Service | arn:aws:states:::aws\-sdk:forecast:\[apiAction\] | 
| Amazon Fraud Detector | arn:aws:states:::aws\-sdk:frauddetector:\[apiAction\] | 
| Amazon GameLift | arn:aws:states:::aws\-sdk:gamelift:\[apiAction\] | 
| Amazon S3 Glacier | arn:aws:states:::aws\-sdk:glacier:\[apiAction\] | 
| Amazon GuardDuty | arn:aws:states:::aws\-sdk:guardduty:\[apiAction\] | 
| Amazon HealthLake | arn:aws:states:::aws\-sdk:healthlake:\[apiAction\] | 
| Amazon Honeycode | arn:aws:states:::aws\-sdk:honeycode:\[apiAction\] | 
| Amazon Inspector | arn:aws:states:::aws\-sdk:inspector:\[apiAction\] | 
| Amazon Interactive Video Service | arn:aws:states:::aws\-sdk:ivs:\[apiAction\] | 
| Amazon Kendra | arn:aws:states:::aws\-sdk:kendra:\[apiAction\] | 
| Amazon Kinesis | arn:aws:states:::aws\-sdk:kinesis:\[apiAction\] | 
| Amazon Kinesis Analytics | arn:aws:states:::aws\-sdk:kinesisanalytics:\[apiAction\] | 
| Amazon Kinesis Analytics V2 | arn:aws:states:::aws\-sdk:kinesisanalyticsv2:\[apiAction\] | 
| Amazon Kinesis Firehose | arn:aws:states:::aws\-sdk:firehose:\[apiAction\] | 
| Amazon Kinesis Video Signaling Channels | arn:aws:states:::aws\-sdk:kinesisvideosignaling:\[apiAction\] | 
| Amazon Kinesis Video Streams | arn:aws:states:::aws\-sdk:kinesisvideo:\[apiAction\] | 
| Amazon Kinesis Video Streams \(Archived Media\) | arn:aws:states:::aws\-sdk:kinesisvideoarchivedmedia:\[apiAction\] | 
| Amazon Kinesis video stream | arn:aws:states:::aws\-sdk:kinesisvideomedia:\[apiAction\] | 
| Amazon Lex Model Building Service | arn:aws:states:::aws\-sdk:lexmodelbuilding:\[apiAction\] | 
| Amazon Lex Model Building Service V2 | arn:aws:states:::aws\-sdk:lexmodelsv2:\[apiAction\] | 
| Amazon Lex | arn:aws:states:::aws\-sdk:lexruntime:\[apiAction\] | 
| Amazon Lex Runtime V2 | arn:aws:states:::aws\-sdk:lexruntimev2:\[apiAction\] | 
| Amazon Lightsail | arn:aws:states:::aws\-sdk:lightsail:\[apiAction\] | 
| Amazon Location Service | arn:aws:states:::aws\-sdk:location:\[apiAction\] | 
| Amazon Lookout for Vision | arn:aws:states:::aws\-sdk:lookoutvision:\[apiAction\] | 
| Amazon MQ | arn:aws:states:::aws\-sdk:mq:\[apiAction\] | 
| Amazon Macie | arn:aws:states:::aws\-sdk:macie:\[apiAction\] | 
| Amazon Macie 2 | arn:aws:states:::aws\-sdk:macie2:\[apiAction\] | 
| Amazon Managed Blockchain | arn:aws:states:::aws\-sdk:managedblockchain:\[apiAction\] | 
| Amazon Managed Service for Prometheus | arn:aws:states:::aws\-sdk:amp:\[apiAction\] | 
| Amazon Managed Streaming for Apache Kafka | arn:aws:states:::aws\-sdk:kafka:\[apiAction\] | 
| Amazon Managed Workflows for Apache Airflow | arn:aws:states:::aws\-sdk:mwaa:\[apiAction\] | 
| Amazon Mechanical Turk | arn:aws:states:::aws\-sdk:mturk:\[apiAction\] | 
| Amazon Neptune | arn:aws:states:::aws\-sdk:neptune:\[apiAction\] | 
| Amazon Personalize | arn:aws:states:::aws\-sdk:personalize:\[apiAction\] | 
| Amazon Personalize Events | arn:aws:states:::aws\-sdk:personalizeevents:\[apiAction\] | 
| Amazon Personalize Runtime | arn:aws:states:::aws\-sdk:personalizeruntime:\[apiAction\] | 
| Amazon Pinpoint | arn:aws:states:::aws\-sdk:pinpoint:\[apiAction\] | 
| Amazon Pinpoint Email Service | arn:aws:states:::aws\-sdk:pinpointemail:\[apiAction\] | 
| Amazon Pinpoint SMS and Voice Service | arn:aws:states:::aws\-sdk:pinpointsmsvoice:\[apiAction\] | 
| Amazon Polly | arn:aws:states:::aws\-sdk:polly:\[apiAction\] | 
| Amazon QLDB | arn:aws:states:::aws\-sdk:qldb:\[apiAction\] | 
| Amazon QLDB Session | arn:aws:states:::aws\-sdk:qldbsession:\[apiAction\] | 
| Amazon QuickSight | arn:aws:states:::aws\-sdk:quicksight:\[apiAction\] | 
| Amazon Redshift | arn:aws:states:::aws\-sdk:redshift:\[apiAction\] | 
| Amazon Redshift Data API | arn:aws:states:::aws\-sdk:redshiftdata:\[apiAction\] | 
| Amazon Rekognition | arn:aws:states:::aws\-sdk:rekognition:\[apiAction\] | 
| Amazon Relational Database Service | arn:aws:states:::aws\-sdk:rds:\[apiAction\] | 
| Amazon Route 53 | arn:aws:states:::aws\-sdk:route53:\[apiAction\] | 
| Amazon Route 53 Domains | arn:aws:states:::aws\-sdk:route53domains:\[apiAction\] | 
| Amazon Route 53 Resolver | arn:aws:states:::aws\-sdk:route53resolver:\[apiAction\] | 
| Amazon S3 Control | arn:aws:states:::aws\-sdk:s3outposts:\[apiAction\] | 
| Amazon SageMaker Feature Store Runtime | arn:aws:states:::aws\-sdk:sagemakerfeaturestoreruntime:\[apiAction\] | 
| Amazon SageMaker Runtime | arn:aws:states:::aws\-sdk:sagemakerruntime:\[apiAction\] | 
| Amazon SageMaker | arn:aws:states:::aws\-sdk:sagemaker:\[apiAction\] | 
| Amazon SageMaker Edge Manager | arn:aws:states:::aws\-sdk:sagemakeredge:\[apiAction\] | 
| Amazon Simple Email Service | arn:aws:states:::aws\-sdk:ses:\[apiAction\] | 
| Amazon Simple Email Service V2 | arn:aws:states:::aws\-sdk:sesv2:\[apiAction\] | 
| Amazon Simple Notification Service | arn:aws:states:::aws\-sdk:sns:\[apiAction\] | 
| Amazon Simple Queue Service | arn:aws:states:::aws\-sdk:sqs:\[apiAction\] | 
| Amazon Simple Storage Service | arn:aws:states:::aws\-sdk:s3:\[apiAction\] | 
| Amazon Simple Workflow Service | arn:aws:states:::aws\-sdk:swf:\[apiAction\] | 
| Amazon Textract | arn:aws:states:::aws\-sdk:textract:\[apiAction\] | 
| Amazon Transcribe | arn:aws:states:::aws\-sdk:transcribe:\[apiAction\] | 
| Amazon Translate | arn:aws:states:::aws\-sdk:translate:\[apiAction\] | 
| Amazon WorkDocs | arn:aws:states:::aws\-sdk:workdocs:\[apiAction\] | 
| Amazon WorkMail | arn:aws:states:::aws\-sdk:workmail:\[apiAction\] | 
| Amazon WorkMail Message Flow | arn:aws:states:::aws\-sdk:workmailmessageflow:\[apiAction\] | 
| Amazon WorkSpaces | arn:aws:states:::aws\-sdk:workspaces:\[apiAction\] | 
| Amplify | arn:aws:states:::aws\-sdk:amplifybackend:\[apiAction\] | 
| Application Auto Scaling | arn:aws:states:::aws\-sdk:applicationautoscaling:\[apiAction\] | 
| Auto Scaling | arn:aws:states:::aws\-sdk:autoscaling:\[apiAction\] | 
| CodeArtifact | arn:aws:states:::aws\-sdk:codeartifact:\[apiAction\] | 
| DynamoDB Accelerator | arn:aws:states:::aws\-sdk:dax:\[apiAction\] | 
| EC2 Image Builder | arn:aws:states:::aws\-sdk:imagebuilder:\[apiAction\] | 
| Elastic Load Balancing | arn:aws:states:::aws\-sdk:elasticloadbalancing:\[apiAction\] | 
| Elastic Load Balancing V2 | arn:aws:states:::aws\-sdk:elasticloadbalancingv2:\[apiAction\] | 
| MediaConnect | arn:aws:states:::aws\-sdk:mediaconnect:\[apiAction\] | 
| Network Manager | arn:aws:states:::aws\-sdk:networkmanager:\[apiAction\] | 
| Amazon S3 Control | arn:aws:states:::aws\-sdk:s3control:\[apiAction\] | 
| Savings Plans | arn:aws:states:::aws\-sdk:savingsplans:\[apiAction\] | 
| Schemas | arn:aws:states:::aws\-sdk:schemas:\[apiAction\] | 
| Service Quotas | arn:aws:states:::aws\-sdk:servicequotas:\[apiAction\] | 
| Snowball | arn:aws:states:::aws\-sdk:snowball:\[apiAction\] | 