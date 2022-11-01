---
layout: single
title: Templates
excerpt: "Instructions for how to setup access and create needed assets in Microsoft Azure"
permalink: /automation-app/templates/
last_modified_at: 2022-10-24T16:36:18+01:00
sidebar:
  nav: "aa"
toc: true

---

At Automize Software we want to give you the best possible start to your automations. Thus, we have made several templates available for different technologies that you can use free of charge.

With Runbook Manager you have access to this ever-increasing list of Runbook Templates that you can use to fast track your automation project.

In the previous section we saw how we could [Create a Runbook](/automation-app/runbooks/#create-a-runbook). In the below we will apply a template to a blank Runbook, but you can apply a Template to any existing Runbook of your choosing.

## Apply Template

 Locate the Runbook that you wish to apply the Template to in the list of Runbooks and click on it to open it.

> Notice: You can only edit Runbooks with Runbook type "PowerShell" or "Python" in ServiceNow.

If the Runbook is not already in edit mode, click the "Edit" button in the upper right corner to edit the Runbook. If the runbook is already in edit mode, the "Edit" button will not be visible.

![Put Runbook in Edit Mode](/assets/images/x_autps_azure_auto_create_runbook2.webp)

Next locate the template that you wish to apply by clicking on **Templates** in the menu to the left. In this example we will use the **DocuSign** -> **New Envelope**. Once you click on a template the template will be copied to your clipboard.

![Apply a Template](/assets/images/x_autps_azure_auto_add_template.webp)

1. Click on Templates
2. Click on DocuSign
3. Click on New Envelope
4. Mark the code of your Runbook and then paste in the template from your clipboard

You can now start to modify the template to make it fit to your needs.

## Template structure

All templates follow the same structure.

### Parameters
The first section is the parameters. These are the parameters that the Runbook is expecting to get from ServiceNow.

You do not have to make any modifications to this section unless you would like add additional parameters or if you would like remove some and then hardcode them into your script.

### Environment Variables

This section expects you to enter names of variables and credentials that contains information specific to your environment.

We always strive to use variables or credentials stored in Azure Automation and we thus expect you to provide the **name** of these assets. This provides you with an easier to maintain these.

### Script
The idea with the script is that it needs no modification, but will work out of the box. You thus do not have to modify anything in this section, but you are free to do so if you so desire.

Whenever possible we try to wrap the script in a **Try, Catch, Finally** block to ensure that any error can be properly handled and reported back to ServiceNow.

## Updating templates

The templates will be automatically imported to your ServiceNow instance on a daily basis. Thus you can be sure that you will always have the latest list of templates available in your ServiceNow instance.

If you would like to perform an ad hoc update you can do so by locating the scheduled job called **Import templates** in your ServiceNow instance and click on **Execute Now**.

## Access templates from GitHub

You can also access the templates directly on [GitHub](https://github.com/Automize-Software/AzureRunbooks) if you prefer to get to them this way.

Remember to stay tuned as we continuously will add new templates to the list based on customer feedback.

## Submit or request new template

If you have any templates that you think that we are missing or if you have suggestions for improvements to any existing templates, then please let us know.
