---
layout: single
title: Azure Automation Variables
permalink: /automation-app/variables/
last_modified_at: 2022-10-13T08:16:18+01:00
sidebar:
  nav: "aa"
toc: true
---

Azure Automation Variables are a powerfull and secure way to store variables in your Automation Account. The variables can then be access from multiple Runbooks and centrally managed using **Automation App** and/or the **Azure Portal**

## Create a new variable

You have two options for creating a new Variable using Runbook Manager.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_variable0.webp)

From the main menu of Runbook Manager navigate to **Variables** and click on **Create new**.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_variable1.webp)

Alternatively you can obtain the same by click on **Variables** if you have an open Runbook and then click on **Create new**.

![Click on Dashboard](/assets/images/x_autps_azure_auto_add_variable2.webp)

In either case a modal will appear in which you can fill in the information around your variable.

## Accessing a Variable

Accessing a Variable in a Runbook is very useful. The easiest way to do so is by using the menu to the left.

![Click on Dashboard](/assets/images/x_autps_azure_auto_get_variable.webp)

1. Click on the variable that you would to get and click on **Get-AutomationVariable**. This will copy a snippet to your clipboard.
2. Write a variable name and an equal sign, eg. "$var = ".
3. Paste in the snippet from your clipboard.

## Updating a Variable

You can update a variable when the Runbook executes by using the **Set-AutomationVariable** snippet from the menu.

You can also edit the value of the variable from the UI.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_variable0.webp)

Click on the pencil next to the variable you wish to edit.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_variable1.webp)

Update the value and click on **Update**.

## Delete a variable

It is possible to delete a variable that you no longer need.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_variable0.webp)

Click on the pencil icon next to the variable you wish to delete.

![Click on Dashboard](/assets/images/x_autps_azure_auto_update_variable1.webp)

Click the **Delete** button.
