---
layout: single
title: Microsoft Azure Active Directory Integration
permalink: /automation-app/azure-ad-integration/
last_modified_at: 2023-02-28T11:36:18-04:00
sidebar:
  nav: "aa"
toc: true
read_time: true
---

In this guide we will show you how you can create a user in Azure Active Directory from ServiceNow.

We will keep this guide simple and only focus on user creation, but you should get the knowledge you need to be able to easily expand this to also manage groups, group memberships, licenses etc.

## Setup Azure Active Directory Integration

We will be using an App Registration (Service Principal) in combination with a certificate to authenticate with Azure Active Directory. 

To do so we will first generate a certificate that we will use for authentication and next configure the App Registration in Azure.

You must have suffient administrative access to Azure Active Directory (Eg. Global Administrator) to complete this part.

### Create Certificate

To create a new certificate, write the following command, where "test1-key" is the name of the key and "test-cert" is the name of the certificate to be created.

```
openssl req -x509 -sha1 -nodes -days 365 -newkey rsa:2048 -keyout test1-key.key -out test1-cert.crt
```

Next convert the key and the certificate into a PKCS#12 certficate where "test1-certificate" is the name of the combined certificate.

```
openssl pkcs12 -export -out test1-certificate.pfx -inkey test1-key.key -in test1-cert.crt
```

We now have 3 files. **test1-key.key** is our private key, **test1-cert.crt** is our public key, while **test1-certificate.pfx** is a password protected file that contains both our private and public key.

You can delete **test1-key.key** as we will not be needing this. The two other files you should keep for now.

### Create App Registration

Go to [portal.azure.com](https://portal.azure.com) and login with your azure credentials.

In the Azure Portal go to "Azure Active Directory" in the menu to the left. Then click on "App registrations". This will give you a list of registered Apps. Click the little "+ New registration" at the top left of the list.

![Register an application](/assets/images/x_autps_azure_auto_aad0.webp)

Give the application a meaningfull name and click on **Register**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad1.webp)

Copy the **Application (client) ID** and the **Directory (tenant) ID**. We will need this later.

Click on **Certificates & Secrets**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad2.webp)

Click on **Certificates** and select **Upload certificate**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad3.webp)

Find the public key (crt) that we created in the previous section.

It is recommended to fill out the description field, but this is not required. 

Click **Add**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad4.webp)

Verify that the certifcate was uploaded correctly. If you have multiple certificates you can use the **Thumbprint** to identify the certificate.

### Setup permissions on for the application

Next we will give the application access to manage Azure Active Directory. To do so navigate to Azure Active Directory on the [Azure Portal](https://portal.azure.com).

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad5.webp)

Click on **Roles and administrators**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad6.webp)

Search for **User Administrator** and click on the role.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad7.webp)

Click on **Add assignments** and search for the application that we previously created.

Select the application by clicking on it and then click on **Add**.


### Add PKCS#12 certificate to Automation Account

Next we will add the certificate to the Automation Account that you wish to use. 

First navigate to your Automation Account on the [Azure Portal](https://portal.azure.com).

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad8.webp)

Select **Certificates** in the menu to the left and click on **Add a certificate**.

Give the certificate a name and add an optional description.

Select the PKCS#12 certificate (pfx) that we created in the previous section and type in the password that you used when it was created.

Set **Exportable** to **Yes** and click on **Create**.

### Add Tenant ID and Applocation ID variable to Automation Account

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad9.webp)

Click on **Variables** in the menu to the left and click on **Add a variable**.

Give the variable a meaningfull name and an optional description.

Set the type to **String** and paste in the **Application (client) ID** that you copied in the previous section.

There is no reason to encrypt this value, so you can set **Encrypted** to **No**.

Click **Create**.

Repeat the above to also save the **Directory (tenant) ID** as a variable.

### Verify that prerequisite modules are installed

To be able to execute the runbook that we will soon create in Automation App the following modules must be installed in the Automation Account.

- AzureAd
- Az.Accounts
- Az.Automation
- Az.Resources

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad10.webp)

Click on **Modules** and search for the modules. Make sure they appear in the list with status **Available** with the Runtime version 5.1.

If one or more modules are missing install them by click the **Add a module** and follow the instructions. 

> Notice that if you do not have the Az.Accounts module installed you will have to wait for the import of it to complete before you can install the modules.

### Import certificate and variables to ServiceNow.

Since we have made added the certificate and variables directly to Azure using the Azure Portal, ServiceNow may not yet be aware of them.

To ensure that ServiceNow is up to date with Azure run the scheduled import. In order to do so go to **Automation App** -> **Configuration** -> **Scheduled imports** and start the import.

Wait af few minutes for the import to complete.

### Create Runbook in Automation App

Next we will setup the runbook in ServiceNow using Automation App.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_menu_runbooks.webp)

In ServiceNow open Runbook Manager in the Application Navigator

![Select Runbooks in Runbook Manager](/assets/images/x_autps_azure_auto_create_runbook0.webp)

Select **Runbooks** i the main menu in **Runbook Manager**.

Make sure that you are in the same **Automation Account** as you created the certificate and the two variables in previously and click **Create** at the bottom of the list.


![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad11.webp)

Enter a name for the **Runbook** and set the **Type** to **PowerShell 5.1**.

Click **Create** to create the **Runbook**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad12.webp)

Open the newly create **Runbook** by clicking on it in the list.


![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad13.webp)

