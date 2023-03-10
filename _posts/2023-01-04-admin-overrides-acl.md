---
author: LKH
title: "Working with Admin overrides ACL"
---

You may find yourself in a situation where you want to give access to a certain table or field for only users with a specific role. Admins per default implicitly have all roles and setting a specific role on an ACL will still allow users with admin role access.

One way to circumway this has been to set the field **Admin overrides** to false indicating that user must explicitly have the roles listed.

![Setting Admin overrides](/assets/images/post_admin-override-acl.webp)

Setting **Admin overrides** to false will however only take effect if **High-Security Plugin** is active and the system property **glide.security.admin.override.accessterm** is set to true.

Since our apps are installed in many different customer instances, assuming a certain configuration of a customer's ServiceNow instance is not an option for us. Thus we needed a secure way of disabling access to certain tables for admins.

The below ACL script condition is an example on how to evaluate if a user has a certian role explicitly.

```javascript
answer = gs.getSession().getRoles().indexOf('role_name') !== -1;
```

The script gets the current session and extracts an array of roles that the current user has. It then searches for the role **role_name** and if the index is not **-1**, meaning that the role is not in the array of roles that the user has, it will return **true**. Otherwise it will return **false** meaning that the user does not have the role.