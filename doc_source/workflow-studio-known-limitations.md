# Known limitations when using Workflow Studio<a name="workflow-studio-known-limitations"></a>

When working with Workflow Studio, there are several limitations that you should be aware of\.

## Workflow Studio does not support dynamic resource IDs<a name="workflow-studio-known-limitations-dynamic-resource-id"></a>

You cannot use dynamic ARNs in the `Resource` section of a [Task](amazon-states-language-task-state.md) state\. Instead, you can enter the dynamic ARN in the appropriate `Parameters` field of your task state\. For example, if you have the dynamic ARN `${my-input}`, you could add to the ` ` field as seen in the following:

![\[Dynamic resource IDs.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-dyn_ids.png)

**Note**  
State machine definitions that contain AWS CloudFormation references cannot be saved\. However, you can copy or export the definition for use elsewhere\.

## Workflow Studio does not automatically save workflows<a name="workflow-studio-known-limitations-browser-autosave"></a>

Currently, Workflow Studio doesn't automatically save your workflows\. In case of an AWS console timeout, you could lose your work\. Therefore, we recommend that you save your workflows or export their Amazon States Language definition frequently\. 

On the console, you can save a workflow after creating it or save the updates made to an existing workflow on its edit page\. In either scenarios, your state machine definition must be valid to save it\.

For information about console session expiry, see [AWS Management Console FAQs](http://aws.amazon.com/console/faq-console/)\.

## Workflow Studio does not support Internet Explorer 11<a name="workflow-studio-known-limitations-browser"></a>

Internet Explorer 11 is not supported by Workflow Studio\. If you are you using Internet Explorer 11 and encounter issues using Workflow Studio, try using a different browser\.