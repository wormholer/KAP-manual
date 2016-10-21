## Cell Level Security

Cell Level Security helps administrator guarantee the data security on cell level, which is achieved by configuring access control file. The examples mentioned in this text are based on KAP's Sample.

#### Configurations

First of all, we need to compose a couple of configs:

**1.kylin.properties**

```kylin.cell.level.security.enable=``` is the switch，```true```: means on，```false```: means off，the default is:```false```

```kylin.cell.level.security.acl.config=``` specifies the name of access control file. In KAP Example, the default is: ```userctrl.acl```. Meanwhile, an access control file with the same name should exist in ```$KYLIN_HOME/conf```.

```kylin.query.access.controller=``` specifies the interface.

**2.User Access Control File**

In access control file, it defines the specific conditions for restricting user behavior. Here is a KAP example file: **userctrl.acl** *(the default split is whitespace)*:

| **login_user** | **kylin_sales.user_id** | **kylin_sales.region** | **kylin_sales.price** | **kylin_sales.lstg_format_name** |
| :------------: | :---------------------: | :--------------------: | :-------------------: | :------------------------------: |
|     ADMIN      |          ADMIN          |        Shanghai        |          yes          |               yes                |
|    ANALYST     |         ANALYST         |        Beijing         |          no           |               yes                |
|    MODELER     |         MODELER         |        Hongkong        |          yes          |                no                |


**Description**: *1. The first column is the user login name. 2. The remaining columns are attributes that belong to the user, will restrict the user's access in addition.*

At present, the access control file only supports two expressions, i.e. *boolean* and *equal*.


**Boolean**

A boolean value of yes indicates that the corresponding user can access the column, otherwise not. It  is always used for column-level security, such as: *kylin_sales.lstg_format_name*.

**Equal**
If value is evaluated as a string(non-Boolean), means the corresponding attribute will be used for row level security, for example: *kylin_sales.region*.

#### KAP Sample Test

*Some preparation*s

1. ```kylin.cell.level.security.enable=true```
2. ```kylin.cell.level.security.acl.config=userctrl.acl```
3. ```kylin.query.access.controller=io.kyligence.kap.query.security.KapAccessDecisionMaker```
4. `kylin_sales_cube is built`
5. `Grant MODELER to access Cube`. On KAP GUI, click on the **project management**, then select the project **learn_kylin**, click **privilege**, add grant: **MODELER** , **Cube query**

*Query Test*

a. ADMIN login KAP, and execute query：

```select * from kylin_sales```

As the results shown,  the records that fulfill the conditions are returned.

b. Execute the query：

```select lstg_site_id, sum(price) from kylin_sales group by lstg_site_id```

Modify ADMIN's *kylin_sales.lstg_format_name* to *Others*

execute again：

```select lstg_site_id, sum(price) from kylin_sales group by lstg_site_id```

Compare to the last result, the sum of price is much less.

c. Modify ADMIN's *kylin_sales.lstg_format_name* to *No*，execute：

```select * from kylin_sales```

The results shows an error: User ADMIN is not allowed to access kylin_sales.lstg_format_name.

Switch the login user to MODELER, modify the corresponding properties, then execute the above queries again.

**Notice**: After modifying the userctrl.acl, but haven't got the correct query results, it is probably due to the system cache. After disabling System->Disable Cache, try the query again. 