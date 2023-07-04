---
layout: single
title: Create an Azure Active Directory Guest User
permalink: /rights-management-app/create-aadguestuser/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

In this guide we will show how to create a guest user in your Azure AD with Flow Designer.

## Create a Service Catalog Item

Navigate to Maintain Items list in ServiceNow and click on the New button.

![Maintain Item](/assets/images/maintaintemstwo.webp)

Enter a name for the catalog item like "Create guest user", select a catalog and a category for the item to appear to, give a short description and right-click on the grey bar at the top and select Save. 

![CI create guest user](/assets/images/cicreateguestuser.webp)

Then scroll down and find the variables related list. Click on New, select type Single Line Text and set it to mandatory. In the Question tab set the Question to be "Email of the guest user" and the name to be "email" and click on Submit.

![Variable email](/assets/images/variableemail.webp)

Next add one more variable of type Single Line Text, set it to mandatory and in the Question section set the Question to be "Username" and the name to be "username". Click on Submit.

![Variable username](/assets/images/variableusername.webp)

Add a new variable of type Single Line Text and set the Question to be "Redirect URL" and the name of it to be "redirect_url". Click on Submit.

![Variable redirect url](/assets/images/variableredirecturl.webp)

Then create a variable of type Reference and set it to Mandatory. In the question tab set the Question to be "Domain" and the name to be "domain". On the Type Specifications tab select the reference to be Domain[x_autps_active-dir_domain] and click on Submit.

![Variable domain](/assets/images/variabledomain.webp)

Lastly create a variable of type Single Line Text and set the Question to be "Message" and the name to be "message". Click on Submit.

![Variable message](/assets/images/variablemessage.webp)

## Create Flow

Before we can complete the Catalog Item we will create a Flow to complete the request. Navigate to Flow Designer anf create a new Flow. Give a meaningful name to the Flow and select Run ad System User.

![Flow guest user](/assets/images/flowguestuser.webp)

Select as a trigger the Service Catalog and click on Done.

![Trigger Service Catalog](/assets/images/servicecatalogtrigger.webp)

Then add the action from ServiceNow Core > Get Catalog Variables.

![Get catalog variables](/assets/images/getcatvar.webp)

In the submitted request drag and drop the Requested Item Record from the data pill, in the template Catalog Items and Variable sets select the Create guest user Catalog Item we created before and from the available catalog variables select them all and move them to the selected list. Click on Done.

![Get variables from create guest user](/assets/images/variablesguest.webp)

Next select the action from Rights Management App > Create Azure AD guest user. 

![create guest user](/assets/images/createguestuser.webp)

Drag and drop the catalog variables from the data pill to the corresponding fields in the create Azure AD guest user action. Then click on Submit.

![create guest user action](/assets/images/createguaction.webp)

Then select the ServiceNow Core action Update Record.

![Update record](/assets/images/updaterecordgu.webp)

In the record field drap and drop the Requested Item Record, add a work note if you want and set the state to Closed Complete. Then click on Done.

![Update record](/assets/images/updaterecordguaction.webp)

Click on Save and Activate in the top right corner and your flow is ready.

Navigate back to your catalog item and in the Process Engine tab set the flow you created and right click on the grey bar at the top of the form and click on Save.

![Create guest user CI](/assets/images/readyguestuserci.webp)

Now click on try it at the top of the form. Input the variables that are asked and click on order now.

![Create guest user test](/assets/images/testguestuser.webp)

Here we can see the result:

![Servicenow result guest user](/assets/images/resulttestuser.webp)

![Azure result guest user](/assets/images/resultguaad.webp)