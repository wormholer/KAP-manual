## User management

Login on KAP, users can click `system button` on navigation bar to enter system management page. Then click user bar on the left side to enter users management page. Only administrator could get the access to the user management page. 

KAP is set to initial three users, and corresponding account information would be admin (ADMIN/KYLIN), modeler(MODELER/MODELER), analyst(ANALYST/ANALYST).

![](images/users/user.png)

#### Characters
The KAP system defaults to three characters: administrator, modeler and analyst.
Administrator has whole authority related to system. 

Analyst has main authority to query, such as checking authorized entity information(project, model, Cube and so on) and querying built data.

Modeler is capable to build model, Cube, and other actions(such as building, editing, deleting, cleaning, enabling, disabling, which including all authorities of the analyst).


#### Add users
Administrator could add new users and input user name, password and character information. 

![](images/users/add_user.png)

#### Edit user information
Once users are added, administrator would be able to edit user's character information.

![](images/users/edit_user.png)

#### Change password
Administrator is able to change password and only needs to input twice the new password. 

![](images/users/update_password.png)

#### Delete users
Administrator could delete users within the page. Please be noticed that deleted users cannot be restored. 

#### Enable/disable users
Administrator could enable or disable users and disabled users cannot login the system. 

#### Change password as normal users
Click the `welcome tag` on the far right side of navigation bar, then users could choose the `setting` option and click it to enter the personal change password page.      

![](images/users/setting_update_password.png)


