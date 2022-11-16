---
layout: single
title: Microsoft Azure DevOps Integration
permalink: /automation-app/azure-devops-integration/
last_modified_at: 2022-11-15T16:36:18-04:00
sidebar:
  nav: "aa"
toc: true
---

In this guide we will build a simple bi-directional integration between ServiceNow and Microsoft Azure DevOps.

This guide will be based on a process where incidents are created in ServiceNow. If the Incident is assigned to a specific assignment group, then a work item in Microsoft Azure DevOps will be created.

We will keep the integration simple, while still showing you all the techniques that you need to know to build a more complex production ready integration.

## Setting up connection to Azure DevOps

This integration will be based on an integration user in Azure DevOps.

**Notice!** For testing purposes you can use your own user, but it is recommended to create a service account for the integration.
{: .notice--warning}

### Getting a Personal Access Token

Login to [Azure DevOps](https://dev.azure.com/) and select the Project that you wish to integrate to.

![Select personal access token](/assets/images/x_autps_azure_auto_ado_add_credential0.webp)

Click the user icon with the little gear icon in the upper right corner and select **Personal acccess tokens**

![Create a new personal access token](/assets/images/x_autps_azure_auto_ado_add_credential1.webp)

1. Click on **New Token**
2. Give the Token a name
3. Set expiration date of the token.
4. Under Work Items set the access to **Read, write, & manage**

Then click on **Create**

![Create a new credential](/assets/images/x_autps_azure_auto_ado_add_credential2.webp)

A token will now be displayed. Make sure to click the copy icon to copy the token

![Open Runbook Manager](/assets/images/x_autps_azure_auto_menu_runbooks.webp)

In ServiceNow open Runbook Manager in the Application Navigator

![Select varialbes](/assets/images/x_autps_azure_auto_add_variable0.webp)

Click on **Variables** in the top menu and select **Create** at the bottom of the list to create a new credential.

![Create a new credential](/assets/images/x_autps_azure_auto_ado_add_credential3.webp)

1. Give the credential a name. Eg. **AzureDevOpsTokenCred**
2. Give the variable an optional description. It is always a good approach to note the expiration date of the token
3. Provide the full username to use
4. Insert the token that you have just created
5. Click on **Create** to create the credential

### Create a Runbook for new Work Items

Next up we will create a new Runbook that we will use to create new Work Items.

![Select Runbooks in Runbook Manager](/assets/images/x_autps_azure_auto_create_runbook0.webp)

Select **Runbooks** i the main menu in **Runbook Manager**.

Make sure that you are in the same **Automation Account** as you created the Personal Access Token variable in and click **Create** at the bottom of the list.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook0.webp)

Fill out the fields and click **Create**. Notice that we will be using PowerShell 5.1.

![Put Runbook into Edit mode](/assets/images/x_autps_azure_auto_ado_create_runbook1.webp)

Open the newly create Runbook by click on it and then click on **Edit** to unlock the Runbook for editing.

![Select Runbook template](/assets/images/x_autps_azure_auto_ado_create_runbook2.webp)

Click on **Templates** in the snippets section to the left and select **Microsoft Azure DevOps** -> **New Task**.

Then click in the blank canvas to the right and paste in the template.

![Configure and publish the Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook3.webp)

Fill out the environment variables section of the template.

The **tokenName** should be set to the name of the credentials that you created earlier.

**OrganizationName** should be set to the name of your organization in AzureDevOps.

Fill in the **projectName** with the name of the project that you wish to add Work Items to.

In case your project name contains spaces, replace them with **%20**.

Next click on **Publish**.

![Run a job](/assets/images/x_autps_azure_auto_ado_create_runbook4.webp)

Click on **Run** to start a new job.

Fill out the form with some test data and click on **Create**

![Wait for job to complete](/assets/images/x_autps_azure_auto_ado_create_runbook5.webp)

After a few minutes the state should change to **Completed** indicating that job has completed without any errors.

