# Setting Alarms for Step Functions<a name="monitoring-using-cloudwatch-console-set-alarm"></a>

You can use CloudWatch alarms to perform actions\. For example, if you want to know when an alarm threshold is reached, you can set an alarm to send a notification to an Amazon SNS topic or to send an email when the `StateMachinesFailed` metric rises above a certain threshold\.

## To set an alarm on a metric<a name="to-set-an-alarm-on-a-metric"></a>

1. Sign in to the AWS Management Console and open the CloudWatch console\.

1. Choose **Metrics**, and on the **All Metrics** tab, choose **States**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-states.png)

   If you ran any executions recently, you will see up to three types of metrics:
   +  **Execution Metrics** 
   +  **Activity Function Metrics** 
   +  **Lambda Function Metrics** 

1. Choose a metric type to see a list of metrics\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-list-metrics.png)

1. Choose a metric, and then choose **Graphed metrics**\.

1. Choose ![\[bell\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-bell.png)![\[bell\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[bell\]](http://docs.aws.amazon.com/step-functions/latest/dg/) next to a metric on the list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-setting-alarms-graph.png)

   The **Create Alarm** page is displayed\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-setting-alarms-define-alarm.png)

1. Enter the values for the **Alarm threshold** and **Actions**, and then choose **Create Alarm**\.

For more information about setting and using CloudWatch alarms, see [Creating Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.