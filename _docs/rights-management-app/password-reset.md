---
layout: single
title: ServiceNow Flow Designer
permalink: /rights-management-app/password-reset/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

In this guide we will show you how to create a flow for a password reset request.

## Create a Service Catalog Request

We assume that you know how to create a service catalog request so we will present this step quick. Navigate to Maintain Items into your ServiceNow instance and create a New one. Give a meaningfull name to your Catalog Item and in the Catalogs field choose the Service Catalog. Choose a Category for the Catalog Item to appear to. Give a Short description and a Description and right click on the grey bar and select Save. On the Variables related list create a new variable of type Reference that references the Rights Management App user table. The user variable will represent the user for whom you request the password reset. The request should look like the following in your Service Catalog.

![Request](/assets/images/x_autps_active_dir_passreset.webp)


## Create the Flow

Open the Flow Designer by navigating to Process Automation -> Flow Designer. Click on New -> Flow on the right side of the screen. Give a name to the flow like "Reset Password" and click on Submit. 

![Flow creation](/assets/images/x_autps_active_dir_flowcreate.webp)

Select Service Catalog as trigger.

![Flow trigger](/assets/images/x_autps_active_dir_flowtrigger.webp)

 As a first action we need to pass the user record for whom we requested the new password. We choose Get Catalog Variables as action, the Submitted Request should be the Request Item Record from the Service Catalog Trigger and the Template Catalog Items and Variable Sets should be the maintain item you created in step 1. From the available catalog variables select the user variable and pass to the selected list. 
 
 ![Step1](/assets/images/x_autps_active_dir_flowaction1.webp)

 As a second action select the Generate Password from Rights Management App and select a Domain. 
 
 ![Step2](/assets/images/x_autps_active_dir_flowaction2.webp)

The third and last action should be the Set Password action from Rights Management App. As User select the user variable from the Get Catalog Variables action and as Password select the password created from the Generate Password action. You can select if the user must change the password after the password reset and if the account of the user should be unlocked by checking the relevant boxes.

![Step3](/assets/images/x_autps_active_dir_flowaction3.webp)


