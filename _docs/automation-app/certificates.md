---
layout: single
title: Azure Automation Certificates
permalink: /automation-app/certificates/
last_modified_at: 2023-01-10T08:16:18+01:00
sidebar:
  nav: "aa"
toc: true
---

Azure Automation Certificates are a powerfull and secure way to store certificates in your Automation Account. The certificates can then be access from multiple Runbooks and centrally managed using **Automation App** and/or the **Azure Portal**

## Create a new certificate

You have two options for creating a new Certificate using Runbook Manager.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_certificate0.webp)

From the main menu of Runbook Manager navigate to **Certificates** and click on **Create new**.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_certificate5.webp)

Alternatively you can obtain the same by clicking on **Certificate** if you have an open Runbook and then click on **Create new**.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_certificate6.webp)

In either case a modal will appear in which you can fill in the information around your Certificate.

## Accessing a Certificate

Accessing a Certificate in a Runbook is very useful. The easiest way to do so is by using the menu to the left.

![Click on Dashboard](/assets/images/x_autps_azure_auto_get_certificate.webp)

1. Click on the certificate that you would to get and click on **Get-AutomationCertificate**. This will copy a snippet to your clipboard.
2. Write a variable name and an equal sign, eg. "$cert = ".
3. Paste in the snippet from your clipboard.

## Updating a Certificate

You can update the descrition af a certificate from the UI.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_certificate0.webp)

Click on the pencil next to the certificate you wish to edit.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_certificate1.webp)

Update the value and click on **Update**.

## Delete a Certificate

It is possible to delete a Certificate that you no longer need.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_certificate0.webp)

Click on the pencil icon next to the Certificate you wish to delete.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_certificate1.webp)

Click the **Delete** button.
