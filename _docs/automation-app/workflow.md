---
layout: single
title: ServiceNow Workflow
permalink: /automation-app/workflow/
last_modified_at: 2022-10-24T08:16:18+01:00
sidebar:
  nav: "aa"
toc: true
---

It is possible to start and follow up on Jobs using the workflow engine of. To do so you need to use scripts to start a Job. The result will be a record on the Job table similar to what we did in the previous chapters.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_copy_sysid.webp)

Before we can implement a Runbook in a Workflow, we first need to know a few things about the Runbook. Open "Runbook Manager" as described in the section "Edit a Runbook" and locate the Runbook that you wish to implement in your workflow. Click the "Copy" icon next to it to copy a snippet to your clipboard.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_module.webp)

Keep the snippet in your clipboard or save it to a text editor. Then open "Workflow Editor" in the main application menu to the left.

For this demonstration we will create a new Workflow, but you can add the steps below to any of your existing Workflows if these run on a table extending the Task table.

Click the "New Workflow" button in the upper right corner.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_new.webp)

For this demonstration we will create a simple workflow that starts a Job and then waits for the Job to complete. Give the Workflow a Name, e.g., "Run Hello World Runbook" and select the table on which the Workflow will run. For this demonstration I have picked the "Requested Item" table. Click "Submit" to create the new workflow.

Select "Core" in the Menu to the right and locate the "Timer" action. Drag this to workflow canvas.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_timer.webp)

Give the activity a Name, e.g., "Wait 5 seconds" and set the Duration to 5 seconds. We do this to make sure that the Requested Item record is completely inserted and that all business rules have completed as we otherwise risk to see system_ID conflicts. You may not need this if you are implementing a Runbook execution in an existing workflow that has preceding activities. Click "Submit to create the new activity.

Next drag a "Run Script" action to the canvas by selecting and dragging it from the menu to the right.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_start_job.webp)

Give the activity a name. In this example we call it "Execute Runbook". Then paste in the code snippet that you copied earlier in this chapter.

Here is an explanation of the code:

**Line 1**: Here we set the “runOn” for the job. This needs to be the System_ID of the Hybrid Worker Group on which you would like the job to run on. For demonstration we set this to "null" as we do not wish to run this on a Hybrid Worker Group.

**Line 2**: Here we set the “runAs” for the job. Like "runOn" this needs to be a System_ID of a Credentials object in Azure. Use this if you want the job to run as a different user than the default user. We will use "null" to leave it to default for this demonstration.

**Line 3 – 5**: Here we create the parameters object. You can add as many as you like. Consult the Runbook to see what input is required.

**Line 6**: Here we create the runbook object. The guide, e7636ecedb0d9740e61a5040cf961988, is the System_ID of the runbook in ServiceNow that we want to execute. This is where you paste the System_ID of the "Hello-World" Runbook that we copied earlier.

**Line 7**: Here we create the job and assigned a variable to the workflow scratchpad. You do not need to assign the variable, but if you do so you get an easy way of following up on the job status.

To follow up on when the Job completes, we need to add a "Wait for condition" activity.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_wait_for_add1.webp)

Locate the "Wait for condition" activity and drag it to the canvas.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_wait_for_add2.webp)

Give the activity a name, e.g., "Wait for job completion" and enter the code as shown above.

Here is an explanation of what the code does:

**Line 1**: We create a new GlideRecord object on the Job table

**Line 2**: We set the default answer to false

**Line 3**: If we can get a record matching the System_ID that we created in the previous step we do further investigation

**Line 4 – 5**: If the status of the job is "Failed" or "Completed" we set the answer to true, meaning that the job has now completed.

Click "Submit" to add the "Wait for condition" to your workflow.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_show.webp)

Your workflow should now look like the workflow above. For production workflows you may want to add error handlers etc., but for our demonstration we will keep it simple and continue with this.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_publish.webp)

Now click the little hamburger icon and select "Publish" to make your workflow available. To test the workflow, we will create a simple catalogue item and attach the workflow to it.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_maintain_item_module.webp)

Close the "Workflow Editor" and select "Maintain Items" under "Service Catalog" -> "Catalog Definitions" in the main application menu.

Click the "New" icon in the upper left corner of the list.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_maintain_item_edit.webp)

Give it a "Name" and a "Short description", e.g., "Run Hello World Runbook" and select the "Run Hello World Runbook" that we just created in the "Workflow field" and click "Try It" in the upper right corner.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_catalog_item_show.webp)

This should show an order form similar to the one above. Click the "Order Now" button to kick of the workflow.

![Click on Tenants](/assets/images/x_autps_azure_auto_workflow_requested_item_confirmation.webp)

You should now see a screen like the one above confirming that your "order" has been submitted. Click the link to the requested item (Run Hello World Runbook) in the "Description" column.

Under "Related Links" you can now click the "Show Workflow" to monitor as the Job progresses.

![Click on Tenants](/assets/images/x_autps_azure_auto_job_module.png.webp)

To verify that the Job has completed as expected you can also go to "Automation App" -> "Jobs" and find the Job in the list. This is also described in the section: Manually start a Runbook
