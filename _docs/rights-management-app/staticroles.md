---
layout: single
title: Static Roles
permalink: /rights-management-app/staticroles/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

### Create a Static Role
 Navigate to Rights Management App > Static Roles and click on New.

![Static role list](/assets/images/staticrolelist.webp)

 Give a name to the Static Role. Here exept of setting a valid from and to date you can also add an approval proccess. You can choose between owner,group or manager approval. An approval flow is already created for you. The only thing you need to do is to decide who should be the one to approve the role assignment. In this example we will choose the owner approval.

![Create static role](/assets/images/createstaticrole.webp) 

 Once you have finished with creating the Static Role rigth-click on the top grey bar and select Save.

### Assign the Static Role to Groups
Second step of the process is to assign groups to the Static Role. Scroll down to the related list and select between the Active Directory Groups, Azure Active Directory Groups and ServiceNow Groups. 

![Related lists](/assets/images/assigngroupstostaticrole.webp)

(In this example we will add Azure Active Directory Group. The process is the same for each group category you select). Select a Group from the list and an Entitlement type and click on Submit.

![Add group to static role](/assets/images/addgrouptostaticrole.webp)

Then in the Identities related list click on New. 

![Identities related lists](/assets/images/addidentitytostaticrole.webp)

In the Identity field select the Identity you want to assign to the Static Role to and you can also select a valid to and a valid from date. Then click on Submit. 

![Identity static role assignment](/assets/images/addidentitytostaticroletwo.webp)

Group memberships are not yet assign and the reason is that the owner did not yet approve the Role assignment. To approve the assignement the owner has to navigate to the Static Role- Identity Link and change the state of the request to Approved. Once this happens the accounts of the Identity become automatically members of the groups in the Static Role.

![Approve static role assignment](/assets/images/approveassignment.webp)