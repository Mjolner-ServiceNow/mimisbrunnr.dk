---
layout: single
title: Group Monitoring
permalink: /rights-management-app/group-monitoring/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

With Rights Management App you are able to access all groups in your Active Directory and track the group memberships from ServiceNow. This is a powerfull tool as it gives you the advantage of reviewing and controlling access permissions to ensure that users have the exact permissions they need to do their job and that no access is given that the user should not have. 

You decide to what extend you want Rights Management App to take control over your groups in Active Directory. This is configured per group, so you can fine grain the level of control that you want to apply to different groups of your Active Directory. 

## Group sync policies

There are four different group sync policies available in Rights Management App. Depending on which you se in each group there will be different functionality applied to the group. 

![Sync policy](/assets/images/x_autps_active_dir_groupsync.webp)

### Ignoring

The default behavior is **"Ignoring"**. At this stage Rights Management App will show you that the group exists, but it will not list the group  members.

### Observing 

One level up is **"Observing"**. If you set the sync policy to observing Rights Management App will monitor the memberships of the groups, so that you know who is a member of the given group from ServiceNow.

### Permissive

One more level up is **"Permissive"**. With this sync policy you can manage access to a group from ServiceNow. An incident will be created if any membership is discovered that ServiceNow did not expect to see. This is an indication that somebody gave someone access without following the process in ServiceNow.

### Enforcing

The highest level is **"Enforcing"**. At this level you can manage access to a group and ServiceNow will not report on deviations, it will correct them, meaning that memberships you have defined that they shoul be there in ServiceNow will be enforced and any deviation will be overwritten.