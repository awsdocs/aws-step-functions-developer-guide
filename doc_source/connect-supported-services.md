# Optimized integrations for Step Functions<a name="connect-supported-services"></a>

The following topics include the supported APIs, parameters, and request/response syntax in the Amazon States Language for coordinating other AWS services\. The topics also provide example code\. You can call Optimized integrations services directly from the Amazon States Language in the `Resource` field of a task state\. 

You can use three service integration patterns:
+ [Default response](connect-to-resource.md#connect-default)
+ [Wait for a job to complete \(`.sync`\)\.](connect-to-resource.md#connect-sync)
+ [Wait for a task token \(`.waitForTaskToken`\)\.](connect-to-resource.md#connect-wait-token)

 Standard Workflows and Express Workflows support the same integrations but do not support the same integration patterns\. Express Workflows do not support Run a Job \(\.sync\) or Wait for Callback \(\.waitForTaskToken\)\. Optimized integrations pattern support is different for each integration\. For more information, see [Standard vs\. Express Workflows](concepts-standard-vs-express.md)\.

------
#### [ Standard Workflows ]


**Supported service integrations**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/connect-supported-services.html)

------
#### [ Express Workflows ]


**Supported service integrations**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/connect-supported-services.html)

------