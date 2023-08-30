---
author: LKH
title: "Microsoft will remove Run As Accounts - are you using it?"
---

Microsoft has announced the deprecation of **Run As Accounts** and will completely remove the feature on September 30th, 2023. So now is the time to go through your **Runbooks** to ensure, that you are ready for this change.

But how do you know if you are using this feature? Maybe you did not write all your **Runbooks** yourself or you are simply unsure if you are using **Run As Accounts**.

Here are some tips on how you can identify if you are using **Run As Accounts**.

## Verify if there is an active Run As Account

Navigate to [portal.azure.com](https://portal.azure.com) and open your Automation Account.

![Run as accounts in the azure portal](/assets/images/post_identify-if-you-are-using-run-as-accounts.webp)

Click on **Run as accounts** in the menu to the left and have a look at what has been created.

In the screenshot above we can see that there has been created an **Azure Run As Account**, so perhaps this has been in use at some point. However, the account expired last year, so if you have not experienced any issues lately, you are likely not using this account.

We can also see that there has not been created any **Azure Classic Run As Account**, so this we do not have to worry about.

## Go through your Runbooks

If you have an active **Run as account**  you will want to go through your **Runbooks** to verify if they are in use anywhere.

The CMDLET **Get-AutomationConnectio** is what we are mainly looking for. So you should look for code similar to the below:

```powershell
$connection = Get-AutomationConnection -Name $connectionAssetName  
```

Automation Connections are still supported, so it is only if this connection is using the **Run as account** for authentication that we must update it. 

If the connection name is *AzureRunAsConnection* or *AzureClassicRunAsConnection* you would need to take action.

## What should I do if I have identified, that I am using a Run As Account?

A **Run As Account** is basicly an application, so it is easy to replace. Instead of using the **Run As Account** simply register an application in Azure Active Directory and give it the access required for your automation to work.

Then add a secret or a certificate and use the application for authentication instead.

We have few tutorials, like [this one](/automation-app/azure-ad-integration/), that uses this approach for authentication.