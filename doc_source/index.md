# AWS Step Functions Developer Guide

-----
*****Copyright &copy; 2018 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is AWS Step Functions?](welcome.md)
+ [Getting Started](getting-started.md)
+ [Tutorials](tutorials.md)
   + [Development Options](development-options.md)
   + [Creating a Lambda State Machine](tutorial-creating-lambda-state-machine.md)
   + [Creating a Lambda State Machine Using AWS CloudFormation](tutorial-lambda-state-machine-cloudformation.md)
   + [Creating an Activity State Machine](tutorial-creating-activity-state-machine.md)
   + [Handling Error Conditions Using a State Machine](tutorial-handling-error-conditions.md)
   + [Starting a State Machine Execution Using CloudWatch Events](tutorial-cloudwatch-events-target.md)
   + [Creating a Step Functions API Using API Gateway](tutorial-api-gateway.md)
   + [Iterating a Loop Using Lambda](tutorial-create-iterate-pattern-section.md)
+ [How Step Functions Works](how-step-functions-works.md)
   + [Templates](concepts-templates.md)
   + [Sample Projects](create-sample-projects.md)
      + [Job Status Poller](job-status-poller-sample.md)
      + [Task Timer](task-timer-sample.md)
   + [States](concepts-states.md)
   + [Tasks](concepts-tasks.md)
   + [Activities](concepts-activities.md)
      + [Example Activity Worker in Ruby](example-ruby-activity-worker.md)
   + [Transitions](concepts-transitions.md)
   + [State Machine Data](concepts-state-machine-data.md)
   + [Executions](concepts-state-machine-executions.md)
   + [Error Handling](concepts-error-handling.md)
   + [Read Consistency](concepts-read-consistency.md)
+ [Amazon States Language](concepts-amazon-states-language.md)
   + [State Machine Structure](amazon-states-language-state-machine-structure.md)
   + [States](amazon-states-language-states.md)
      + [Pass](amazon-states-language-pass-state.md)
      + [Task](amazon-states-language-task-state.md)
      + [Choice](amazon-states-language-choice-state.md)
      + [Wait](amazon-states-language-wait-state.md)
      + [Succeed](amazon-states-language-succeed-state.md)
      + [Fail](amazon-states-language-fail-state.md)
      + [Parallel](amazon-states-language-parallel-state.md)
   + [Input and Output Processing](amazon-states-language-input-output-processing.md)
   + [Errors](amazon-states-language-errors.md)
+ [Best Practices for Step Functions](sfn-best-practices.md)
   + [Use Timeouts to Avoid Stuck Executions](sfn-stuck-execution.md)
   + [Use ARNs Instead of Passing Large Payloads](avoid-exec-failures.md)
   + [Iterate a Count to Avoid Reaching the History Limit](bp-history-limit.md)
+ [Limits](limits.md)
+ [Monitoring and Logging](monitoring-logging.md)
   + [Monitoring Step Functions Using CloudWatch](procedure-cw-metrics.md)
      + [Viewing Metrics for Step Functions](monitoring-using-cloudwatch-console.md)
      + [Setting Alarms for Step Functions](monitoring-using-cloudwatch-console-set-alarm.md)
   + [Logging Step Functions using CloudTrail](procedure-cloud-trail.md)
+ [Security](security.md)
   + [Creating IAM Roles for AWS Step Functions](procedure-create-iam-role.md)
   + [Creating Granular IAM Permissions for Non-Admin Users](concept-create-iam-advanced.md)
+ [Document History](document-history.md)
+ [AWS Glossary](glossary.md)