---
layout: single
title: Positions
permalink: /rights-management-app/positions/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

### Create Position
Positions can be created to collect groups that are related to specific positions in your organization. For example think of a manager and a trainee position in a company. The people working as managers need to have wider access to recourses than trainees and some of the resources managers nedd to have access to are common, and the same stands for the trainees too. By using the Position feature in Rights Management App you can collect all the groups that are related to specific positions and assign memebrships much easier and quicker. Navigate to Rights Management App > Positions and click on New.

![Positions list](/assets/images/position.webp)

Give a Name to the position, you can also write a Description and select a Valid from and a Valid to date. If a Valid from date is selected then the Identities that will added to this Position will get the memberships to the groups added to this Position when the valid from date comes. On the same way those memberships will be deleted when the Valid to date comes. Then right-click on the upper grey bar and select Save.

![Position creation](/assets/images/createposition.webp)

Scroll down to the related lists. From there you can add Active Directory Groups, Azure Active Directory Groups and ServiceNow groups to the Position. Select one of the three and click on New (in the example shown here we seleted the ServiceNow Groups. The proccess is the same for the other two group types). 

![Add groups to the position](/assets/images/snposition.webp)

Select the group you want to add to the position and the Entitlement Type of the user accounts that will become members of the group and click on Submit. User accounts that are linked to the Identities which belong to this position and have the same entitlement type as the Position-Group link will become members of the group.

![Add a ServiceNow group to the Position](/assets/images/positionlink.webp)

### Assign the Position to an Identity
Navigate to Rights Management App > Identities and select an Identity you want to add to the Position and open the record. In the Position field click on the search/magnifier button and select the Position you created. Then click on Update. On this way the Position is assigned to the Identity and the accounts linked to the Identity became members of the groups that are linked to the position.

![Add an identity to the Position](/assets/images/identityposition.webp)

![Accounts added to groups](/assets/images/identityaddedtogroupspos.webp)