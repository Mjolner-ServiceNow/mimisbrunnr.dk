---
layout: single
title: ServiceNow Setup
permalink: /rights-management-app/servicenow-setup/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

In this guide we will configure **Rights Management App** in ServiceNow to be able to connect to your Active Directory. To complete this guide you must have the role of **Admin** in ServiceNow.

## Create ServiceNow Credentials

Navigate to the Users List under User Administration into ServiceNow and create a new user.

![User list](/assets/images/useradminstation.webp)

Give a username and a password to the user and copy and store them because you will need them. Set the Web Service access only field to true. Give the user the **"x_autps_active_dir.user"** and **"x_autps_active_dir.service"** role.

![Register a user](/assets/images/x_autps_active_dir_user.webp)

Then go to your Azure Automation Account click on **"Credentials"** on the left-side menu and **"Add a credential"**. 

![Create credentials](/assets/images/x_autps_active_dir_create_cred.webp)

Set the Name and username to be the one you copied and stored and set the password to be the password you copied and stored.

![Create credentials username password](/assets/images/x_autps_active_dir_cred.webp)

## Create a Domain and connect to your Active Directory

Go to the **Domains** list in Rights Managament App and click on **New**. Give a meaningful name to your Domain. In the Setup Section set the value of the **Domain Controller IP** to be the IP address of your Hybrid Worker. Next set the value of the **Automation Account** to be equal to the Automation Account you are going to use. Set the Active Directory Credentials to be the credentials of the admin in your Active Directory and the ServiceNow Credentials to be the credentials you created in step Create ServiceNow Credentials.  Select your Hybrid worker group and click on Update. A Runbook will be automatically created for you and this will be used to fetch User, Groups and Group memberships from your Active Directory and import them into your Domain in your ServiceNow instance.

![Create domain](/assets/images/x_autps_active_dir_domain.webp)