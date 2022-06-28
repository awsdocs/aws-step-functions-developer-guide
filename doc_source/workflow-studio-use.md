# Using Workflow Studio<a name="workflow-studio-use"></a>

Learn to create, edit and run workflows using Step Functions Workflow Studio\. After your workflow is ready, you can export it\. You can also use Workflow Studio for rapid prototyping\. 

## Create a workflow<a name="workflow-studio-components-create"></a>

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

1. Choose **Create state machine**\.

1. On the **Choose authoring method** page, choose **Design your workflow visually**\.  
![\[Step Functions console authoring method selection\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_main_03_graph_editor_create.png)

1.  Under **Type**, choose `Standard` or `Express`, and then choose **Next**\.

   `Standard` is recommended for your first state machine\.

1. Design your workflow using Workflow Studio, then choose **Next**\.
**Note**  
If you see errors in your workflow, you can stay in Workflow Studio and correct the errors, or you can proceed to the next step and correct the errors in code\.

1. On the **Review generated code** page, you can view the generated Amazon States Language definition of your workflow\. If you want to make changes, you can do this in the **Definition** area\. You can also access code snippets under **Generate code snippet**\. 

1. Choose **Next**\.

1. In the **Name** box, enter a name for your state machine, for example, `MyStateMachine`\.
**Note**  
State machine, execution, and activity names must be 1–80 characters in length, must be unique for your account and AWS Region, and must not contain any of the following:  
Whitespace
Wildcard characters \(`? *`\)
Bracket characters \(`< > { } [ ]`\)
Special characters \(`: ; , \ | ^ ~ $ # % & ` "`\)
Control characters \(`\\u0000` \- `\\u001f` or `\\u007f` \- `\\u009f`\)\.
Step Functions allows you to create state machine, execution, and activity names that contain non\-ASCII characters\. These non\-ASCII names don't work with Amazon CloudWatch\. To ensure that you can track CloudWatch metrics, choose a name that uses only ASCII characters\.

1. In **Execution role**, under the **Permissions** section, choose **Create new role**\.

1. Choose **Create state machine**\.

You can now run your new workflow\.

## Design a workflow<a name="workflow-studio-build"></a>

If you know the name of the state you want to add, use the search box at the top of the states browser to find that state in the **Actions** and **Flow** panels\.

![\[Step Functions Workflow Studio search\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-design-01.png)

Otherwise, choose a state from the states browser and drag and drop it onto the canvas, placing it where you want in your workflow\. You can also re\-order states in your workflow by dragging them to a different location in your workflow\. As you drag a state onto the canvas, a line appears wherever you can drop it in your workflow\. After a state is dropped onto the canvas, its code is auto\-generated and added inside your workflow definition\.

![\[Step Functions Workflow Studio add state to canvas\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-design-02.png)

After you drop a state onto the canvas, you can configure it using the **Form** panel on the right\. This panel further contains the **Configuration**, **Input**, **Output**, and **Error Handling** tabs for each of the state or API action that you place on the canvas\. For example, the **Configuration** tab for Lambda Invoke API action consists of the following sections:

![\[Configuration options\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_config_01.png)

1. The **State name** identifies the state\. You can use your own name or accept the default generated name\.

1. The **API** shows which API the action uses\. 

1. The **Integration type** dropdown list provides options to choose the type of service integrations available in Step Functions\. The integration type you choose is used to call API actions of a specific AWS service from your workflow\.

1. The **Function name** provides options to:
   +  **Enter a function name**: You can enter your function name or its ARN\. 
   +  **Get function name at runtime from state input**: You can use this option to dynamically get the function name from the state input based on the path you specify\. 
   +  **Select function name**: You can directly select from the functions available in your account and region\. 

1. The **Payload** lets you select from the following:
   + **Use state input as payload** You can use this option to pass the state’s input as the payload provided to your Lambda function\.
   + **Enter your own payload** ou can use this option to construct a JSON object to pass as the payload to your Lambda function\. This JSON can include both static values and values selected from the state input\. 
   + **No payload** You can use this option if you don’t want to pass any payload to your Lambda function\.

1. **Next state** lets you to select the state you want to transition to next\. 

1. \(optional\) The **Comment** field can be used to add your own comment\. It will not affect the workflow, but can be used to annotate your workflow\.

1. \(optional\) Some states will have an option to select **Wait for task to complete** or **Wait for callback**\. When available, these options select one of the following [service integration patterns](connect-to-resource.md): 
   + **No option selected**: Step Functions will use the [Request Response](connect-to-resource.md#connect-default) integration pattern\. Step Functions will wait for an HTTP response and then progress to the next state\. Step Functions will not wait for a job to complete\. When no options are available, the state will use this pattern\. 
   + **Wait for task to complete**: Step Functions will use the [Run a Job \(\.sync\)](connect-to-resource.md#connect-sync) integration pattern\.
   + **Wait for callback**: Step Functions will use the [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) integration pattern\.

Some states will have more generic configuration options\. For example, the Amazon ECS `RunTask` state configuration contains an `API Parameters` field populated with placeholder values\.

![\[Configuration options\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-design-04.png)

For these states, you can replace the placeholder values with configurations that are suited to your needs\.

To delete a state, you can use backspace, right\-click and choose **Delete state** or use the **Form** and choose **Delete state**\.

![\[Delete state\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-delete-01.png)

As your workflow grows, it may not fit in the canvas\. You can: 

1. Use the controls on the side panels to resize or close the panels\.

1. Use the controls at the top of the canvas to zoom the workflow graph in or out\.

![\[Canvas controls\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-canvas-zoom-01.png)

## Run your workflow<a name="workflow-studio-components-create-run"></a>

After you create or edit your workflow with the Workflow Studio, you can run it and view its execution in the [Step Functions console](https://console.aws.amazon.com/states/home):

1. Choose your workflow in the Step Functions console\.

1. Choose **Start execution**\. The **Start execution** dialog box is displayed\.

1. Choose **Start execution** to run the workflow\.

1. You can see the execution results for each state in the **Graph inspector**\. Choose each step to view details about its execution\.

## Edit your workflow<a name="workflow-studio-components-create-edit"></a>

You can edit an existing workflow visually using Workflow Studio, or you can edit an existing workflow in code\. To edit an existing workflow:

1. Choose the workflow you want to edit\.

1. Choose **Edit**\.  
![\[Edit workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_main_03_graph_editor_edit.png)

1. The state machine has two panes: a code pane and a visual workflow pane\. Choose **Workflow Studio** in the visual workflow pane to edit your workflow with Workflow Studio\. When you are done, choose **Apply and exit** to save your changes and exit\.
**Note**  
If you see errors in your workflow, you can stay in Workflow Studio and correct the errors, or you can proceed to the next step and correct the errors in code\.

1. \(Optional\) Use the code pane to edit the workflow definition in code\.

1. If you have added new resources to your workflow, in **Permissions**, choose **Create new role** or use a role that allows additional access to these resources\.

1. Choose **Save** to save your updated workflow\.

## Export your workflow<a name="workflow-studio-components-create-export"></a>

You can export your workflow's Amazon States Language code, and also a graph of your workflow: 

1.  Choose your workflow in the Step Functions console\. 

1.  \(optional\) Choose **Edit**\. 

1.  Choose **Definition**\. 

1. In the workflow definition:
   + To export your workflow's code, select it, then copy it to its destination\.
   + To export the graph of your workflow to an SVG or PNG file, choose **Export** and select the format you want use\.

## Prototype a workflow<a name="workflow-studio-components-create-prototype"></a>

You can use Workflow Studio to prototype new workflows that contain placeholder resources\. To create a prototype:

1. Sign in to the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/)\.

1. On the **Choose authoring method** page, choose **Design your workflow visually**\.

1. Under **Type**, choose `Standard` or `Express`, then choose **Next**\.

1. Design your workflow using Workflow Studio\. To include placeholder resources: 

   1. Choose the state for which you want to include a placeholder resource:
      + For Lambda Invoke states, under **Configuration**, choose **Function name**, then choose **Enter function name**\. Enter a custom name for your function\.
      + For Amazon SQS Send Message states, under **Configuration**, choose **Queue URL**, then choose **Enter queue URL**\. Enter a placeholder queue URL\.
      + For all other states listed under **Actions**, you can use the default configuration\.

   1.  Choose **Next**\. 
**Note**  
If you see errors in your workflow, you can stay in Workflow Studio and correct the errors, or you can proceed to the next step and correct the errors in code\.

   1.  On the **Review generated code** page, you can view the generated Amazon States Language definition of your workflow\. If needed, make any changes under **Definition**, then choose **Next**\.

   1. Specify your workflow settings, then choose **Create state machine**\. 

You now have created a new workflow with placeholder resources that can be used to prototype\. You can export your workflow definition and the graph of your workflow\.
+ To export your workflow definition, select and copy it to your clipboard\. You can paste this definition and use it as the starting point for local development with the [AWS Toolkit for Visual Studio Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/bulding-stepfunctions.html)\.
+ To export the graph of your workflow to an SVG or PNG file, choose **Export** and choose the format you want to use\.