![Inspect extracted output variables](/assets/images/x_autps_azure_auto_ado_create_runbook6.webp)

Scroll down through the **Extracted output variables** and notice the **url** variable. We will using this later in this tutorial as an identifier, so make sure you copy it.

### Create a Runbook for adding comments to Work Item

Next we will create another Runbook. This time to be able to add comments to existing Work Items.

![Navigate to Runbook list in Runbook Manager](/assets/images/x_autps_azure_auto_ado_create_runbook7.webp)

Click the **home** icon in Runbook Manager and then click on **Create new** to create another Runbook.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook8.webp)

This time we will create a Runbook to add a comment to a Work Item.

![Open the new Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook9.webp)

Click the newly create Runbook to open it.

![Put Runbook into edit mode](/assets/images/x_autps_azure_auto_ado_create_runbook10.webp)

Click on **Edit** to unlock the Runbook for editing.

![Apply Runbook template](/assets/images/x_autps_azure_auto_ado_create_runbook11.webp)

1. Click on **Templates**
2. Click on **Microsoft Azure DevOps**
3. Click on **New Task Comment**
4. Paste the now copied template into the blank canvas.

![Tailor and publish the Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook12.webp)

Modify the environment variable by inserting the name of the variable containing your token and click on **Publish**.

![Start a new job](/assets/images/x_autps_azure_auto_ado_create_runbook13.webp)

Click on **Run** and paste in the URL that you copied in the previous section under workItemURL and fill in the other fields with test data.

Click create to create a new Job.

![Wait for job to complete](/assets/images/x_autps_azure_auto_ado_create_runbook14.webp)

After a few minutes the State should change to **Completed** indicating that we have now added the comment to the Work Item.

To verify that everything works open op Azure DevOps.

![Navigate to Work Item in Azure DevOps](/assets/images/x_autps_azure_auto_ado_create_runbook15.webp)

Go to **Boards** and select **Work items**. Click on the Work Item that we have just created using the Runbook.

![Inspect Work Item](/assets/images/x_autps_azure_auto_ado_create_runbook16.webp)

Verify that everything looks as you would expect.

## Creating Flow in ServiceNow

Now that we have the connection and the Runbooks in place we will create two Flows in ServiceNow.

### Creating Work Item from Incident record

![Open ServiceNow Flow Designer](/assets/images/x_autps_azure_auto_open_flow_designer.webp)

Open flow designer by selecting **Process Automation** -> **Flow Designer**.

![Create a new flow](/assets/images/x_autps_azure_auto_create_flow0.webp)

Click the **New** button and select **Flow**.

![Enter defails for the new flow](/assets/images/x_autps_azure_auto_ado-tutorial0.webp)

Give the flow a meaningful **Flow name** and set the **Run As** to **System User**.

Click **Submit** to create the flow.

![Add a trigger to the flow](/assets/images/x_autps_azure_auto_ado-tutorial1.webp)

Click to add a trigger to the flow and select **RECORD** -> **Created or Updated**.

![Configure trigger](/assets/images/x_autps_azure_auto_ado-tutorial2.webp)

Select the table **Incident** and set the filter to the assignment group, that you wish to syncronise with Azure DevOps. In this case we will use an assignment group called **Azure DevOps** which we have created for this purpose.

Click **Done** to save the trigger.

![Add an action to the flow](/assets/images/x_autps_azure_auto_flow_add_action0.webp)

Next click on **Add an Action, Flow Logic, or Subflow** and select **Automation App** -> **Start Job** to add a Start Job action to the flow.

![Configure the action](/assets/images/x_autps_azure_auto_ado-tutorial3.webp)

Set **Runbook** to the Runbook that we created previously to create a Work Item in Azure DevOps. In our case we named it **ADO-CreateWorkItem** and set the Input format to JSON

Click on the code toggle button to the right of the Input field and paste in the following code:

