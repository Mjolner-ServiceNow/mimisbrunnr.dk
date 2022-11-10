---
layout: single
title: Microsoft Azure Setup
excerpt: "Instructions for how to setup access and create needed assets in Microsoft Azure"
permalink: /rights-management-app/azure-setup/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

To be able to access your Active Directory from ServiceNow you need to have an Automation Account in Microsoft Azure that you can use. 

In the following guide you are going to walk you through on how to set up Microsoft Azure to get Rights Management App working in your ServiceNow instance. When you are ready, go to [portal.azure.com](https://portal.azure.com) and login with your azure credentials.

## Prerequisites

To complete this guide, you need to have sufficient rights in your Azure platform. You can complete this guide if you have the role of **Global Administrator** or **Application Administrator** on your Microsoft Azure Tenant or if **App Registration** has been enabled in you Azure Tenant and you have sufficient access to the subscription in which the Azure Automation Account should reside.

To check if **App Registration** is activated. Click on "Azure Active Directory" in the menu to the left and then select "User Settings". Ensure that the option "Users can register applications" is set to yes

## Create an App Registration 

Now we are ready to configure Azure. Go to **"App Registration"** in Azure and click on **"New Registration"**. Give a name to the application and click on **"Register"**. Copy and store the **"Application (client) ID"** and the **"Directory (tenant) ID"** because you will need them in following steps. 

## Generate a secret

In the Application you created, in the left side menu, click on **"Certificates & secrets"**. Add a **new client secret** with a meaningful Description and select an expiration date. Once you click **"Add"** a key will be generated. This is the only time that you will be  able to see this key, so make sure that you copy it and store it somewhere safe.

## Set up an Automation Account

If you do not already have an Automation Account follow this step to get one. 
Navigate to **Automation Account** from the main menu in Microsoft Azure and click on **"+Create"**. Give a Name, select a Subscription, create or select an existing Resource Group and select a Location. Click on **"Create"** button. 

**Give access to the Automation Account**

Now we need to give the application we created access to the Automation Account. Navigate to the Automation Account and click on **"Access Control (IAM)"**. Click on "Add" and then "Add role assignment". Select **"Contributor"** under **Role**, **Assign access to * "Azure AD user, group or service principal"** and under **Members** select the Application you registered in previous step.

You are now set and ready to setup your ServiceNow to be ready to use the Rights Management App.