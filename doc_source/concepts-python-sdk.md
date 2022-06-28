# AWS Step Functions Data Science SDK for Python<a name="concepts-python-sdk"></a>

The AWS Step Functions Data Science SDK is an open source library for data scientists\. With this SDK, you can create workflows that process and publish machine learning models using SageMaker and Step Functions\. You can also create multi\-step machine learning workflows in Python that orchestrate AWS infrastructure at scale, without having to provision and integrate the AWS services separately\.

The AWS Step Functions Data Science SDK provides a Python API that can create and invoke Step Functions workflows\. You can manage and execute these workflows directly in Python, as well as Jupyter notebooks\.

In addition to creating production\-ready workflows directly in Python, the AWS Step Functions Data Science SDK allows you to copy that workflow, experiment with new options, and then put the refined workflow into production\.

For more information about the AWS Step Functions Data Science SDK, see the following: 
+ [Project on Github](https://github.com/aws/aws-step-functions-data-science-sdk-python)
+ [SDK documentation](https://aws-step-functions-data-science-sdk.readthedocs.io/)
+ The following [Example notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-nbexamples.html), which are available in Jupyter notebook instances in the [SageMaker console](https://console.aws.amazon.com/sagemaker/) and the related [GitHub project](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/step-functions-data-science-sdk):
  + `hello_world_workflow.ipynb`
  + `machine_learning_workflow_abalone.ipynb`
  + `training_pipeline_pytorch_mnist.ipynb`