# Manage AWS Glue DataBrew Jobs with Step Functions<a name="connect-databrew"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information about working with AWS Step Functions and its integrations, see the following:
+ [Working with other services](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

You can use the DataBrew integration to add data cleaning and data normalization steps into your analytics and machine learning workflows\.

Supported DataBrew API:
+ `[https://docs.aws.amazon.com/databrew/latest/dg/API_StartJobRun.html](https://docs.aws.amazon.com/databrew/latest/dg/API_StartJobRun.html)`

The following includes a `Task` state that starts a request\-response DataBrew job\.

```
"DataBrew StartJobRun": {
            "Type": "Task",
            "Resource": "arn:aws:states:::databrew:startJobRun",
            "Parameters": {
               "Name": "sample-proj-job-1"
            },
            "Next": "NEXT_STATE"
          },
```

The following includes a `Task` state that starts a sync DataBrew job\.

```
"DataBrew StartJobRun": {
           "Type": "Task",
           "Resource": "arn:aws:states:::databrew:startJobRun.sync",
           "Parameters": {
              "Name": "sample-proj-job-1"
           },
           "Next": "NEXT_STATE"
          },
```

For information on how to configure IAM when using Step Functions with other AWS services, see [IAM Policies for integrated services](service-integration-iam-templates.md)\.