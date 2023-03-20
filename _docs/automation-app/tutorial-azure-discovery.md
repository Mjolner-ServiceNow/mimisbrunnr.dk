---
layout: single
title: Microsoft Azure Discovery & CMDB Enrichment
permalink: /automation-app/azure-discovery/
last_modified_at: 2023-03-20T11:36:18-04:00
sidebar:
  nav: "aa"
toc: true
read_time: true
---

The purpose of this tutorial is to guide you through the process of connecting to Azure and enrich your ServiceNow CMDB with any type of resources from Azure.

We will be setting up a flow in ServiceNow that will run daily and import the resources to an import set. From there we will look at how we can map the data to get it into our CMDB.

The purpose of this tutorial is not to provide a complete discovery with mapping of all data, but we will show you how to get all data and give you the basics so that you can map the data to fit the needs of your organization.

## Setup Azure connection

We will be using an App Registration (Service Principal) in combination with a secret to authenticate with Azure. 

You can also use a certificate instead of a secret for added security. For an example of how to setup a certificate based connection see this [tutorial](/automation-app/azure-ad-integration/).

You must have suffient administrative access to Azure Active Directory (Eg. Global Administrator) to complete this part.

### Create App Registration

Go to [portal.azure.com](https://portal.azure.com) and login with your azure credentials.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery0.webp)

In the Azure Portal go to **Azure Active Directory** from the search bar at the top. 

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery1.webp)

Click on **App registrations** in the menu to the left. This will give you a list of registered Apps. Click on **+ New registration** at the top left of the list.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery2.webp)

Give the application a meaningfull name and click on **Register**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery3.webp)

Copy the **Application (client) ID** and the **Directory (tenant) ID**. We will need this later.

Click on **Certificates & Secrets**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery4.webp)

Click on **Client secrets** and select **New client secret**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery5.webp)

Add a description for the client secret. This could be the name of your ServiceNow instance, so that you know where this secret is in use. Set an expiration time and click on **Add**.

> Tip: The maximum expiration when using the UI is 2 years. If you want an expiration date, that is longer that this you would need to use PowerShell or Azure CLI to create the secret.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery6.webp)

Click the **copy** icon and save the secret for later. It is important that you do this immediately, as you will not have an option to retrieve it again later.

### Setup permissions for the application

Next we will give the Application access to read all resources in Azure. We will do this by giving the application read access on the subscription level. You can also do this on a lower level, eg. on selected Resource Groups, if you wish to limit what resources will be discovered.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery7.webp)

Search for **Subscriptions** in the search bar at the top and click on **Subscriptions**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery8.webp)

This will give you a list of all available subscriptions. Select the subscription you wish to give access to.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery9.webp)

Click on **Access control (IAM)** in the menu to the left. The click on **Add** and select **Add role assignment**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery11.webp)

Select the **Reader** role and click **Next**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery12.webp)

1. Select **User, group, or service principal**.
2. Click on **Select members**.
3. Search for the application that we created previously. Once it appears, click on it to select it.
4. Once selected the application will move to the **Selected members** section.
5. Click on **Select** to confirm your selection.
6. Click on **Next**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery13.webp)

Verify that everything looks okay, and then click **Review + assign**.

## Create integration in ServiceNow

We now have everything in place in Azure, so let us continue in ServiceNow.

### Create a new App Scope

To make sure that we do not change any standard ServiceNow functionality we always recommend to create your integrations in seperate application scopes.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery14.webp)

In ServiceNow go to **Studio** in the application navigator.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery15.webp)

Click on **Create Application**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery16.webp)

1. Give the Application a name. We will call it **Azure Discovery**.
2. Select **Scoped** under **Advanced settings**.
3. Click on **Create**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery17.webp)

We will not be using the guide, so just click **Continue in Studio (Advanced)**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery18.webp)

Find the newly created applicaiton in the application list and click on it to open it.

### Create an import set table

Now that we have an application, we will create a table in which we will save the data that we are about to receive from Azure.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery19.webp)

Click **Create Application File**, select **Table** and click **Create**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery20.webp)

1. We will call the table **Azure Resource**.
2. Set the table to extend **Import Set Row**.
3. Unselect **Create mobile module**.
4. Set the name of the new menu to **Azure Discovery**.
5. Select the tab **Controls**.
6. Make sure **Create access controls** is selected.
7. We will call the role **azure_import_user**. Notice that you should only change the part, that is after the scope name of your application (eg. x_autops_azure_disc). Make sure to note down this name, as we will need it later.
8. Click on **Submit**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery21.webp)

