# Tagging<a name="concepts-tagging"></a>

AWS Step Functions supports tagging of state machines and activities\. This can help you track and manage the costs associated with your resources, and provide better security in your IAM policies\. Tagging AWS Step Functions resources allows them to be managed by AWS Resource Groups\. For more information on Resource Groups, see the [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/) User Guide\.

To review the restrictions related to resource tagging, see [Restrictions Related to Tagging](limits.md#sfn-limits-tagging)\.

**Topics**
+ [Tagging for Cost Allocation](#tagging-cost)
+ [Tagging for Security](#tagging-security)
+ [Viewing and Managing](#tagging-console)
+ [Tagging API](#tagging-api)

## Tagging for Cost Allocation<a name="tagging-cost"></a>

To organize and identify your Step Functions resources for cost allocation, you can add metadata *tags* that identify the purpose of a state machine or activity\. This is especially useful when you have many resources\. You can use cost allocation tags to organize your AWS bill to reflect your own cost structure\. To do this, sign up to get your AWS account bill to include the tag keys and values\. For more information, see [Setting Up a Monthly Cost Allocation Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html#allocation-report) in the *AWS Billing and Cost Management User Guide*\.

For instance, you could add tags that represent the cost center and purpose of your Step Functions resources:


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/step-functions/latest/dg/concepts-tagging.html)

This tagging scheme allows you to group two state machines performing related tasks in the same cost center, while tagging an unrelated activity with a different cost allocation tag\.

## Tagging for Security<a name="tagging-security"></a>

IAM supports controlling access to resources based on tags\. To control access based on tags, provide information about your resource tags in the condition element of an IAM policy\.

For instance, you could restrict access to all Step Functions resources that include a tag with the key `environment` and the value `production`:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "states:TagResource",
                "states:DeleteActivity",
                "states:DeleteStateMachine",
                "states:StopExecution"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {"aws:ResourceTag/environment": "production"}
            }
        }
    ]
}
```

For more information, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the IAM User Guide\.

## Viewing and Managing Tags in the Step Functions Console<a name="tagging-console"></a>

AWS Step Functions allows you to view and manage the tags for your state machines in the Step Functions console\. From the **Details** page of a state machine, select **Tags**\. Here, you can view the existing tags associated with your state machine\.

**Note**  
To manage tags for activites, see [Manage Tags With Step Functions API Actions\.](#tagging-api)\.

 To add or delete tags that are associated with your state machine, select the **Manage Tags** button\.

1. Browse to the details page of a state machine\.

1. Select **Tags**, next to **Executions** and **Definition**\.  
![\[Select Tags on Details page\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tagging-select-tab.png)![\[Select Tags on Details page\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Select Tags on Details page\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

1. Choose **Manage tags**\.

1. 
   + To modify existing tags, edit the **Key** and **Value**\.
   + To remove existing tags, select **Remove tag**\.
   + To add a new tag, select **Add tag** and enter a **Key** and **Value**\.  
![\[Manage Tags\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tagging-manage.png)![\[Manage Tags\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Manage Tags\]](http://docs.aws.amazon.com/step-functions/latest/dg/)

1. Choose **Save**\.

## Manage Tags With Step Functions API Actions\.<a name="tagging-api"></a>

To manage tags using the Step Functions API, use the following API actions:
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListTagsForResource.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListTagsForResource.html)
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_TagResource.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_TagResource.html)
+ [https://docs.aws.amazon.com/step-functions/latest/apireference/API_UntagResource.html](https://docs.aws.amazon.com/step-functions/latest/apireference/API_UntagResource.html)