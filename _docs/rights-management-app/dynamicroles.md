---
layout: single
title: Dynamic Roles
permalink: /rights-management-app/dynamicroles/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

Dynamic Roles can make membership assignment an easy task. You just need to add some conditions in the dynamic role and was those conditions match an identity then the identity will automatically be assigned tot he role and the memberships will be created. In this tutorial you will learn how to create dynamic roles and how those dynamic roles work.

### Create a Dynamic Role

Navigate to Rights Management App > Dynamic Roles and click on New. Give a name to the dynamic role and define some conditions that must be met. For the purpose of this tutorial we wil add the following condition: 

Right-click on the top grey bar and select Save.

If an Identity match those conditions then the accounts of the Identity will become members of the groups that assigned to the Dynamic Role.

### Assign Groups to the Dynamic Role 

You can add Active Directory Groups, Azure Active Directory Groups and ServiceNow Groups to the Dynamic Role. To do that you need to scroll down to the related lists of the Dynamic Role and select one of the group categories and click on New. In this tutorial we select the Azure Active Directory Groups. 

Select a group and an Entitlement type and click on Submit. If you navigate now to the group you just added to the Dynamic role the user that match the conditions of the Dynamic role should have become member of the group. 