```javascript
var input = {};
input.title = fd_data.trigger.current.number.toString() + ": " + fd_data.trigger.current.short_description.toString();
input.description = fd_data.trigger.current.description.toString();
input.requester = fd_data.trigger.current.caller_id.email.toString();
input.priority = fd_data.trigger.current.getValue('priority').toString();
input.category = fd_data.trigger.current.category.toString();
input.subCategory = fd_data.trigger.current.subcategory.toString();
return JSON.stringify(input);
```

Insert the Incident Record from the trigger in the **Parent** field and click on **Done**.

![Add a second action](/assets/images/x_autps_azure_auto_ado-tutorial4.webp)

Add another action below the **Start job** action. This time we pick the **Get output variable**.

![Configure the second action](/assets/images/x_autps_azure_auto_ado-tutorial5.webp)

1. Set the **Job** to the job record from step 1.
2. Set the **Variable** to "url".
3. Set **Fail if missing** to true, to ensure that we capture if we are not getting an url returned.
4. Click on **Done**.

![Add a third action](/assets/images/x_autps_azure_auto_ado-tutorial6.webp)

Add another action below the **Get output variable**. This time we will use **ServiceNow Core** -> **Update Record**.

![Congifure an update record action](/assets/images/x_autps_azure_auto_ado-tutorial7.webp)

Set **Record** To the incident record from the trigger of the flow.

Under **Fields** set **Correlation ID** to **Value** from action 2 and add a work note to **Work notes** so that it is easily visible on the incident form, that a work item has been created.

Click on **Done**.

Click on **Save** and **Activate**.

![Navigate to create new incident](/assets/images/x_autps_azure_auto_ado-tutorial8.webp)

To test that that integration works as expected navigate to **Incident** -> **Create New**.

![Fill out create incident form](/assets/images/x_autps_azure_auto_ado-tutorial9.webp)

1. Fill out Caller, Category and Subcategory.
2. Give the Incident a short description and description.
3. Set the assignment group to the assignment group that you configured as the trigger in the flow.
4. Click on submit.

![Open created incident](/assets/images/x_autps_azure_auto_ado-tutorial10.webp)

Open the Incident record that you just created.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial11.webp)

After a few seconds a work note should become visible incidicating that a work item has been created.

Log on to Azure DevOps to verify that everything looks as expected.

### Adding comments from Incident record to Work Item

Next we will setup a flow that copy any comments added to the incident to the work item in Azure DevOps.

![Open ServiceNow Flow Designer](/assets/images/x_autps_azure_auto_open_flow_designer.webp)

Open flow designer by selecting **Process Automation** -> **Flow Designer**.

![Create a new flow](/assets/images/x_autps_azure_auto_create_flow0.webp)

Click the **New** button and select **Flow**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial12.webp)

Give the flow a meaningful name and set **Run as** to **System User**.

Click **Submit**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial13.webp)

Click to add a trigger to the flow and select **RECORD** -> **Created or Updated**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial14.webp)

1. Set the table to **Incident**.
2. Set the tilter to only select records that where **Correlation ID** is not empty, the **Assignment group** is **Azure DevOps** and the **Additional comments** changes.
3. Make sure to select **For every update** under **Run Trigger**.
4. Click on **Done**.

![Add an action to the flow](/assets/images/x_autps_azure_auto_flow_add_action0.webp)

Next click on **Add an Action, Flow Logic, or Subflow** and select **Automation App** -> **Start Job** to add a Start Job action to the flow.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial15.webp)

Select the Runbook naned **ADO-CommentWorkItem** that we created earlier and set the **Input format** to **JSON**.

Click on the **code** button next to the **Input** field and paste in the below code:

```javascript
var sys_id = fd_data.trigger.current.sys_id.toString();
var input = {};
input.requester = fd_data.trigger.current.sys_updated_by.toString();
var gr = new GlideRecord('sys_journal_field');
gr.addQuery('element','comments');
gr.addQuery('element_id',sys_id);
gr.setLimit(1);
gr.orderByDesc('sys_created_on', 'DESC');
gr.query();
if(gr.next()) {
    input.comment = gr.getValue('value');
}
input.workItemURL = fd_data.trigger.current.correlation_id.toString();
return JSON.stringify(input);
```

