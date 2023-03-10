---
layout: single
title: Setting up Hybrid Workers
excerpt: "Instructions for how to setup hybrid workers to access on premise resources"
permalink: /automation-app/hybrid-worker/
last_modified_at: 2023-03-01,T10:10:18-04:00
sidebar:
  nav: "aa"
toc: true
---

If you want to access resources that does not have a public facing endpoint or if you have special requirements for software that you wish to use you will need a **Hybrid Worker**.

A **Hybrid Worker** can be any machine that runs a supported version of Linux or Windows. It does not have to be a server and it does not have to be a dedicated machine.

## Hybrid Worker Groups

All **Hybrid Workers** are added to **Hybrid Worker Groups**, which is then discovered by Automation App. 

When you start a **Runbook** you can select if it shoud be executed directly in Azure or on a **Hybrid Worker** in a **Hybrid Worker Group**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker0.webp)

In **Flow Designer** this is done by selecting the **Hybrid Worker Group**, that you want to execute the selected **Runbook**, in the **Run on** field. If this field is left blank the Runbook will execute on **Azure**. Read more about using **Flow Designer** [here](/automation-app/flow-designer/).

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker1.webp)

When working in **Runbook Manager** you can select the **Hybrid Worker Group** after clicking to **Run** or **Test** button by filling out the **Hybrid Worker Group** field. If this field is left at **--None--** then **Runbook** will be execute on **Azure**. Read more about working with **Runbooks** in **Runbook Manager** [here](/automation-app/runbooks/).

It is recommended to have more than one **Hybrid Worker** in each **Hybrid Worker Group**, but it is not a requirement.

## How does a Hybrid Worker work

The **Hybrid Worker** will create an outbound connection to your **Automation Account** in your **Microsoft Azure Tenant** and ask for jobs to complete. When a job is assigned to a **Hybrid Worker Group** a **Hybrid Worker** from this group will pick op the job and exectue the **Runbook** locally on the machine.

This enables the **Runbook** to utilize software installed on the **Hybrid Worker** or to access any local resource that the **Hybrid Worker** has access to. The **Hybrid Worker** will allways fetch the latest version of the Runbook which is stored in your **Automation Account**.

## How to create a Hybrid Worker

There are multiple ways to deploy a **Hybrid Worker**. We recommend the **Extension-based worker** and will briefly explain how this is done here. For detailed instructions on how to install a **Hybrid Worker** please refer to this [article](https://learn.microsoft.com/en-us/azure/automation/extension-based-hybrid-runbook-worker-install) on Microsoft Learn.

The **Extension-based worker** only works for machines that are known to Azure. If your machine is not an Azure machine, you can create an **Agent-based worker**. Se instructions [here](https://learn.microsoft.com/en-us/azure/automation/automation-windows-hrw-install).

### Create a Hybrid Worker Group

Open the **Azure Portal** and navigate to the **Automation Account** that you wish to create a **Hybrid Worker Group** for.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker2.webp)

1. Click on **Hybrid worker groups**.
2. Click on **Create hybrid worker group**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker6.webp)

1. Fill out the **Name** field with a name for your **Hybrid Worker Group**.
2. Set **User Hybrid Worker Credentials** to **Default**.
3. Click on **Next**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker3.webp)

Click on **Add machines**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker4.webp)

Select that machines that you wish to add as **Hybrid Workers** and click **Add**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker5.webp)

Click on **Review + Create**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker7.webp)

Review that everything looks as expected and click on **Create**.

### Add a Hybrid Worker

If you wish to add a **Hybrid Worker** to an already existing **Hybrid Worker Group** you can navigate to the **Automation Account** on the **Azure Portal**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker8.webp)

Then click on **Hybrid worker groups** and then on the **Hybrid Worker Group** that you wish to add the machine to.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker9.webp)

Next click on **Hybrid Workers** and then on **Add**.

![Start Runbook using a Hybrid Worker Group from Flow Designer](/assets/images/x_autps_azure_auto_hybrid-worker10.webp)

Select the machines you would like to add as **Hybrid Workers** and click **Add**.