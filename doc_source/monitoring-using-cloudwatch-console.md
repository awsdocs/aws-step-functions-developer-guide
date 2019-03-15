# Viewing Metrics for Step Functions<a name="monitoring-using-cloudwatch-console"></a>

1. Sign in to the AWS Management Console and open the CloudWatch console\.

1. Choose **Metrics**, and on the **All Metrics** tab, choose **States**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-states.png)

   If you ran any executions recently, you will see up to three types of metrics:
   +  **Execution Metrics** 
   +  **Activity Function Metrics** 
   +  **Lambda Function Metrics** 

1. Choose a metric type to see a list of metrics\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-list-metrics.png)
   + To sort your metrics by **Metric Name** or **StateMachineArn**, use the column headings\.
   + To view graphs for a metric, choose the box next to the metric on the list\. You can change the graph parameters using the time range controls above the graph view\.

     You can choose custom time ranges using relative or absolute values \(specific days and times\)\. You can also use the dropdown list to display values as lines, stacked areas, or numbers \(values\)\.
   + To view the details about a graph, hover over the metric color code that appears below the graph\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-metrics-legend.png)

     The metric's details are displayed\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-cloudwatch-monitoring-metrics-detail.png)

For more information about working with CloudWatch metrics, see [Using Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/working_with_metrics.html) in the *Amazon CloudWatch User Guide*\.