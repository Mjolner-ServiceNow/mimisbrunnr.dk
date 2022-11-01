---
layout: single
title: ServiceNow Flow Designer
permalink: /automation-app/flow-designer/
last_modified_at: 2022-10-28T08:16:18+01:00
sidebar:
  nav: "aa"
toc: true
---

Using Automation App in ServiceNow Flow Designer is a powerfull way to integrate your flows with Azure Automation to accomplish simple and secure automation.

## Create a new Flow

In this guide we will show how to create a basic flow using ServiceNow Flow Designer.

![Open Flow Designer](/assets/images/x_autps_azure_auto_open_flow_designer.webp)

Open flow designer by selecting **Process Automation** -> **Flow Designer**

![Create a new Flow](/assets/images/x_autps_azure_auto_create_flow0.webp)

Click the **New** button and select **Flow**.

![Create new flow form](/assets/images/x_autps_azure_auto_create_flow1.webp)

Give the Flow a name e.g., "Order Hello-World" and click the **Submit** button.

> If you do not select "System User" under "Run As" you need to ensure that the user initiating the flow has the "x_autps_azure_auto.user" role.

![Add a trigger to your flow](/assets/images/x_autps_azure_auto_create_flow2.webp)

All flows need a trigger. Click on **Add a trigger**. For this demo we will use **Service Catalog** as the trigger to simulate something that is ordered from the Service Catalog.

> To use the "Service Catalog" trigger it is required that you have installed the Flow Designer support for the Service Catalog plugin (com.glideapp.servicecatalog.flow_designer). This is a free plugin that enables you to use Flow Designer for Requested Items.

![Complete trigger](/assets/images/x_autps_azure_auto_create_flow3.webp)

Click on the **Done** button and then on the **Add an Action, Flow Logic, or Subflow** to add your first action.

## Start a Job

To start a Job in Microsoft Azure Automation we will use one of the standard actions of Automation App.

![Add the start job action](/assets/images/x_autps_azure_auto_flow_add_action0.webp)

Select **Action** and pick **Automation App** -> **Start job**.

![Fill out start job action form](/assets/images/x_autps_azure_auto_flow_add_action1.webp)

1. Under Runbook select the **Hello-World** Runbook that we created in the [Create a Runbook](/runbooks/#create-a-runbook) section.
2. Set the input format to **Auto**.
3. Insert something in the **Input** field. In most cases you would drag something in from the preceding actions, but in this case we just input a string. If there were more than one input parameter needed you can separate each input by using "\|",";", or ",". You can also supply a JSON string as the input.
4. For Requested Items we recommend that you input the Requested Items record as the Parent. This way you have easy access to related jobs from the Requested Items form.
5. Insert the figures for the **Average manual time to complete** as well as the **Average manual lead time to complete** to support accurate reporting. See the [Reporting](/reporting) section for more details.

> MSP Notice: If you are using Domain Separation the predefined action "Start job" will only work as long as the Flow is in the global domain. To use a Flow in any other domain you must create your own custom action for that domain.

## Working with Job Output

The output from a Job is stored in three different places. The first is the "Output" field on the job record. All output from the job is stored in this field.

Additionally for each output from the job a record is created in the "Output" table, which is related to the Job. Use this list of outputs to easily filter the different output types, such as "Output", "Error" etc.
If one of the outputs from the job contains a simple JSON object the JSON string is parsed and the different attributes of the object is saved as output variables in the "Output variable" table, which is also related to the job.

If your runbook is set to output a JSON, accessing the output becomes very easy in Flow Designer.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_get_output0.webp)

Click **Add an Action, Flow Logic, or Subflow** after your **Start job** action.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_get_output1.webp)

Select **Automation App** -> **Get output variable**.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_get_output2.webp)

1. Drag in the **Job** record that you wish to get the output from.
2. Enter the name of the variable that you wish to get.

You can optionally set this action to fail if it cannot find the variable that you are looking for.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_get_output3.webp)

To test we are getting the output we can add a **Log** action. Click on **Add an Action, Flow Logic, or Subflow**.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_get_output4.webp)

Select **ServiceNow Core** -> **Log**.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_get_output5.webp)

Put some text in the **Message** field and drag in the **Value** field from **Get output variable** using the data picker to the right.

## Test your flow

Next we will test our flow to see if we can start job and get the output that we expect.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_test0.webp)

Click on **Save** and then on the **Test** button in the upper right corner.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_test1.webp)

Since in our case we are not using any input from the Requested Item you can pick any Requested Item from the list.

Next click **Run Test**.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_test2.webp)

The test will now start. Once it has run, click the text link **Your test has finished running. View the flow execution details.**.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_test3.webp)

In the execution details you should now see that the your first action is in state **Waiting**.

Click on the **Refresh** icon at the top until the state of **Start job** changes to **Completed**. This may take a minute.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_test4.webp)

1. All actions should now be in state **Completed**.
2. Click on the **Log** action to expand it.
3. Verify that we got the output of the variable as expected.

We have now created a flow that has successfully executed in Microsoft Azure Automation.

We send the job input from ServiceNow which was processed and we successfully retrieved the output.

## Handle Runbook errors

At some point you are likely to experience a job will fail. In this case it is good to be prepared.

To handle job failures in Flow Designer is easy. Simply add an **If** condition after your **Start job** action.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_fail0.webp)

Click on the little **+** icon under the **Start job** action and select **Flow Logic** -> **If**.

![Click on Tenants](/assets/images/x_autps_azure_auto_flow_fail1.webp)

1. Give the condition a name. Eg. "If failed"
2. Drag in the **Status** field of the **Job** record.
3. Set the condition to **is not** with the value "Completed".

After this action you could create an Incident and/or a manual task to perform a manual workaround.

> Notice that you have both an **output** as well as an **exception** field available on the job record which may contain useful information for creating an Incident.
