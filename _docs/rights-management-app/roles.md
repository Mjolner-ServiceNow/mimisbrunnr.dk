---
layout: single
title: Roles
permalink: /rights-management-app/roles/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

Rights Management App adds a new abstraction lasyer called roles. Roles can be used to control access of users and groups and to apply time limited memberships. In this guide we will learn how to create a role in RIghts Management App.

## Create a Role

To create a Role in Rights Management App you have to go to the Roles list and click the **New** button on the upper left corner of the screen.

![Roles](/assets/images/x_autps_active_dir_roles.webp)

Give a meaningful name to the Role and select the domain in which the role will be submitted. You can choose a valid to date from which date the role will be available and a valid to date which will be the date the role will be no longer functional. Then click on Submit.

![Role creation](/assets/images/x_autps_active_dir_rolesubmit.webp)

## Assign role to group

Once the role is submitted you can navigate back to the form. There are two related lists created from where you can add users and groups. Navigate to the groups related list and click on **Edit...** to give to an existing group the Role you created.

![Group edit](/assets/images/x_autps_active_dir_groupedit.webp)

Select the group from the list on the left of the screen and click on **Save**.

![Group select](/assets/images/x_autps_active_dir_groupselection.webp)

## Assign role to user

Navigate to the role you created and click on the **Edit...** button on the Users related list. From there you can give to an existing user the role. 

![User edit](/assets/images/x_autps_active_dir_useredit.webp)

Select the user from the list on the left of the screen and click on **Save**.

![User select](/assets/images/x_autps_active_dir_userrole.webp)

Now the user has the Role IT Support and the user became member of the Group IT Group as they have the same role. You can confirm this by navigating to the users form and checking if the group is in the Active DIrectory Group Member related list and the Group Membership Changes related list should have a record "Added to group".

![Group member](/assets/images/x_autps_active_dir_usermem.webp)