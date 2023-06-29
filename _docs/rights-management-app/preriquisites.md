---
layout: single
title: Microsoft Azure Setup
excerpt: "Preriquisites needed to use the Rights Management App"
permalink: /rights-management-app/preriquisites/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

To be able to use the Rights Management App and access your Active Directory from ServiceNow you need to have the Automation App installed into your instance. You can do that form the [ServiceNow Store](https://store.servicenow.com) and you can follow the [installation guide for the Automation App](/automation-app/azure-setup/) to set it up on your ServiceNow instance.

## Register an application in Azure
When you are ready, go to portal.azure.com and log in with your Azure credentials. Go to "Azure Active Directory" in the menu to the left. Then click on "App registrations". This will give you a list of registered Apps. You can add an application by clicking "+ New registration" at the top left of the list.
![Request](/assets/images/aappregistration.webp)

Under "Name" you enter "ServiceNow" or something that makes sense to you. Under the account type select "Accounts in this organizational directory only". Then click the "Register" button at the bottom.
![Request](/assets/images/appregistrationtwo.webp)

Copy the Application (client) ID and Directory (tenant) ID.

![Request](/assets/images/appiddirectoryid.webp)

## Generate a client secret 
In order for the Automation App to connect with Azure Automation you need to generate a client secret . In your App click on "Certificates & secrets".