Click on **Done**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial16.webp)

Next go to the Incident that we created in the previous step and add a comment.

Make sure that **Work notes** is not selected, as we are only looking at comments.

Click on **Post**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial17.webp)

Navigate to Azure DevOps to verify that the comment was successfully added within a few seconds.

## Setting up connection from Azure DevOps

We also want to make sure that when the Work Item is set to Done in Azure DevOps the corresponding Incident is ServiceNow is marked as resolved.

### Creating integration user in ServiceNow

For Azure DevOps to be able to update ServiceNow we need to first create a user for the purpose.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial18.webp)

Navigate to **User Administration** -> **Users** and click on **New**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial19.webp)

Fill out the **User ID**, **First name**, **Last name**, and set **Web service access only** to **true**.

Then click on **Submit**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial20.webp)

Click on **Set Passsword** and then on **Generate**.

Once the password has been generated click on the **copy** icon and then on **Save Passsword.

We will be using the password in just a second, so keep it in your clipboard.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial21.webp)

Click on the related list **Roles** and select **Edit**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial22.webp)

To be able to update Incidents we will add the role **itil** to the user.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_menu_runbooks.webp)

Located **Runbook Manager** in the navigator and open it.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_credential0.webp)

Select **Credentials** in the the menu to the top and click on **Create**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial23.webp)

Fill out the form as shown and paste in the password that we set for the user.

Click on **Create**.

### Creating Runbook for updating ServiceNow

Now that we have a ServiceNow user with credentials stored in Azure Automation we will continue to create a Runbook that can update an Incident.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial24.webp)

Create a new Runbook in Runbook Manager like so.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial25.webp)

1. Click on **Edit**.
2. Click on **Templates**.
3. Expand **Microsoft Azure DevOps**
4. Click on **Update ServiceNow State** to copy the template to your clipboard.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial26.webp)

Paste in the template in the canvas and modify the variables to match your environment.

Click on **Publish** to save and publish the Runbook.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial27.webp)

Next go to [portal.azure.com](https://portal.azure.com) and search for the Runbook that you have just created.

Click on it to open it.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial28.webp)

In the menu to the left select **Webhooks**.

Click on **Add Webhook**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial29.webp)

Click on **Create new webhook**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial30.webp)

1. Give the webhook a meaningful name.
2. Set **Enabled** to **Yes**.
3. Set the expiration date. This can be any date up to 10 years out in the future.
4. Copy the link to the webhook, by clicking the copy icon to the right.
5. Click on **OK**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial31.webp)

Click on **Configure Parameters and run settings**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial32.webp)

Do not make any changes, but click on **OK**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial33.webp)

Next click on **Create** and the webhook will be created.

### Configuring Azure DevOps to trigger Runbook

Now that we have the webhook in place we need to update Azure DevOps to trigger the webhook on state changes.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial34.webp)

Open your project and click on **Project settings** in the lower left corner.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial35.webp)

Click on **Service hooks** and then **Create subscription**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial36.webp)

Select **Web Hooks** and click on **Next**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial37.webp)

Select **Work item updated** and set the Field to **State**. Click on **Next**.

![Work note showing a work item has been created](/assets/images/x_autps_azure_auto_ado-tutorial38.webp)

Paste in the URL to the **webhook** that you copied earlier and click on **Finish**.

Now change the state on the Work Item that we created in one of the previous steps to **Done** and observe that the state of corresponding incident in ServiceNow is updated to **Resolved**.

## Before taking this to production

The above is a simple integration with a single state mapping.

To make this integration production ready you must consider what should happen with other state changes. Eg. what would happen if the Incident is reopened in ServiceNow. What would happen if the incident is re-assigned to another assignment group etc.
