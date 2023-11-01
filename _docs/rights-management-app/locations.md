---
layout: single
title: Locations
permalink: /rights-management-app/locations/
last_modified_at: 
sidebar:
  nav: "rma"
toc: true
---

### Create Location
Locations can be created to collect groups that are related to specific locations in your organization. For example think of a company that has two locations one in Copenhagen and one in Kolding. The people working in Copenhagen have different access to systems than the ones working in Koldning because for example they are using specific printers or parking ereas related to their location. By using the Location feature in Rigths Management App you can collect all the groups that are relates to specific locations and assign memebrships much easier and quicker. Navigate to Rights Management App > Locations and click on New.

![Locations list](/assets/images/location.webp)

Give a Name to the location, you can also write a Description and select a Valid from and a Valid to date. If a Valid from date is selected then the Identities that will added to this Location will get the memberships to the groups added to this location when the valid from date comes. On the same way those memberships will be deleted when the Valid to date comes. Then right-click on the upper grey bar and select Save.

![Location creation](/assets/images/createlocation.webp)

Scroll down to the related lists. From there you can add Active Directory Groups, Azure Active Directory Groups and ServiceNow groups to the Location. Select one of the three and click on New (in the example shown here we seleted the Azure Active Directory Groups. The process is the same for the other two group types). 

![Add groups to the location](/assets/images/aadlocation.webp)

Select the group you want to add to the location and the Entitlement Type of the user accounts that will become members of the group and click on Submit. User accounts that are linked to the Identities which belong to this location and have the same entitlement type as the Location-Group link will become members of the group.

![Add an azure active directory group to the location](/assets/images/locationlink.webp)

### Assign the Location to an Identity
Navigate to Rights Management App > Identities and select an Identity you want to add to the Location and open the record. In the Location field click on the search/magnifier button and select the Location you created. Then click on Update. On this way the Location is assigned to the Identity and the accounts linked to the Identity became members of the groups that are linked to the location.

![Add an identity to the Location](/assets/images/identitylocation.webp)

![Accounts added to groups](/assets/images/identityaddedtogroupsloc.webp)