1. Click on **Edit**
2. Click on **Templates**
3. Click on **Microsoft Azure Active Directory** to expand the list templates
4. Click on **New User**

The template is now copied to your clipboard.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad14.webp)

Click on the canvas and paste in the template. 

Then copy the names of the two variables that you created and insert them into **ClientIDVariableName** and **TenantVariableName**.

Also copy the name of certificate and insert it into the **CertificateName** variable.

Then click on **Save** and next on **Test**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad15.webp)

1. Insert a **password** that lives up the required complexity level of your Azure Active Directory.
2. Insert a valid **username** for the new user. Notice that you must include the domain.
3. Enter a **Display name** for the new user.
4. Set **Hybrid Worker Group** to **--None--** as we will run this directly on Azure.
5. Click **Create** to start the test job.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad16.webp)

Await that the **State** changes to **Completed** to indicate that the user was successfully created in Microsoft Azure Active Directory.

Notice that a lot of usefull information about the new user is available in the **Extracted output variables**. You may want to save this information or use it for subsequent tasks when we start working with Flow Designer.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad17.webp)

To verify that the user was create you can also go to the Azure Portal and navigate to **Azure Active Directory** -> **Users** and search for the new user. You can safely delete the user, as this was only a test.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad18.webp)

Go to **Runbook Manager** and click on **Publish**. Our Runbook is now ready for use.

## Setup process in ServiceNow

We are now able to create a new user from a **Runbook**. Next we will look into how we can create a **Catalog Item** that we can use to trigger a **Flow** that will start a **Job** from the **Runbook**.

### Create Catalog Item

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad19.webp)

Use the application navigator and go to **Maintain Items** and click the **New** button.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad20.webp)

1. Enter a name for the new catalog item
2. Enter a short description.
3. Right click on the grey bar at the top and select **Save**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad21.webp)

Select **Variables** in the related lists section and click on **New**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad22.webp)

1. Set type to **Singe Line Text**.
2. Select **Mandatory**.
3. Set the **Question** to **Username**.

Then click on **Submit** and repeat this step for **Display name**.

### Create Flow

Before we can complete the **Catalog Item** we will create a **Flow** to complete the request.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad23.webp)

Use the Application Navigator to go to **Flow Designer**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad24.webp)

In **Flow Desinger** create a new **Flow**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad25.webp)

Give the **Flow** a name and set the **Run As** to **System User**. 

Click **Submit**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad26.webp)

Click on **Add a trigger**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad27.webp)

Select **Service Catalog**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad28.webp)

Click on **Done**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad29.webp)

Click on **Add an Action, Flow Logic, or Subflow**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad30.webp)

1. Click on **Action**.
2. Click on **ServiceNow Core**.
3. Click on **Get Catalog Variable**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad31.webp)

1. Drag the **Requested Item Record** to the **Submitted Request** field.
2. Select the catalog item that we created in the previous section.
3. Move both **username** and **display_name** to the **Selected** column
4. Click **Done**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad32.webp)

1. Click on **Add an Action, Flow Logic, or Subflow** and select **Action**.
2. Click on **Automation App**.
3. Click on **Start job**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad33.webp)

1. Select the **Runbook** that we create in the previous section.
2. Set **Input format** to **Pipe separated list**.
3. Drag **username** to the **Input** field.
4. Type in a password for the new user to be created with surrounded by a pipe at each end.
5. Drag **display_name** to the **Input** field.
6. Drag **Requested Item Record** to the **Parent** field.
7. Click on **Done**.

> Notice: This way all new users will be created with the same password. For production use you may want to use a script to generate a random password either in the Flow or in the Runbook. We only do it this way in this tutorial to keep things as simple as possible.

We will now add another action to obtain the **Object Id** of the new user created. This is to showcase how values can be extracted from the **Job** which has now completed.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad34.webp)

1. Click on **Add an Action, Flow Logic, or Subflow** and select **Action**.
2. Click on **Automation App**.
3. Click on **Get output variable**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad35.webp)

1. Drag the **job** to the **Job** field.
2. Type in *ObjectId* in the **Variable** field.
3. Click on **Done**.

Now we will update the **Requeted Item** with a work note containing the variable that we have just extracted as well as set it to **Closed Complete**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad36.webp)

1. Click on **Add an Action, Flow Logic, or Subflow** and select **Action**.
2. Click on **ServiceNow Core**.
3. Click on **Update Record**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad37.webp)

Drag the **Requested Item Record** to the **Record** field and click on **Add field value**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad38.webp)

1. Select **Work notes** in the dropdown.
2. Enter the text *User created with boject id:* in the value field.
3. Drag the **Value** to the field after the text.
4. Click on **Add field value**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad39.webp)

1. Select **State** in the dropdown.
2. Select **Closed Complete**.
3. Click on **Done**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad40.webp)

Click on **Save** in the upper right corner and then on **Activate**.

### Complete Catalog Item

Navigate back to the catalog item that we create previously.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad41.webp)

1. Click on **Process Enginge** tab.
2. Select the **Flow** that we just created in the **Flow** field.
3. Right-click on the grey bar at the top and select **Save**.
4. Click on **Try It** to test that everything works.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad42.webp)

Fill in the **Username** and **Display Name** field and click on **Order Now**.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad43.webp)

Click on the description to open the record.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_aad44.webp)

After a few seconds you should see that the state changes to **Closed Complete** and that a **Work note** is added to the **Requested Item**.











