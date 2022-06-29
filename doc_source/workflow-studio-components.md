# Interface overview<a name="workflow-studio-components"></a>

Workflow Studio for AWS Step Functions is a low\-code visual workflow designer for Step Functions that lets you create serverless workflows by orchestrating AWS services\.

Get to know Workflow Studio with an overview of the interface components: The states browser, the canvas, and the inspector panel\.

![\[Step Functions Workflow Studio components\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_main_01.png)

1. The **States browser** contains two panels\. The **Actions** panel provides a list of AWS APIs that you can drag and drop into your workflow graph in the canvas\. Each Action represents a [Task](amazon-states-language-task-state.md) state\. The **Flow** panel provides a list of flow states that you can drag and drop into your workflow graph in the canvas\. 

1. The *canvas* is where you drag and drop states into your workflow graph, change the order of states, and select states to configure or view\.

1. The **Inspector** panel is where you can choose the **Form**, which lets you view and edit the properties of any state you select, and the **Definition**, where you can view the Amazon States Language code for your workflow, with the currently selected state highlighted\. 

1. **Info** links open a panel with contextual information when you need help\.  These panels also include links to related topics in the Step Functions documentation\. 

## States browser<a name="workflow-studio-components"></a>

The **States browser** is where you select states to drag and drop into your workflow graph\. The **Actions** panel provides a list of AWS APIs, and the **Flow** panel provides a list of flow states\. You can search all states in the **States Browser** using the search field at the top\.

![\[Step Functions Workflow Studio States browser\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-states-browser-01.png)

There are seven flow states that you can use to direct and control your workflow\. All of them take input from the previous state, and many let you filter the input from the preceding state, and the output to the state that follows\. The flow states are:
+ `Choice`: Add a choice between branches of execution to your workflow\. In the **Configuration** tab of the Inspector, you can configure rules to determine which state the workflow will transition to\.
+ `Parallel`: Add parallel branches of execution to your workflow\. 
+ `Map`: Dynamically iterate steps for each element of an input array\. Unlike a `Parallel` flow state, a `Map` state will execute the same steps for multiple entries of an array in the state input\.
+ `Pass`: Lets you pass its input to its output\. \(Optional\) You can add fixed data into the output\.
+ `Wait`: Have your workflow pause for a certain amount of time or until a specified time or date\. 
+ `Succeed`: Stops your workflow with a success\. 
+ `Fail`: Stops your workflow with a failure\. 

## Canvas<a name="workflow-studio-components-grapheditor"></a>

After you choose a state to add to your workflow, drag it to to the canvas and drop it into your workflow graph\. You can also drag and drop states to move them to different places in your workflow\. If your workflow is complex, you may not be able to view all of it in the canvas panel\. Use the controls at the top of the canvas to zoom in or out\. To view different parts of a workflow graph, you can drag the workflow graph in the canvas\. 

Drag a workflow state from the `Actions` or `Flow` panel and drop it into your workflow\. A line shows where it will be placed in your workflow\. The new workflow state has been added to your workflow, and its code is auto\-generated\.

![\[Workflow Studio canvas\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_main_03_graph_editor_add.png)

To change the order of a state, you can drag it to a different place in your workflow\.

![\[Workflow Studio drag drop\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfs_main_03_graph_editor_move.png)

## Inspector<a name="workflow-studio-components-formdefinition"></a>

After you have added a state to your workflow, you will want to configure it\. Choose the state you want to configure, and you will see its configuration options in the Inspector panel\. You can also see the workflow code by choosing the **Definition** panel\. The code associated with the state you have selected will be highlighted\.

![\[Workflow Studio inspector configuration panel\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-forms-definitions-01.png)

![\[Workflow Studio inspector definition panel\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/wfe-forms-definitions-02.png)

## Keyboard shortcuts<a name="workflow-studio-components-key-shortcuts"></a>

Workflow Studio supports the following keyboard shortcuts:


| Keyboard shortcut | Function | 
| --- | --- | 
| Ctrl\+Z | Undo the last operation | 
| Ctrl\+Shift\+Z | Redo the last operation | 
| Alt\+C | Center the workflow in the canvas | 
| Backspace | Remove all selected states | 
| Delete | Remove all selected states | 
| Ctrl\+D | Duplicate selected state | 