1. Note down the name of the table, we will be needing this soon.
2. Click on the **Columns** tab.
3. Scroll down.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery22.webp)

Double click the **Insert a new row** and create the following fields:

```
location [string, 64]
name [string, 64]
namespace [string, 64]
properties [string, 4096]
resource_group [string, 128]
resource_id [string, 256]
resource_type [string, 128]
subscription_id [string, 36]
subscription_name [string, 128]
sku [string, 512]
tags [string, 1024]
instance_view [string, 4096]
```

Once the fields are added click **Update**.

### Setup an integration user

Next we will create a user, that has write access to this table

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery23.webp)

Go to **System Security** -> **Users and Groups** -> **Users** and click the **New** button.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery24.webp)

1. Give the new user a **User ID** and note it down as we will be using this later.
2. Insert a **First name**, eg. *Azure*.
3. Insert a **Last name**, eg. *Discovery*.
4. Right-click the grey bare at the top, and select **Save**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery25.webp)

1. Click on **Set Password**.
2. Click on **Generate**.
3. Click the **copy** icon and save the password in your notes.
4. Click on **Save Password**.
5. Close the dialouge.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery26.webp)

1. Unselect **Password needs reset**.
2. Select **Web service access only**.
3. Right-click the grey bare at the top, and select **Save**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery27.webp)

Scroll down under related records and select the **Roles** tab and click **Edit...**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery28.webp)

Find the role, that we created earlier by searching for it. Then select it in the left column and click the arrow button to move it to the right column. Then click **Save**.

### Create Runbook in Automation App

Next we will setup the runbook in ServiceNow using Automation App.

![Open Runbook Manager](/assets/images/x_autps_azure_auto_menu_runbooks.webp)

In ServiceNow open Runbook Manager in the Application Navigator

![Select Runbooks in Runbook Manager](/assets/images/x_autps_azure_auto_create_runbook0.webp)

Select the **Automation Account** you wish to use and click on **Create New** under **Runbooks**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery29.webp)

1. Give the Runbook a name, eg. *Import-Azure-Resources*.
2. Select **PowerShell 5.1** as **Type**.
3. Give the Runbook a meaningfull description.
4. Click on **Create**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery30.webp)

Click on the newly created Runbook to open it. If you have many Runbooks you can use the filter function to the right to search for it.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery31.webp)

Click on **Edit** to put the Runbook into edit mode.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery32.webp)

1. Click on **Templates**.
2. Expand **Microsoft Azure Resource** and click on **Import Resources**.
3. Right-click on the canvas and choose paste. This will insert the template which was copied to your clipboard in step 2.
4. Click on **Save**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery33.webp)

1. In the coding canvas scroll to the top.
2. Click on **Variables**.
3. Click on **Create new**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery34.webp)

The first variable we will create is for our Directory ID. We call this one *AzureDirectoryID*. If you have multiple directories, you may want to use a more unique name. Set **Is encrypted** to false and paste in the Directory ID that we copied earlier. Click on **Create**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery35.webp)

Insert the name of the Directory ID variable, eg. *AzureDirectoryID*, into the Runbook as shown above.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery36.webp)

Next we will create a variable for the Client ID following the same procedure. Notice that you may want to choose a more unique naming, that what I did here.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery37.webp)

Insert the name of the variable into the Runbook as shown above.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery38.webp)

Next we will do the same for the Client Secret. This time though you will want to set **Is Encrypted** to true.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery39.webp)

Insert the name of the variable containing the secret into the Runbook as shown above.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery40.webp)

We now have what we need to get the data from Azure. Next we will create credentails, so that we can store this data in ServiceNow.

1. Click on **Credentials**.
2. Click on **Create new**.
3. Give the credentials a name, eg. *AzureDiscoveryServiceNowUser*.
4. Give a description of what the credentials are used for.
5. Enter the **username** that we created in ServiceNow earlier.
6. Insert the **password** that we created in ServiceNow earlier.
7. Click **Create**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery41.webp)

1. Insert the name of the credentials in the Runbook as shown above.
2. Insert the name of table that we create earlier in the Runbook as shown above.
3. Click on **Save**.
4. Click on **Test**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery42.webp)

1. Enter the name of your ServiceNow instance
2. Click on **Create**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery43.webp)

