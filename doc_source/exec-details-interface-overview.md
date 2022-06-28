# Execution Details page – Interface overview<a name="exec-details-interface-overview"></a>

You can find the details of all your in\-progress and past state machine executions for Standard Workflows on the *Execution Details* page\. If you specified an execution ID while starting your execution, this page is titled with that execution ID\. Otherwise, it’s titled with the unique execution ID that Step Functions automatically generates for you\.

Besides the execution metrics, the *Execution Details* page provides the following options for managing your state machine's execution:


| Button | Choose this button to: | 
| --- | --- | 
| New execution page | Toggle between the new and old layout of the Execution Details page\. | 
| Edit state machine | Edit your state machine's Amazon States Language definition\. | 
| Export | Export the execution details in JSON format to share it with others or perform offline analysis\. | 
| New execution | Start a new execution of your state machine\. | 
| Stop execution | Stop the ongoing execution of your state machine\. | 

The *Execution Details* console page contains the following sections:

![\[Step Functions' Execution Details page interface displaying the three sections: Execution summary, Workflow views, and Split panel. Execution summary section displays a high-level execution summary for a completed state machine execution. The Graph view tab is selected in the View mode section. In the Graph view, the GetActivityTask step is selected in the state machine workflow while the Step Details on the right displays the selected step's input and output details.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-details-interface.png)

1. [Execution summary](#exec-details-intf-exec-summ)

1. [View mode](#exec-details-intf-sm-wf-view)

1. [Step details](#exec-details-intf-step-details)

## Execution summary<a name="exec-details-intf-exec-summ"></a>

The *Execution summary* section appears at the top of the *Execution Details* page\. This section provides you with the high\-level execution details of your workflow\. This information is divided between the following three tabs:

**Details**  
Shows information, such as the execution's status, ARN, and timestamps for execution start and end time\. You can also view the total count of **State transitions** that occurred while executing your state machine\. In addition, you can view the links for **X\-Ray trace map** and Amazon CloudWatch **Execution Logs** if you enabled tracing or logs for your state machine\.  
If your state machine execution was triggered by another state machine, you can view the link for the parent state machine on this tab\.

**Execution input & output**  
Shows the state machine execution input and output side\-by\-side\.

**Definition**  
Shows the state machine's Amazon States Language definition\.

**Note**  
If your state machine execution failed, the *Execution Details* page displays an error message\. Choose **Cause** or **Show step detail** on the error message to view the reason for execution failure or the step that caused the error\.  
If you choose **Show step detail**, the *[Step detail](#exec-details-intf-step-details)* section displays the **Input & Output** tab for the step that caused the error\. Additionally, the erroneous step appears highlighted in the **Graph view** and **Table view**\.

![\[Execution summary section of the Execution Details page interface. Details tab is selected. An AWS Lambda error message is displayed for the step GetListOfFruits.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-execution-summary.png)

## View mode<a name="exec-details-intf-sm-wf-view"></a>

The *View mode* section contains three different visualizations for your state machine\. You can choose to view a graphic representation of the workflow, a table outlining the states in your workflow, or a list of the events associated with your state machine's execution:

**Note**  
Choose a tab to view its contents\.

------
#### [ Graph view ]

The **Graph view** mode displays a graphical representation of your workflow\. A legend is included at the bottom that indicates the execution status of the state machine\. It also contains buttons that let you zoom in, zoom out, center the full workflow, or view the workflow in full\-screen mode\.

![\[Graph view of the View mode section. A step named GetActivityTask is selected in the workflow.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-details-graph-view.png)

From this view, you can choose any step in your workflow to view details about its execution in the *[Step details](#exec-details-intf-step-details)* component\. Further, the step that you choose in the **Graph view** gets selected in the **Table view**, as well as the other way around\.

If your state machine contains a `Map` state, `Parallel` state, or both, you can view their names in the workflow in the **Graph view**\. In addition, for the `Map` state, the **Graph view** lets you move across different iterations of the **Map** state execution data\. For example, if your **Map** state has five iterations and you want to view the execution data for the third and fourth iterations, do the following:

1. Choose the **Map** state whose iteration data you want to view\.

1. From **Map iteration viewer**, choose **\#2** from the dropdown list for third iteration because iterations are counted from zero\. Similarly, choose **\#3** from the dropdown list for the fourth iteration\.

   Alternatively, use the ![\[up arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/map_state_iteration_up_arrow_icon.png)![\[up arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[up arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/) and ![\[down arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/map_state_iteration_down_arrow_icon.png)![\[down arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[down arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/) arrow keys to move across different iterations of the **Map** state\.
**Note**  
If your state machine contains nested `Map` states, the dropdown lists for the parent and child `Map` state iterations will be displayed as shown in the following example:  

![\[Graph view mode section's Map iteration viewer, displaying a series of dropdown lists representing the iteration data for nested Map states.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-view-mode-nested-map-states-iterations.png)

1. \(Optional\) If one or more of your `Map` state iterations failed to execute, or the execution was stopped, you can view their data by choosing those iteration numbers under **Failed** or **Aborted** in the dropdown list\.

Finally, you can use the **Export** and **Layout** buttons to export the workflow graph as an SVG or PNG image or switch between horizontal and vertical views of your workflow\.

------
#### [ Table view ]

The **Table view** mode displays a tabular representation of the states in your workflow\. In this *View mode*, you can see the details of each state that was executed in your workflow, including its name, the name of any resource it used \(such as an AWS Lambda function\), and if the state executed successfully\.

From this view, you can choose any state in your workflow to view details about its execution in the *[Step details](#exec-details-intf-step-details)* component\. Further, the state that you choose in the **Table view** gets selected in the **Graph view**, as well as the other way around\.

You can also limit the amount of data displayed in the **Table view** mode by applying filters to the view\. You can create a filter for a specific property, such as execution status, or for a date and time range\. For more information, see [Tutorial: Examining state machine executions using the Step Functions console](debug-sm-exec-using-ui.md)\.

![\[Table view of the View mode section. The Duration and Timeline are added to this Table view.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-details-table-view.png)

By default, this mode displays the **Name**, **Type**, **Status**, **Resource**, and **Started After** columns\. However, you can configure the columns you want to view using the **Preferences** dialog box\. The selections that you make on this dialog box persist for future state machine executions\.

If you add the **Timeline** column, the execution duration of each state is shown with respect to the runtime for the entire execution\. This is displayed as a color\-coded, linear timeline\. This can help you identify any performance\-related issues with a specific state's execution\. The color\-coded segments for each state on the timeline help you identify the state's execution status, such as in\-progress, failed, or aborted\.

For example, if you've defined execution retries for a state in your state machine, these retries are shown in the timline\. Red segments represent the failed `Retry` attempts, while light gray segments represent the `BackoffRate` between each `Retry` attempt\.

![\[Timeline column of the Table view section displaying the color-coded segments on the linear timeline to indicate different statuses of individual step executions.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-table-view-timeline-color-codes.png)

If your state machine contains a `Map` state, `Parallel` state, or both, you can view their names in the workflow in **Table view**\. For `Map` and `Parallel` states, the **Table view** mode displays the execution data for their iterations and parallel branches as nodes inside a tree view\. You can choose each node in these states to view their individual details in the *[Step details](#exec-details-intf-step-details)* section\. For example, you can review the data for a specific **Map** state iteration that caused the state to fail\. Expand the node for the **Map** state, and then view the status for each iteration in the **Status** column\.

------
#### [ Event view ]

The **Event view** mode displays the complete history for the selected execution as a list of events spanning multiple pages\. Each page contains up to 25 events\. This view also displays the total event count, which can help you determine if you exceeded the maximum count of 25,000 events\.

![\[Event view of the View mode section. Page number 3 is selected to display the execution event history on that page.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-details-event-view.png)

By default, the results in the **Event view** mode are displayed in ascending order based on the **Timestamp** of the events\. You can change the execution event history's sorting to descending order by clicking on the **Timestamp** column header\.

In the **Event view** mode, each event is color\-coded to indicate its execution status\. For example, events that failed are colored red\. To view additional details about an event, choose the ![\[arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-show-details-icon.png)![\[arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/) next to the event ID\. Once open, the event details show the input, output, and resource invocation for the event\.

In addition, in the **Event view** mode, you can apply filters to a property to limit the execution event history results that are displayed\. You can choose properties such as execution ID, or a date and time range\. For more information, see [Tutorial: Examining state machine executions using the Step Functions console](debug-sm-exec-using-ui.md)\.

------

## Step details<a name="exec-details-intf-step-details"></a>

The *Step details* section opens up on the right when you choose a state in the **Graph view** or **Table view**\. This section contains the following tabs, which provide you in\-depth information about the selected state:

**Input & Output**  
Shows the input and output details of the selected state\. If there was an error in the input or output, it is indicated with a ![\[error icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-step-detail-input-output-error-icon.png)![\[error icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[error icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/) on the tab header\. In addition, you can view the reason for the error in the **Reason** box\.  
You can choose the **Advanced view** toggle button to see the input and output data transfer path as the data passed through the selected state\. This lets you identify how your input was processed as one or more of the fields, such as `InputPath`, `Parameters`, `ResultSelector`, `OutputPath`, and `ResultPath`, were applied to the data\.

**Details**  
Shows information, such as the state type, its execution status, and execution duration\.  
For `Task` states that use a resource, such as AWS Lambda, this tab provides links to the resource definition page and Amazon CloudWatch logs page for the resource invocation\. It also shows values, if specified, for the `Task` state's `TimeoutSeconds` and `HeartbeatSeconds` fields\.  
For `Map` states, this tab shows you information regarding the total count of a `Map` state's iterations\. Iterations are categorized as **Failed**, **Aborted**, **Succeeded**, or **InProgress**\.

**Definition**  
Shows the Amazon States Language definition corresponding to the selected state\.

**Retry**  
This tab appears only if you've defined a `Retry` field in your state machine's `Task` or `Parallel` state\.
Shows the initial and subsequent retry attempts of the selected state\. For the **Initial attempt** and all the subsequent failed attempts, choose the ![\[arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-show-details-icon.png)![\[arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[arrow icon\]](http://docs.aws.amazon.com/step-functions/latest/dg/) next to the **Status** to view the **Reason** for failure that appears in a dropdown box\. If the retry attempt succeeds, you can view the **Output** that appears in a dropdown box\.

**Events**  
Shows a filtered list of the events associated with the selected state in an execution\. The information you see on this tab is a subset of the complete execution event history you see in the *[Event view](#events-view-tab3)* mode\.