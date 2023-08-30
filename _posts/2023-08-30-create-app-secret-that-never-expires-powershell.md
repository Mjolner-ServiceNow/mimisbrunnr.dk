---
author: LKH
title: "How to create an App Secret that never expires"
---

If you are using the Azure Portal UI you may have noticed that the maximum lifetime of a secret is two years. This is however only a limitation of the UI and it is possible to create secrets with a longer lifespan.

This approach will show you how you can use PowerShell to create secrets with an expiration time of your choosing.

## Get the Object ID of your App Registration

Navigate to [portal.azure.com](https://portal.azure.com) and open the App Registration for which you would like to create a secret.

![Run as accounts in the azure portal](/assets/images/post_create-app-secret-that-never-expires-powershell.webp)

Locate the **Object ID** of your App registration and copy it.

## Use PowerShell to create your new secret

We are ready to create the secret, so fire up your favorite PowerShell editor.

You must have the [Azure Active Directory PowerShell module](https://www.powershellgallery.com/packages/AzureAD/) to be able to create the secret. Run below code to install it, if you don't already have it.

```powershell
Install-Module -Name AzureAD 
```

Next paste in the following code.

```powershell
# Input
$objectId = "bb32d329-b30c-4b4b-97cb-17de0768541c"
$description = "Unlimited"
$years = "999"

# Setup a connection
Connect-MgGraph -Scopes 'Application.ReadWrite.All'

# Create the secret
$pw = @{
    displayName = $description
    endDateTime = (Get-Date).AddYears($years)
}
$Secret = Add-MgApplicationPassword -ApplicationId $objectId -PasswordCredential $pw

# Output the secret
$Secret | Format-List
```

Replace the **$objectId** value with the Object ID of your application and fill in a meaningfull description in the **$description** variable.

You can lower the **$years** parameter if you want the secret to expire sooner.

Make sure to copy the value of the secret you just generated.