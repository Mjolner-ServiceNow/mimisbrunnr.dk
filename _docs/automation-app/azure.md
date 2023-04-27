---
layout: single
title: Microsoft Azure Setup
excerpt: "Instructions for how to setup access and create needed assets in Microsoft Azure"
permalink: /automation-app/azure-setup/
last_modified_at: 2023-01-23,T14:21:18-04:00
sidebar:
  nav: "aa"
toc: true
---

To manage Runbooks in Azure the Automation App needs contributor access to the Automation Accounts that you wish to use. You can also give contributor access to the app on a subscription or resource group level. This way the app will have access to all Automation Accounts in each subscription or resource group.

In the following guide we will give access to a specific Automation Account only. Please consult your Azure administrator to ensure that the access is setup to best fit your environment.

When you are ready, go to [portal.azure.com](https://portal.azure.com) and login with your azure credentials.

## Prerequisites

To complete this guide, you need to have sufficient rights in your Azure platform. You can complete this guide if you have the role of **Global Administrator** or **Application Administrator** on your Microsoft Azure Tenant or if **App Registration** has been enabled in you Azure Tenant and you have sufficient access to the subscription in which the Azure Automation Account should reside.

To check if **App Registration** is activated. Click on "Azure Active Directory" in the menu to the left and then select "User Settings". Ensure that the option "Users can register applications" is set to yes

## Create an App Registration

Now we are ready to configure Azure. Go to "Azure Active Directory" in the menu to the left. Then click on "App registrations". This will give you a list of registered Apps. If ServiceNow is already on this list, simply click on it to open it. If not, you can add it by clicking the little "+ New registration" at the top left of the list.

![Register an application](/assets/images/x_autps_azure_auto_register_app.webp)

Under "Name" you enter "ServiceNow" or something that makes sense to you. Under the account type select "Accounts in this organizational directory only". Under "Redirect URI" add the link to your instance followed by "/login.do". Ex. https://myinstance.service-now.com/login.do. Then click the "Register" button at the bottom.

> TIP: If you have configured single sign on for your ServiceNow instance using Azure Active Directory, there should already be a registered App that you can use.

Once the Application is created you will see a page that looks like below. If the menu to the right is not visible, click the "Settings" button in the upper left corner.

![Copy Application ID and Directory ID](/assets/images/x_autps_azure_auto_app_details.webp)

Copy and store the "Application (client) ID" and "Directory (tenant) ID" as you will need this later to configure ServiceNow.

### Generate a secret

If you would like Automation App to authenticate with Microsoft Azure using a client secret you must generate a secret. To generate one, click the "Certificates & Secrets" under "Manage".

![Click on New client secret](/assets/images/x_autps_azure_auto_create_client_secret1.webp)

Click on the "New client secret" button to generate a new client secret.

![Add a client secret](/assets/images/x_autps_azure_auto_create_client_secret2.webp)

Enter something meaningful in the "Description" field and select an expiration date. For ease of use I selected "Never". Then click "Add".

![Copy the client secret](/assets/images/x_autps_azure_auto_create_client_secret3.webp)

Once you click "add" a key will be generated. This is the only time that you will be able to see this key, so make sure that you copy it and store it somewhere safe together with the "Application (client) ID" and "Directory (tenant) ID" that you copied earlier.

### Generate a certificate

If instead you wish to use a certificate you need to uplade the certificate you wish to use to Azure. Here is an example of how you can generate a certficate.

To create a new PEM certificate in a CRT file, write the following command, where "test1-key" is the name of the key and "test-cert" is the name of the certificate.

`openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout test1-key.key -out test1-cert.crt`

Next convert the key and the certificate into a PKCS#12 certficate where "test1-certificate" is the name of the combined certificate.

`openssl pkcs12 -export -out test1-certificate.pfx -inkey test1-key.key -in test1-cert.crt`

Lastly add the PFX file to a Java Key Store. Where "test1-jks" is the name of the JKS file and "test_cert" is the entry name (alias) that the certficate will be saved as in the Jave Key Store.

`keytool -importkeystore -srckeystore test1-certificate.pfx -srcstoretype PKCS12 -destkeystore test1-jks.jks -srcalias 1 -destalias test_cert`

We will be using the JKS file later when we setup ServiceNow. Make sure to remember the entry name and the password.

![Upload the certificate](/assets/images/x_autps_azure_auto_create_client_certificate.webp)

The CRT file should be uploaded to Azure, by clicking the certificates and then click "Upload".

## Setup an Automation Account

If you do not already have an Automation Account, now is the time to create one. To do this select "Automation Accounts" from the main menu to the left.

![Add Automation Account](/assets/images/x_autps_azure_auto_create_automation_account.webp)

Give the Automation Account a Name and select the Subscription. Select an existing Resource Group or create a new one. Select the location and choose if you would like to create an Azure Run As account. Click "Create" once everything is filled out.

> Notice: It may take a few seconds before the Automation Account is created. Click "Refresh" on the list until you see the Automation Account and then click on the name to open the Automation Account.

### Give access to an Automation Account

Now we need give the app access to your Automation accounts.

![Click on Access control (IAM)](/assets/images/x_autps_azure_auto_setup_iam1.webp)

Click on "Access control (IAM)" in the menu to the left.

![Click on Add role assignment](/assets/images/x_autps_azure_auto_setup_iam2.webp)

Click on "+ Add" button at the top of the page and select "Add role assignment".

![Add role assignment](/assets/images/x_autps_azure_auto_setup_iam3.webp)

Select "Contributor" under "Role". Next search for the ServiceNow application that we previously created and select it. Then click "Save".

## Summary and notes

We are now done setting up Azure and are ready to configure ServiceNow. Ensure that you have recorded the following information:
* Application (client) ID
* Directory (tenant) ID
* Client secret or Java Key Store with a password and entry name
