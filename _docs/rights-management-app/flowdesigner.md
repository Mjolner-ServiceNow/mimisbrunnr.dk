---
layout: single
title: ServiceNow Flow Designer
permalink: /rights-management-app/flow-designer/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

Using Rights Management App in ServiceNow Flow Designer is a powerfull way to integrate your flow or create new ones with the already defined actions in Rights Management App.

## Available actions 

There are several actions availiable in Rights Management App that you can use to automate processes for groups, users, memberships and roles. Here is a list of all the actions added with Rigths Management App.

### Group
#### Active Directory
- Create group: Give a name, select a domain, a group category, a group scope,, a sync policy and add a description.
- Empty group: Select the AD Group you want to empty.
- Remove group: Select the AD group you want to delete.
#### Azure Active Directory
- Create Azure AD group: Input a name, a mail nick name, select if you want security enabled and mail enabled, choose a sync policy, add a description and select the domain in which the group should be created in.
- Empty Azure AD Group: Select the Azure AD group you want to empty
- Remove Azure AD Group: Select the Azure AD Group you want to delete


### User
#### Active Directory
- Create user: Give a first name, a last name, a username and select a domain and a user path.
- Disable user: Select the AD user you wantto disable.
- Enable user: Select the AD User you want to enable.
- Unlock user: Select the AD User whose account you want to unlock.
- Set password: Sect the AD User and the password you want to set to the user, select if you want the user to change the password and if the account should be unlocked.
#### Azure Active Directory
- Create Azure AD guest user : Set the email of the user you want to create as guest, the url the user will be redirected to, give a username for the user, add a message to the email it will be sent and select the domain in which the user will be added as guest.
- Create Azure AD User: Give first name, last name, username, select the domain in which the user will be created, input a country, location, job title, department, select a manager, give a phone number, set a hire date and select an accoutn type. 
- Disable Azure AD User: Select the user you want to disable. 
- Enable Azure AD User: Select the Azure AD User you want to enable.
- Set Exchange Online Folder Permission: Input a mailbox, a role, a type, select an azure AD User and a domain.
- Set mobile authentication method: input a mobile number, select a domain and the user id of the Azure AD user you want to create the mobile authentication.
- Unlock Azure AD User: Slect the Azure AD User you wish to unlock.
- Set Azure AD user password: Select the azure AD User, the password, if the user must change the password and the account should be unlocked.
#### Can be used in both 
- Generate password: Select the domain for which the password should be genarated.
- Generate username: input the name (it can be the first name and the last name of the user), select the domain, enter a prefix if you want one and select the length of the username that should be genarated.

### Membership
#### Active Directory
- Add group member: Select the AD Group and the AD User you want to add the membership for. You can also select an expiration date for the membership.
- Empty group: Select an AD group you want to empty.
- Remove group member: Select the AD Group and the AD User you want to remove from that group.
#### Azure Active Directory
- Add Azure AD group member :You select the Azure AD Group and the Azure AD User you want to add to the group. You can also set an expiration date, which will be the date that the membership will be removed.
- Remove Azure AD group member : Slect the Azure AD Group and the Azure AD User you want to remove from the group.

### Role
#### Active Directory
- Assign role to group: Select an AD Group and the role you want to give to the group.
- Assign role to ADuser: Select an AD User and the role you want to give to the user.
- Remove role from user: Select the AD User and the Role you want to remove from the user.

#### Azure Active Directory
- Assign role to AD group: Select the Azure AD Group and the role you want to give to the group.
- Assign role to AzureAD user:Select the Azure AD User and the role you want to give to the user. You can also set a valid from and a valid to date.
- Remove role form Azure AD user: Select the Azure AD User and the Role you want to remove from the user.

