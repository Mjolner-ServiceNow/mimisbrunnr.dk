---
layout: single
title: Setup
excerpt: "A set up guide that walks you through the process og installing Rights Management App step by step."
permalink: /rights-management-app/setup/
last_modified_at: 2022-11-10 08:26:01
sidebar:
  nav: "rma"
toc: true
---
To be able to use Rights Management App you need to have an Automation Account.

When you are ready, go to [portal.azure.com](https://portal.azure.com) and login with your azure credentials.

## Register an application in Azure
Go to "Azure Active Directory" in the menu to the left. Then clcik on "App registrations". If you have not an application already registered click the button "+New Registration" at the top left of the list. Give a name to the app and click on Register. Copy and store the Application (client) ID and Directory (tenant) ID.

## Generate a client secret 

Generate a client secret by clicking on "Cerificates & secrets" under "Manage". Click on the "New client secret" button to generate a client secret. Enter something meaningful in the "Description" field and select the expiration date. Once you click "Add" a key will be generated. This will be the only time you will be able to see the key so make sure that you copy it and store it somewhere safe along with the Application (client) ID and Directory (tenant) ID that you copied earlier.

## Create an Automation Account in ServiceNow
This Automation Account will create a link between your ServiceNow instance and your Regired Application in Azure. 

Navigate to the Tenant list under Automation App module in your ServiceNow instance. Click on "New" button. Set a name for the new Tenant and add the Directory (tenant) ID you collected in the "Register an application in Azure" section in this guide. Right click on the top grey bar and select **Save**.
Then scroll down to the bottom of the form and click New in the Applications related list of the Tenant form.
Give a meaningful name to the new application, input the Appication (client) ID and Key you copied earlier and right click on the grey bar and select Save.

## Connect your Automation Account with the ServiceNow application
Once you saved the new application scroll down to the Scheduled Imports related list of the Application form. Click on the name of your applications' scheduled import record and then click on the Execute Now button. The state of the application should change to "Connected".

## Domain creation 
Navigate to Domains list in Rights Management App module and click on the New button. Give a name to your Domain. 