A new test job will now be created. Depending on the number of resources that the app registration has access to this job may take some time to complete. Now is a good time to go for a cup of coffee or a walk with a friend.

Once the State changes to **Complete** the test job has completed. Make sure there are no errors (red text) in the **Output** then close the test tab.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery44.webp)

Click on **Publish**. The Runbook is now ready for use.

Let us head back to the ServiceNow backend and see the data that was imported to make sure, that we discovered the resources, that we expected.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery45.webp)

If you have not refreshed the backend since you created the app scope in Studio you may want to reload the page in your browser.

Search for **Azure Discovery** in the filter navigater an click on **Azure Resources**.

A list of all the resources that we have imported from Azure should now be visible. 

## Create a flow for daily import

Next we will setup a scheduled flow to ensure that we continuesly will get our Azure resources imported into ServiceNow.

![Open Flow Designer](/assets/images/x_autps_azure_auto_open_flow_designer.webp)

Open flow designer by selecting **Process Automation** -> **Flow Designer**

![Create a new Flow](/assets/images/x_autps_azure_auto_create_flow0.webp)

Click the **New** button and select **Flow**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery46.webp)

1. Give the flow a name.
2. Add a description to help the future you to easy understand what the flow does.
3. Make sure the application is the application that we create previously.
4. Set **Run As** to **System User**.
5. Click on **Submit**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery47.webp)

Click on **Add a trigger**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery48.webp)

Click on **Daily**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery49.webp)

Set a time that suits your environment. Choosing a time where there are not so many users on your system is likely ideal. 

Click on **Done**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery50.webp)

Click on **Add an Action, Flow Logic, or Subflow**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery51.webp)

Click on **Action**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery52.webp)

Click on **Automation App** and then **Start job**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery53.webp)

1. Select the Runbook that we created previously.
2. Set **Input format** to **Auto**.
3. Click the **code** icon next to the **Input** field.
4. Replace the text in the **Input** field with *return gs.getProperty('instance_name');*
5. For reporting on your automation savings insert the appropriate time savings.
6. Click on **Done**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery54.webp)

Click on **Save** and then on **Activate**. Confirm that you wish to activate the flow.

Now a daily import of resources from Azure will take place.

## Setup data mapping to your CMDB

We are now getting data imported into the import set table daily, but here it does not add much value. Thus we will now start mapping the data to the different tables of your CMDB.

### Map using transform maps

A well known way of mapping the data from the import set into your CMDB is by using transform maps, which is what we will use in this tutorial.

We will only be mapping Virtual Machines, but the process is the same for any other configuration item.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery14.webp)

If you closed the Studio tab, reopen **Studio** using the application navigator and click on the application name to open it.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery55.webp)

Click on **Create Application File**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery56.webp)

Search for *transform* and click on **Table Transform Map**. Click **Create**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery57.webp)

1. Enter *Virtual Machine* in the **Name** field.
2. Set **Source table** to *Azure Resource*.
3. Set **Target table** to *Virtual Machine Instance*.
4. Click on **Submit**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery58.webp)

Next we will add a few (10 actually) field mappings.

Scroll down to the related records list and select **Field Maps** and then click on **New**. 

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery59.webp)

1. In **Source field** select *Name*.
2. In **Target field** select *Name*.
3. Click **Submit**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery60.webp)

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery61.webp)

1. In **Source field** select *Resource ID*.
2. In **Target field** select *Correlation ID*.
3. Set **Coalesce** to true. This will ensure that already created CIs will be updated rather than created again.
4. Click **Submit**.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery63.webp)

1. In **Source field** select *Created*.
2. In **Target field** select *Most recent discovery*.
3. Click **Submit**.

We do this so that you can track when a CI was last discovered. You may want to setup an action to handle if a CI is not discovered for a period of time.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery64.webp)

1. Set **Use source script** to true.
2. In **Target field** select *State*.
3. Insert the below script in the **Source script** field.
4. Click **Submit**.

```javascript
answer = (function transformEntry(source) {
  var instanceView = JSON.parse(source.instance_view);
  var statusRes = "error";
  instanceView.statuses.forEach(function(status){
     if(status.code == "PowerState/running")
        statusRes = "on";
     if(status.code == "PowerState/deallocated" || status.code == "PowerState/stopped" )
        statusRes = "off";
     if(status.code == "PowerState/deallocating" || status.code == "PowerState/stopping" )
        statusRes = "stopping";
     if(status.code == "PowerState/deleted"  || status.code == "ProvisioningState/deleted")
        statusRes = "terminated";
  });
  return statusRes;
})(source);
```

