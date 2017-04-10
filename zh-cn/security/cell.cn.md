## 用户行列级别安全控制
帮助管理员控制行列级别数据安全，通过配置安全控制文件，定义用户的访问权限。本案例基于KAP自带Sample。

#### 配置文件
在使用前需要配置如下文件：

**1.kylin.properties**

```kap.security.cell-level-acl-enabled``` 是行列安全配置的开关，```true```表示打开，```false```表示关闭，默认值是```false```

```kap.security.cell-level-acl-config``` 指定安全控制文件名，KAP Sample里默认名为```userctrl.acl```。用户指定完安全配置文件名后，同时需要在```$KYLIN_HOME/conf```下新建一个同名文件。

```kylin.query.access.controller``` 指定实现安全控制的接口，当前只有一个实现```io.kyligence.kap.query.security.KapAccessDecisionMaker```

**2.用户安全控制文件**

在安全控制文件里，定义了限制用户行为的具体条件，下面是KAP Sample里userctrl.acl文件内容（**默认分隔符为空格**）：

| **login_user** | **kylin_sales.region** | **kylin_sales.price** | **kylin_sales.lstg_format_name** |
| :------------: | :--------------------: | :-------------------: | :------------------------------: |
|    *ADMIN*     |        Shanghai        |          yes          |               yes                |
|   *ANALYST*    |        Beijing         |          no           |               yes                |
|   *MODELER*    |        Hongkong        |          yes          |                no                |

*内容说明：*

* 第一列是KAP用户登录名。

* 剩余列是隶属于该用户的属性，可以进一步定义该用户访问权限。

  值标记为yes的表示对应的用户对该列有访问权限，no表示无访问权限，主要用于**列级别**的安全控制,如: ```kylin_sales.lstg_format_name＝yes```表示*ADMIN*用户可以访问列：```kylin_sales.lstg_format_name```。

  值标记为特定值，表示此列用于**行级别**的安全控制，如: ```kylin_sales.region＝Shanghai```表示*ADMIN*用户只能看到```kylin_sales.region```为```Shanghai```的记录。


#### KAP Sample测试

*一些准备工作*

1. ```kap.security.cell-level-acl-enabled=true```

2. ```kap.security.cell-level-acl-config=userctrl.acl```

3. ```kylin.query.access.controller=io.kyligence.kap.query.security.KapAccessDecisionMaker```

4. kylin_sales_cube构建完毕

5. 点击界面左上角的**项目管理** ，点击工程**learn_kylin**，展开后点击**权限**，然后添加**授权**，名称填:**MODELER**，许可填：**Cube查询**

*查询测试*

a.ADMIN用户登录KAP，执行查询：

```select * from kylin_sales```

由于在userctrl.acl里对ADMIN用户进行了安全控制，只返回符合安全控制条件的结果。

b.执行查询：

```select lstg_site_id, sum(price) from kylin_sales group by lstg_site_id```

修改```userctrl.acl```中```ADMIN```对应的```kylin_sales.lstg_format_name```为```Others```

再执行：

```select lstg_site_id, sum(price) from kylin_sales group by lstg_site_id```

对比上一次结果，发现第二次的sum(price)比第一次小，是因为后台自动过滤了```kylin_sales.lstg_format_name＝'Others'```纪录后的结果。

c.修改userctrl.acl, 把ADMIN的属性kylin_sales.lstg_format_name设置为No，再执行：

```select * from kylin_sales```

结果报错：用户ADMIN没有权限访问kylin_sales.lstg_format_name。

切换用户MODELER登录，修改对应的安全配置属性，再执行以上查询，同样在结果都做了安全处理。

**注意：** 如果修改了userctrl.acl配置属性，而查询结果没有随之改变，可能是系统开了缓存的原因。在System-Disable Cacheh后，再查询。
