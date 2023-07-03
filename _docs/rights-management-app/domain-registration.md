---
layout: single
title: ServiceNow Setup
permalink: /rights-management-app/domain-registration/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

## Create Domain

The last step of the process is to create a Domain in the Rights Management App so you will be able to manage your Active Directory and Azure Active Directory from ServiceNow. To do so you need to navigate to the Domain list in the Rights Management App menu. Once you selected the Domain list then click on New. Give a meaningful Name to the Domain. In the Setup section select the Automation Account you wish to work with. Fill the ServiceNow credentials field with the credentials created previously. Then select the Hybrid Worker you wish to use. 

Next, navigate to the Active Directory Setup. If you wish to manage your Active Directory from ServiceNow then select Enable Active Directory. Here you need to give the Domain Controller IP and the Active Directory Credentials which should be of an admin user in your Active Directory ( those credentials should be added to your Automation Account, if not then navigate to your Automation Account in Azure and go to the Credentials menu in the left side menu. Click on "+ Add a credential" and fill in the information for the admin user you are going to use. Then click on Create. In ServiceNow navigate to the Scheduled Imports of the Automation App find your application Scheduled Import open it and click on Execute Now. The credentials should be now imported to your automation account and be ready to use in the Active Directory Credentials field of the Active Directory setup in the domain form). 

On the Azure Active Directory Setup section select Enable Azure Active Directory if you wish to manage your Azure Active Directory from ServiceNow. Then fill in the Tenant Azure Active Directory, the Application ID( Which should be the application ID of the app created in the previous step) and the Client secret or Thumbprint.

Click on Submit.

The Runbook field will be populated automatically.

## Domain ready
If all steps were followed and implemented correctly you should see after some minutes the Sync State change to Ready and the related lists of Organizational Units, Users and Groups being populated with information.