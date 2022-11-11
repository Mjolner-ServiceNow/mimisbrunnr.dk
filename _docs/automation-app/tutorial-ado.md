---
layout: single
title: Microsoft Azure DevOps Integration
permalink: /automation-app/azure-devops-integration/
last_modified_at: 2022-11-01T16:36:18-04:00
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

![Create a new credential](/assets/images/x_autps_azure_auto_add_variable0.webp)

Click on **Variables** in the top menu and select **Create** at the bottom of the list to create a new credential.

![Create a new credential](/assets/images/x_autps_azure_auto_ado_add_credential3.webp)

1. Give the credential a name. Eg. **AzureDevOpsTokenCred**
2. Give the variable an optional description. It is always a good approach to note the expiration date of the token
3. Provide the full username to use
4. Insert the token that you have just created
5. Click on **Create** to create the credential

### Create a Runbook for new Work Items

Next up we will create a new Runbook that we will use to create new Work Items.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_create_runbook0.webp)

Select **Runbooks** i the main menu in **Runbook Manager**.

Make sure that you are in the same **Automation Account** as you created the Personal Access Token variable in and click **Create** at the bottom of the list.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook0.webp)

Fill out the fields and click **Create**. Notice that we will be using PowerShell 5.1.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook1.webp)

Open the newly create Runbook by click on it and then click on **Edit** to unlock the Runbook for editing.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook2.webp)

Click on **Templates** in the snippets section to the left and select **Microsoft Azure DevOps** -> **New Task**.

Then click in the blank canvas to the right and paste in the template.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook3.webp)

Fill out the environment variables section of the template.

The **tokenName** should be set to the name of the credentials that you created earlier.

**OrganizationName** should be set to the name of your organization in AzureDevOps.

Fill in the **projectName** with the name of the project that you wish to add Work Items to.

In case your project name contains spaces, replace them with **%20**.

Next click on **Publish**.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook4.webp)

Click on **Run** to start a new job.

Fill out the form with some test data and click on **Create**

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook5.webp)

After a few minutes the state should change to **Completed** indicating that job has completed without any errors.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook6.webp)

Scroll down through the **Extracted output variables** and notice the **url** variable. We will using this later in this tutorial as an identifier, so make sure you copy it.

### Create a Runbook for adding comments to Work Item

Next we will create another Runbook. This time to be able to add comments to existing Work Items.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook7.webp)

Click the **home** icon in Runbook Manager and then click on **Create new** to create another Runbook.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook8.webp)

This time we will create a Runbook to add a comment to a Work Item.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook9.webp)

Click the newly create Runbook to open it.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook10.webp)

Click on **Edit** to unlock the Runbook for editing.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook11.webp)

1. Click on **Templates**
2. Click on **Microsoft Azure DevOps**
3. Click on **New Task Comment**
4. Paste the now copied template into the blank canvas.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook12.webp)

Modify the environment variable by inserting the name of the variable containing your token.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook13.webp)

Paste in the URL that you copied in the previous section under workItemURL and fill in the other fields with test data.

Click create to create a new Job.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook14.webp)

After a few minutes the State should change to **Completed** indicating that we have now added the comment to the Work Item.

To verify that everything works open op Azure DevOps.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook15.webp)

Go to **Boards** and select **Work items**. Click on the Work Item that we have just created using the Runbook.

![Create an empty Runbook](/assets/images/x_autps_azure_auto_ado_create_runbook16.webp)

Verify that everything looks as you would expect.

## Creating Flow in ServiceNow

Now that we have the connection and the Runbooks in place we will create two Flows in ServiceNow.


## Setting up connection from Azure DevOps

We also want to make sure that when the Work Item is set to Done in

### Creating integration user in ServiceNow

### Creating Runbook for updating ServiceNow


## Before taking this to production

The above is a simple integration with a single state mapping.

To make this integration production ready you must consider what should happen with other state changes. Eg. what would happen if the Incident is reopened in ServiceNow. What would happen if the incident is re-assigned to another assignment group etc.
