---
layout: single
title: Create an Azure Active Directory User
permalink: /rights-management-app/create-azureaduser/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

In this guide we will show how to user some actions in flow designer to create an Azure Active Directory User.

## Create a Service Catalog Request
In this step we will create a catalog item that we can use to trigger the flow that will create the Azure AD User.

### Create Catalog Item 
From the Application navigator navigate to Maintain items and click New button.
![Maintain Item](/assets/images/maintainitem.webp)

Input a name for the Catalog Item like the one show below "Create Azure AD User", select a Catalog and a Category for the Item to appear in and add a Short description and a Description for the Catalog Item. Then right click on the grey bar at the top of the form and select save.

![Catalog item](/assets/images/catalogitemcreateaaduser.webp)

Scroll down on the form, select the Variables related list and click on New. 

![Variables](/assets/images/variablesrl.webp)

Set the type of the variable to be "Single Line Text" and set it to be mandatory. Set the question to be "First name".

![First name](/assets/images/username.webp)
 
 Then click on Submit and repeat this step for the variable "Last name".

 ![Last name](/assets/images/lastname.webp)

You can also create a variable of type "Date" to define the start date of the new Azure AD User.

![Last name](/assets/images/startdate.webp)

### Create Flow

Now use the application navigator to go to Flow Designer and create a new Flow. Give a name to the application select the Rights Management App as application and Run as System User and then click on Submit.

![Flow](/assets/images/flow.webp)

In the flow page click on Add a Trigger and select Service Catalog.

![Trigger](/assets/images/triggerservicecat.webp)

Then select the action : Get Catalog Variables.

![Get catalog variables](/assets/images/getcatalogvar.webp)

Drag and drop the Trigger>Request Item Record in the Submitted Request field, select the Create Azure AD User in Template Catalog Items and Variable Sets and select all the available catalog variables from the Catalog Variables list and drop them in the selected list. Then click on Done.

![Get catalog variables](/assets/images/variablescatalog.webp)
