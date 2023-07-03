---
layout: single
title: ServiceNow Setup
permalink: /rights-management-app/servicenow-setup/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

In this guide we will configure **Rights Management App** in ServiceNow to be able to connect to your Active Directory. To complete this guide you must have the role of **Admin** in ServiceNow and Automation App should be alreaady installed and set up in your environment. Here is the guide to [setup the Automation App](/automation-app/azure-setup/).

## Create ServiceNow Credentials

Navigate to the Users List under User Administration into ServiceNow and create a new user.

![User list](/assets/images/x_autps_active_dir_useradminstation.webp)

Give a username and a password to the user and copy and store them because you will need them. Set the Web Service access only field to true. Give the user the **"x_autps_active_dir.user"** and **"x_autps_active_dir.service"** roles.

![Register a user](/assets/images/snowuser.webp)

Then go to your Azure Automation Account click on **"Credentials"** on the left-side menu and **"Add a credential"**. 

![Create credentials](/assets/images/x_autps_active_dir_create_cred.webp)

Set the Name and username to be the one you copied and stored and set the password to be the password you copied and stored.

![Create credentials username password](/assets/images/x_autps_active_dir_cred.webp)

## Create a Tenant in ServiceNow

Open the Automation App in the Navigator of ServiceNow. Click on Tenants and then click on the New icon at the top left of the list.

![Register a user](/assets/images/tenantmenu.webp)

Give a name to the Tenant and set the Directory (tenant) ID to be the Directory (tenant) ID value copied in the previous step. Right-click on the gray header and click on Save.

![Register a user](/assets/images/createtenant.webp)

Then scroll down to the bottom of the form: There you should see an "Applications" related list. Click on the New button.

![Register a user](/assets/images/tenantcreated.webp)

Give a Name to the application. Set the Application ID value to be equal to the Application ID you copied from your Application registration in the previous step. Set the Client secret to be equal to the value of the client secret you copied in the previous step. Lastly, right-click on the grey header and click on "Save".

![Register a user](/assets/images/createappservicenow.webp)

Once you clicked on "Save" you should be able to see a "Scheduled Imports" related list.

![Register a user](/assets/images/appimportsn.webp)

Click on the scheduled import record for your application and then click on "Execute Now".

![Register a user](/assets/images/executeimportsn.webp)

After this step you should be able to see your Automation Account in the Automation Accounts list of the Automation App.

