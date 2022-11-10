---
layout: single
title: ServiceNow Setup
permalink: /rights-management-app/servicenow-setup/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

In this guide we will configure **Rights Management App** in ServiceNow to be able to connect to your Active Directory. To complete this guide you must habe the role of **Admin** in ServiceNow.

To be able to use the Rights Management App you need to have installed and running the Automation App from Automize. If you do not already have the Automation App then go to [store.servicenow.com](https://store.servicenow.com) and download the Automation App from Automize. Follow the guide [Automation App Setup](https://automation-app/servicenow-setup/) to set the Automation App on your ServiceNow instance.

## Create ServiceNow Credentials

Navigate to the Users List under User Administration into ServiceNow and create a new user. Give a username and a password to the user and copy and store them because you will need them. Give the user the **"x_autps_active_dir.user"** and **"x_autps_active_dir.service"** role. Then go to your Azure Automation Account click on **"credentials"** on the left-side menu and **"Add a credential"**. Set the Name and username to be the one you copied and stored and set the password to be the password you copied and stored.

## Setup connection to Microsoft Azure

Open the **Automation App** in the Navigation of ServiceNow and click on **Tenants**. Then click on the **New** button at the top left of the list. Give  Name to your Tenant and add the Tenant ID that you have collected in the Azure setup section of this guide. Right click on the top grey bar and select **Save**. 

Next click on the **New** button in the **Applications** related list of the Tenant record. Give tour applicationa name, input the Application ID and client secret you collected in the Azure setup section of this guide. 

To test that the connection works, open the application record that you just created and click the **Test connection** button in the upper right corner. A new window opens. Click on the **Test connection** button. You should get a **Connection seccessful. Return code 200** message.

If you do not get this, you need to revisit the data that you have entered and ensure that you have given sufficient access to the application in Azure.

## Connect to your Azure Automation Account

Navigate to the application you created in ServiceNow. Go to the **Scheduled Imports** related list and click the name of your application to navigate to your applications scheduled imports. Once you are in your applications scheduled imports form click on the **Execute Now** buttonn in the upper right corner of the form.

Now you should be able to see your Automation Account in the **Automation Account** list of the **Automation App** in you ServiceNow instance.

## Create a Domain and connect toyour Active Directory

Go to the **Domains** list in Rights Managament App and click on **New**. Give a meaningful name to your Domain. In the Setup Section set the value of the **Domain Controller IP** to be the IP address of your Hybrid Worker. Next set the value of the **Autmation Account** to be equal to the Automation Account you configured in the previous step. Set the Active Directory Credentials to be the credentials of the admin in your Active DIrectory and the ServiceNow Credentials to be the credentials you created in step Create ServiceNow Credentials.  Select your Hybrid worker group and click on Update. A Runbook will be automatically created for you and this will be used to fetch User, Groups and Group memberships from your Active Directory and import them into your Domain in your ServiceNow instance.