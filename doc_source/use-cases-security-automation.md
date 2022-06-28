# IT and security automation<a name="use-cases-security-automation"></a>

IT automation can help manage increasingly complex and time\-consuming operations, such as upgrading and patching software, deploying security updates to address vulnerabilities, selecting infrastructure, synchronizing data, routing support tickets, and more\. The automation of repetitive and time\-consuming tasks can allow your organization to complete routine operations quickly and consistently on a large scale\. This lets you focus on strategic work such as feature development, complex support requests, and innovation while meeting these growing demands\.

Step Functions allows you to create workflows that automatically scale to meet the needs of your business without requiring manual intervention\. In cases where an error occurs in your workflow, it often does not require manual intervention\. Step Functions lets you automatically [retry failed tasks](concepts-error-handling.md#error-handling-retrying-after-an-error) and [an exponential backoff](concepts-error-handling.md#error-handling-examples) that can manage errors in your workflow\. 

There can be situations where human intervention is required before the workflow can progress\. For example, approving a substantial credit increase may require human approval\. To manage this, you can define branching logic in Step Functions, so that only requests over a defined amount require human approval, while all other requests are automatically completed\. In cases where human approval is required, Step Functions lets you pause the workflow at a specific step, wait for a response, and then continue the workflow once the response is received\.

Some examples of the types of automation workflows that customers use Step Functions for include:

**IT automation**
+ Auto\-remediate incidents like opening an SSH port, low disk space, or when a public access is given to a Amazon S3 bucket\.
+ Automate the deployment of AWS CloudFormation StackSets 

**Security automation**
+ Automate the response to a scenario where an IAM user and user access key has been exposed\.
+ Auto\-remediate security incident responses according to policy actions defined such as restricting actions to specific ARNs or applying other actions\. 
+ Warn employees of phishing emails within seconds of receipt\. 

**Human Approval**
+ Automate the training of machine learning model and then require manual approval of the model by a data scientist before then automatically deploying or rejecting the model based upon the response received\. 
+ Automate the routing of customer feedback received based on sentiment analysis so that those with a negative sentiment are immediately escalated for manual review\. 