The script parses the instance field of and then runs through the status of the virtual machine. If it finds a code that it understands it sets the result to a status code that matches the states in ServiceNow.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery65.webp)

1. Set **Use source script** to true.
2. In **Target field** select *Disks*.
3. Insert the below script in the **Source script** field.
4. Click **Submit**.

```javascript
answer = (function transformEntry(source) {
  var properties = JSON.parse(source.properties);
  return properties.storageProfile.dataDisks.length;
})(source);
```

In this case we are parsing the properties field and return the length (number of items) in the array containing the data disks. If you want to include the OS disk you may want to add *+ 1* in the return statement.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery66.webp)

1. Set **Use source script** to true.
2. In **Target field** select *Memory (MB)*.
3. Insert the below script in the **Source script** field.
4. Click **Submit**.

```javascript
answer = (function transformEntry(source) {
  var properties = JSON.parse(source.properties);
  return properties.hardwareProfile.memoryInMB;
})(source);
```

In this case we are parsing the properties field and return the value of the property memoryInMB.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery67.webp)

1. Set **Use source script** to true.
2. In **Target field** select *VM Instance ID*.
3. Insert the below script in the **Source script** field.
4. Click **Submit**.

```javascript
answer = (function transformEntry(source) {
  var properties = JSON.parse(source.properties);
  return properties.vmId;
})(source);
```

In this case we are parsing the properties field and return the value of the property vmId.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery68.webp)

1. Set **Use source script** to true.
2. In **Target field** select *Network adapters*.
3. Insert the below script in the **Source script** field.
4. Click **Submit**.

```javascript
answer = (function transformEntry(source) {
  var properties = JSON.parse(source.properties);
  return properties.networkProfile.networkInterfaces.length;
})(source);
```

In this case we are parsing the properties field and return the length (number of items) in the array containing the network interfaces.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery69.webp)

1. Set **Use source script** to true.
2. In **Target field** select *CPUs*.
3. Insert the below script in the **Source script** field.
4. Click **Submit**.

```javascript
answer = (function transformEntry(source) {
  var properties = JSON.parse(source.properties);
  return properties.hardwareProfile.numberOfCores;
})(source);
```

In this case we are parsing the properties field and return the value of the property numberOfCores from the hardwareProfile object. It is debatable if this is the correct way to measure the number of CPUs, but this is up to you.

Close the newly created record and click on **New** again to add another field mapping.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery70.webp)

1. Set **Use source script** to true.
2. In **Target field** select *Discovery source*.
3. Insert the below script in the **Source script** field.
4. Click **Submit**.

```javascript
answer = "Azure Discovery";
```

In this case we are simply return a string *Azure Discovery*. We do this to make it visible where the data in the CMDB is coming from.

Close the newly created record.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery71.webp)

Right-click on grey bar at the top of the list of **Field maps** and select **Refresh List**.

![Register an application](/assets/images/x_autps_azure_auto_azure-discovery72.webp)

You should now see a list of the 10 field maps that we just created.

To verify that everything works as expected you can go back to Flow Designer and select **Test** on the flow that we created there. You should then see that virtual machines should appear in the **Virtual Machine Instance** table.

If the virtual machines instance table is not visible in your application navigater type *cmdb_ci_vm_instance.list* in the filter at the top left and hit enter. This should bring you to the table.

### Apply Identification and Reconciliation

The discovery that we have configered above will add all data to a single import set table from where you will create a transform map for each diferent CI class, that you wish to map.

If you wish to use ServiceNow's Indentification and Reconciliation engine (IRE) you have to use scripting, as the default way documented [here](https://docs.servicenow.com/bundle/utah-servicenow-platform/page/product/configuration-management/concept/identification-import-sets.html) is restricted to only one transform map per import set.

How to use the IdentificationEngine script include is documented on the ServiceNow developer portal [here](https://developer.servicenow.com/dev.do#!/reference/api/utah/server/sn_cmdb-namespace/IdentificationEngineScopedAPI#IESS-createorUpdateCI_S_S).

Using this approach will require some scripting, but it is a great way to ensure that you always get the correct relationships mapped and that you are not creating duplicates in your CMDB. This is especially relevant, if you wish to use other data sources to populate information about your Azure resources.