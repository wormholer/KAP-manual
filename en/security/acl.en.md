## Manage ACL

In the `Cubes` page，double click the cube line to check the detail inforamtion; here we will focus on the `Access` tab. Click `+Grant` button to grant new permission.

![]( /images/security/acl/grant.png)

There are four access levels on a cube; Move cursor to the `?` icon will see the detail description of these accesses.

![]( /images/security/acl/grantInfo.png)

There are two access objects: `User`和`Role`. `Role` means an user group.

### Grant User Permission
* Select `User` as type, enter the user name and then select the permission that you want to grant.

     ![]( /images/security/acl/grant-user.png)

* Click `Grant` button to submit. After this you will see a new line be added to the permission table. You can select one line to update the permission, or click `Revoke` button to delete the permission.

     ![]( /images/security/acl/user-update.png)

### Grant Role Permission
* Select `Role` as type, select a role (user group) in the drop-down list (the list is retrieved from LDAP server), and also select the permission that you want to grant.

* Click `Grant` button to submit. Other operations are the same as for user.
