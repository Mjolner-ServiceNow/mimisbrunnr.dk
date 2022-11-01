---
layout: single
title: Azure Automation Credentials
permalink: /automation-app/credentials/
last_modified_at: 2022-10-25T08:16:18+01:00
sidebar:
  nav: "aa"
toc: true
---

Azure Automation Credentials are a powerfull and secure way to store credentials in your Automation Account. The credentials can then be access from multiple Runbooks and centrally managed using **Automation App** and/or the **Azure Portal**

## Create a new Credential

You have two options for creating a new Credential using Runbook Manager.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_credential0.webp)

From the main menu of Runbook Manager navigate to **Credentials** and click on **Create new**.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_credential1.webp)

Alternatively you can obtain the same by click on **Credentials** if you have an open Runbook and then click on **Create new**.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_credential2.webp)

In either case a modal will appear in which you can fill in the information around your credential.

## Accessing a Credential

Accessing a Credential in a Runbook is very useful. The easiest way to do so is by using the menu to the left.

![Click on Dashboard](/assets/images/x_autps_azure_auto_get_credential.webp)

1. Click on the credential that you would to get and click on **Get-AutomationPSCredential**. This will copy a snippet to your clipboard.
2. Write a credential name and an equal sign, eg. "$cred = ".
3. Paste in the snippet from your clipboard.

## Updating a Credential

You can edit the value of the credential from the Runbook Manager.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_credential0.webp)

Click on the pencil next to the credential you wish to edit.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_credential1.webp)

Update the value and click on **Update**.

## Delete a Credential

It is possible to delete a credential that you no longer need.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_credential0.webp)

Click on the pencil icon next to the credential you wish to delete.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_credential1.webp)

Click the **Delete** button.
