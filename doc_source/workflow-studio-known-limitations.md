# Known limitations when using Workflow Studio<a name="workflow-studio-known-limitations"></a>

When working with Workflow Studio, there are several limitations that you should be aware of\.

## Workflow Studio does not support dynamic resource IDs<a name="workflow-studio-known-limitations-dynamic-resource-id"></a>

You cannot use dynamic ARNs in the `Resource` section of a [Task](amazon-states-language-task-state.md) state\. Instead, you can enter the dynamic ARN in the appropriate `Parameters` field of your task state\. For example, if you have the dynamic ARN `${my-input}`, you could add to the ` ` field as seen in the following:

![\[Dynamic resource IDs.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-dyn_ids.png)

**Note**  
State machine definitions that contain AWS CloudFormation references cannot be saved\. However, you can copy or export the definition for use elsewhere\.

## A task token is not added to the state definition when you select `Wait for callback` in Workflow Studio<a name="workflow-studio-known-limitations-tasktoken"></a>

 When you select **Wait for callback**, Workflow Studio adds `.waitForTaskToken` to the resource, but no task token is added to the state definition\. To add a task token: 

1. On the **Configuration** tab, make sure the **Wait for a callback** option is selected\.

1.  Finish editing your state machine in Workflow Studio, then choose **Next** if you're creating a new state machine, or choose **Apply and exit** if you're editing an existing state machine\. 

1.  Choose **Generate code snippet**, then choose the task state for which you want to use **Wait for callback**\. 

1.  Choose **Wait for a callback with task token**, then copy the added text\. This text will be similar to `"TaskToken.$": "$$.Task.Token"`\. 

1.  Paste this text in the appropriate field in the editor\. 

You can now finish creating or updating your state machine\.

## Some service states are not available in Workflow Studio<a name="workflow-studio-known-limitations-missing-states"></a>

Some service states are not currently available in Workflow Studio\. Instead, after you creat or edit your state machine in Workflow Studio, you can use [code snippets](concepts-code-snippets.md) to add the state to your state machine definition\. Code snippets can be found in the drop down menu on your state machine's code pane\.

![\[Dynamic resource IDs.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-snippets.png)

## Workflow Studio does not support Internet Explorer 11<a name="workflow-studio-known-limitations-browser"></a>

Internet Explorer 11 is not supported by Workflow Studio\. If you are you using Internet Explorer 11 and encounter issues using Workflow Studio, try using a different browser\.