# Tutorial: Examining state machine executions using the Step Functions console<a name="debug-sm-exec-using-ui"></a>

In this tutorial, you will learn how to inspect the execution information displayed on the *Execution Details* page and view the reason for a failed execution\. Then, you'll learn how to access different iterations of a `Map` state execution\. Finally, you'll learn how to configure the columns to display on the **Table view** and apply suitable filters to view only the information of interest\.

The state machine used in this tutorial obtains the price of a set of fruits\. To do this, the state machine uses three AWS Lambda functions which return a random list of four fruits, the price of each fruit, and the average cost of the fruits\. The Lambda functions are defined to throw an error if the price of the fruits is less than or equal to a threshold value\.

**Contents**
+ [Step 1: Create and test the required Lambda functions](#step-create-all-lambda-functions)
+ [Step 2: Create and execute the state machine](#step-create-exec-sm)
+ [Step 3: View the state machine execution details](#view-sm-exec-details)
+ [Step 4: Explore the different *View modes*](#sm-exec-details-exp-view-modes)

## Step 1: Create and test the required Lambda functions<a name="step-create-all-lambda-functions"></a>

1. Open the [Lambda console](https://console.aws.amazon.com/lambda/home) and then perform steps 1 through 4 in the [Step 1: Create a Lambda Function](tutorial-creating-lambda-state-machine.md#create-lambda-state-machine-step-2) section\. Make sure to name the Lambda function **GetListOfFruits**\.

1. After your Lambda function is created, copy the function's Amazon Resource Name \(ARN\) displayed in the upper\-right corner of the page\. To copy the ARN, click the ![\[icon to copy the Lambda function's Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-sm-using-lambda-copy-icon.png)![\[icon to copy the Lambda function's Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[icon to copy the Lambda function's Amazon Resource Name\]](http://docs.aws.amazon.com/step-functions/latest/dg/)\. The following is an example ARN:

   ```
   arn:aws:lambda:us-east-1:123456789012:function:function-name
   ```

1. Copy the following code for the Lambda function into the **Code source** area of the **GetListOfFruits** page\.

   ```
   function getRandomSubarray(arr, size) {
       var shuffled = arr.slice(0), i = arr.length, temp, index;
       while (i--) {
           index = Math.floor((i + 1) * Math.random());
           temp = shuffled[index];
           shuffled[index] = shuffled[i];
           shuffled[i] = temp;
       }
       return shuffled.slice(0, size);
   }
   
   exports.handler = async function(event, context) {
       
       const fruits = ['Abiu','Açaí','Acerola','Ackee','African cucumber','Apple','Apricot','Avocado','Banana','Bilberry','Blackberry','Blackcurrant','Jostaberry'];
   
       
        const errorChance = 45;
       
       const waitTime = Math.floor( 100 * Math.random() );
   
       await new Promise( r => setTimeout(() => r(), waitTime));
   
       const num = Math.floor( 100 * Math.random() );
       // const num = 51;
        if (num <= errorChance) {
            throw(new Error('Error'));
        }
   
       return getRandomSubarray(fruits, 4);
   };
   ```

1. Choose **Deploy** and then choose **Test** to deploy the changes and see the output of your Lambda function\.

1. Create the remaining two Lambda functions named **GetFruitPrice** and **CalculateAverage** with the following steps:

   1. Copy the following code into the **Code source** area of the **GetFruitPrice** Lambda function:

      ```
      exports.handler = async function(event, context) {
          
          const errorChance = 0;
          const waitTime = Math.floor( 100 * Math.random() );
      
          await new Promise( r => setTimeout(() => r(), waitTime));
      
          const num = Math.floor( 100 * Math.random() );
          if (num <= errorChance) {
              throw(new Error('Error'));
          }
      
          return Math.floor(Math.random()*100)/10;
      };
      ```

   1. Copy the following code into the **Code source** area of the **CalculateAverage** Lambda function:

      ```
      function getRandomSubarray(arr, size) {
          var shuffled = arr.slice(0), i = arr.length, temp, index;
          while (i--) {
              index = Math.floor((i + 1) * Math.random());
              temp = shuffled[index];
              shuffled[index] = shuffled[i];
              shuffled[i] = temp;
          }
          return shuffled.slice(0, size);
      }
      
      const average = arr => arr.reduce( ( p, c ) => p + c, 0 ) / arr.length;
          
      exports.handler = async function(event, context) {
              const errors = [
              "Error getting data from DynamoDB",
              "Error connecting to DynamoDB",
              "Network error",
              "MemoryError - Low memory"
              ]
              
          const errorChance = 0;
          
          const waitTime = Math.floor( 100 * Math.random() );
      
          await new Promise( r => setTimeout(() => r(), waitTime));
      
          const num = Math.floor( 100 * Math.random() );
          if (num <= errorChance) {
              throw(new Error(getRandomSubarray(errors, 1)[0]));
          }
      
          return average(event);
      };
      ```

   1. Make sure to copy the ARNs of these two Lambda functions, and then **Deploy** and **Test** them\.

## Step 2: Create and execute the state machine<a name="step-create-exec-sm"></a>

Use the [Step Functions console](https://console.aws.amazon.com/states/home?region=us-east-1#/) to create a state machine that invokes the [Lambda functions you created in Step 1](#step-create-all-lambda-functions)\. In this state machine, three `Map` states are defined\. Each of these `Map` states contains a `Task` state to invoke each of the Lambda functions\. Additionally, a `Retry` field is defined in each `Task` state with a number of retry attempts defined for each state\. If a `Task` state encounters a runtime error, it's executed again up to the defined number of retry attempts for that `Task`\.

1. Open the [Step Functions console](https://console.aws.amazon.com/states/home) and choose **Write your workflow in code**\.
**Important**  
Ensure that your state machine is under the same AWS account and Region as the Lambda function you created earlier\.

1. For **Type**, keep the default selection of **Standard**\.

1. Copy the following Amazon States Language definition and paste it under **Definition**\. Make sure to replace the ARNs shown with those of the Lambda functions that you previously created\.

   ```
   {
       "StartAt": "LoopOverStores",
       "States": {
           "LoopOverStores": {
               "Type": "Map",
               "Iterator": {
                   "StartAt": "GetListOfFruits",
                   "States": {
                       "GetListOfFruits": {
                           "Type": "Task",
                           "Resource": "arn:aws:states:::lambda:invoke",
                           "OutputPath": "$.Payload",
                           "Parameters": {
                               "FunctionName": "arn:aws:lambda:us-east-1:123456789012:function:GetListofFruits:$LATEST",
                               "Payload": {
                                   "storeName.$": "$"
                               }
                           },
                           "Retry": [
                               {
                                   "ErrorEquals": [
                                       "States.ALL"
                                   ],
                                   "IntervalSeconds": 2,
                                   "MaxAttempts": 1,
                                   "BackoffRate": 1.3
                               }
                           ],
                           "Next": "LoopOverFruits"
                       },
                       "LoopOverFruits": {
                           "Type": "Map",
                           "Iterator": {
                               "StartAt": "GetFruitPrice",
                               "States": {
                                   "GetFruitPrice": {
                                       "Type": "Task",
                                       "Resource": "arn:aws:states:::lambda:invoke",
                                       "OutputPath": "$.Payload",
                                       "Parameters": {
                                           "FunctionName": "arn:aws:lambda:us-east-1:123456789012:function:GetFruitPrice:$LATEST",
                                           "Payload": {
                                               "fruitName.$": "$"
                                           }
                                       },
                                       "Retry": [
                                           {
                                               "ErrorEquals": [
                                                   "States.ALL"
                                               ],
                                               "IntervalSeconds": 2,
                                               "MaxAttempts": 3,
                                               "BackoffRate": 1.3
                                           }
                                       ],
                                       "End": true
                                   }
                               }
                           },
                           "ItemsPath": "$",
                           "End": true
                       }
                   }
               },
               "ItemsPath": "$.stores",
               "Next": "LoopOverStoreFruitsPrice",
               "ResultPath": "$.storesFruitsPrice"
           },
           "LoopOverStoreFruitsPrice": {
               "Type": "Map",
               "End": true,
               "Iterator": {
                   "StartAt": "CalculateAverage",
                   "States": {
                       "CalculateAverage": {
                           "Type": "Task",
                           "Resource": "arn:aws:states:::lambda:invoke",
                           "OutputPath": "$.Payload",
                           "Parameters": {
                               "FunctionName": "arn:aws:lambda:us-east-1:123456789012:function:Calculate-average:$LATEST",
                               "Payload.$": "$"
                           },
                           "Retry": [
                               {
                                   "ErrorEquals": [
                                       "States.ALL"
                                   ],
                                   "IntervalSeconds": 2,
                                   "MaxAttempts": 2,
                                   "BackoffRate": 1.3
                               }
                           ],
                           "End": true
                       }
                   }
               },
               "ItemsPath": "$.storesFruitsPrice",
               "ResultPath": "$.storesPriceAverage",
               "MaxConcurrency": 1
           }
       }
   }
   ```

1. Enter a name for your state machine\. Keep the default selections for the other options on the page, and then choose **Create state machine**\.

1. Open the page titled with your state machine name\. Perform steps 1 through 4 in the [Step 4: Start a New Execution](tutorial-creating-lambda-state-machine.md#create-lambda-state-machine-step-5) section, but use the following data as the execution input:

   ```
   {
       "stores": [
         "Store A",
         "Store B",
         "Store C",
         "Store D"
       ]
   }
   ```

## Step 3: View the state machine execution details<a name="view-sm-exec-details"></a>

On the page titled with your execution ID, you can review the results of your execution and debug any errors\.

1. \(Optional\) Choose from the tabs displayed on the *Execution Details* page to see the information present in each of them\. For example, to view the state machine input and its execution output, choose **Execution input & output** on the *[Execution summary](exec-details-interface-overview.md#exec-details-intf-exec-summ)* section\.

1. If your state machine execution failed, choose **Cause** or **Show step detail** on the error message\. Details about the error are displayed in the *[Step details](exec-details-interface-overview.md#exec-details-intf-step-details)* section\. Notice that the affected step, which is a `Task` state named **GetListofFruits**, that caused the error appears highlighted in the **Graph view** and **Table view**\.
**Note**  
Because the **GetListofFruits** step is defined inside a `Map` state, and the step failed to execute successfully, the **Status** of `Map` state step is displayed as **Failed**\.

## Step 4: Explore the different *View modes*<a name="sm-exec-details-exp-view-modes"></a>

You can choose a preferred mode to view the state machine workflow or the execution event history\. Some of the tasks that you can perform in these *View modes* are as follows:

### **Graph view** – Switch between different `Map` state iterations<a name="graph-view-see-map-state-iterations"></a>

If your **Map** state has five iterations and you want to view the execution details for the iteration number three and four respectively, do the following:

1. Choose the `Map` state for which you want to view the different iteration data\.

1. From **Map iteration viewer**, choose the iteration you want to view\. Iterations are counted from zero\. To choose the third iteration out of five, choose **\#2** from the dropdown list next to the **Map** state's name\.
**Note**  
If your state machine contains nested `Map` states, the dropdown lists for the parent and child `Map` state iterations will be displayed as following:  

![\[Graph view mode component's Map iteration viewer, displaying a series of dropdown lists representing the iteration data for nested Map states.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-view-mode-nested-map-states-iterations.png)

1. \(Optional\) If one or more of your `Map` state iterations failed to execute, or the iteration stopped in an aborted state, you can view details about the failed iteration\. To see these details, choose the affected iteration numbers under **Failed** or **Aborted** in the dropdown list\.

### **Table view** – Switch between different `Map` state iterations<a name="table-view-see-map-state-iterations"></a>

If your **Map** state has five iterations and you want to view the execution details for the iteration number three and four, do the following:

1. Choose the `Map` state for which you want to view the different iteration data\.

1. In the tree view display of the `Map` state iterations, choose the row for iteration named **\#2** for the iteration number three\. Similarly, choose the row named **\#3** for the iteration number four\.

### **Table view** – Configure the columns to display<a name="table-view-cfg-display-cols"></a>

Choose ![\[Icon to configure columns to display in the Table view.\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-table-view-preferences-gear-icon.png)![\[Icon to configure columns to display in the Table view.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[Icon to configure columns to display in the Table view.\]](http://docs.aws.amazon.com/step-functions/latest/dg/)\. Then, in the **Preferences** dialog box, choose the columns you want to display under **Select visible columns**\.

By default, this mode displays the **Name**, **Type**, **Status**, **Resource**, and **Started After** columns\.

### **Table view** – Filter the results<a name="table-view-filter-results"></a>

Limit the amount of information displayed by applying one or more filters based on a property, such as **Status**, or a date and time range\. For example, to view the steps that failed execution, apply the following filter:

1. Choose **Filter by properties or search by keyword**, and then choose **Status** under **Properties**\.

1. Under **Operators**, choose **Status =**\.

1. Choose **Status = Failed**\.

   

1. \(Optional\) Choose **Clear filters** to remove the applied filters\.

### **Event view** – Filter the results<a name="event-view-filter-results"></a>

Limit the amount of information displayed by applying one or more filters based on a property, such as **Type**, or a date and time range\. For example, to view the `Task` state steps that failed execution, apply the following filter:

1. Choose **Filter by properties or search by keyword**, and then choose **Type** under **Properties**\.

1. Under **Operators**, choose **Type =**\.

1. Choose **Type = TaskFailed**\.

1. \(Optional\) Choose **Clear filters** to remove the applied filters\.

### **Event view** – Inspect a **TaskFailed** event detail<a name="event-view-inspect-failed-task-details"></a>

Choose the ![\[icon to show input and output details about a specific event step that appears in a dropdown box\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/sm-exec-show-details-icon.png)![\[icon to show input and output details about a specific event step that appears in a dropdown box\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[icon to show input and output details about a specific event step that appears in a dropdown box\]](http://docs.aws.amazon.com/step-functions/latest/dg/) next to the ID of a **TaskFailed** event to inspect its details, including input, output, and resource invocation that appear in a dropdown box\.