## LDAP验证

KAP支持与LDAP服务器集成完成用户验证。这种验证是通过Spring Security框架实现的，所以具有良好的通用性。在启用LDAP验证之前，建议您联系LDAP管理员，以获取必要的信息。

#### LDAP服务器的安装
启用LDAP验证之前，需要一个运行的LDAP服务。如果已经有，联系LDAP管理员，以获取必要的信息，如服务器连接信息、人员和组织结构等。

如果没有可用的LDAP服务器，需要额外安装。推荐使用OpenLDAP Server 2.4，它是一个开源的实现（基于OpenLDAP Public License）并且也是最流行的LDAP服务器之一。很多企业Linux发行版已经内置了OpenLDAP服务，如果没有可以从官网下载： http://www.openldap.org/

OpenLDAP服务器的安装，依系统不同而略有区别。这里以CentOS 6.4为例介绍:  

* 安装之前检查

```shell
find / -name openldap*
```
如果没有安装，使用yum安装：
```shell
sudo yum install -y openldap openldap-servers openldap-clients
```

* 安装以后进行配置

```shell
cp /usr/share/openldap-servers/slapd.conf.obsolete /etc/openldap/slapd.conf
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
mv /etc/openldap/slapd.d{,.bak}
```

* 修改slapd.conf，步骤如下：

1．设置目录树的后缀

找到语句：
```
suffix "dc=my-domain,dc=com"
```
将其改为：
```
suffix "dc=example,dc=com"
```
2．该语句设置LDAP管理员的DN

找到语句：
```
rootdn "cn=Manager,dc=my-domain,dc=com"
```
将其改为：
```
rootdn "cn=Manager,dc=example,dc=com"
```
3．设置LDAP管理员的口令

找到语句：
```
rootpw secret
```

要创建一个新的加密密码，使用下面的命令：
```
slappasswd
```
输入要设置的密码，加密值会被输出在shell界面。然后将此值拷贝在rootpw这一行，如：
```
rootpw {SSHA}vv2y+i6V6esazrIv70xSSnNAJE18bb2u
```

4．为配置文件修改权限
```shell
chown ldap.ldap /etc/openldap/*
chown ldap.ldap /var/lib/ldap/*
```

5．新建目录/etc/openldap/cacerts
```shell
mkdir /etc/openldap/cacerts
```

6．重启系统，然后开启服务  
```shell
sudo service slapd start
```

7．新建文件 example.ldif
```ldif
dn: dc=example,dc=com
objectClass: dcObject
objectClass: organization
objectClass: top
dc: example
o: Example, Inc.

dn: cn=Manager,dc=example,dc=com
objectClass: organizationalRole
objectClass: top
cn: Manager

dn: ou=People,dc=example,dc=com
objectClass: organizationalRole
objectClass: top
cn: People
ou: People

# 用户： johnny 密码：example123
dn: cn=johnny,ou=People,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
cn: johnny
mail: johnny@example.io
ou: Manager
sn: johnny wang
userPassword:: ZXhhbXBsZTEyMw==

# 用户： jenny 密码：example123
dn: cn=jenny,ou=People,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
cn: jenny
mail: jenny@example.io
ou: Analyst
sn: jenny liu
userPassword:: ZXhhbXBsZTEyMw==

dn: ou=Groups,dc=example,dc=com
objectClass: organizationalUnit
objectClass: top
ou: Groups

dn: cn=itpeople,ou=Groups,dc=example,dc=com
objectClass: groupOfNames
objectClass: top
cn: itpeople
member: cn=johnny,ou=People,dc=example,dc=com
member: cn=jenny,ou=People,dc=example,dc=com
```

8．通过命令导入
```shell
/usr/bin/ldapadd -x -W -D "cn=Manager,dc=example,dc=com" -f example.ldif
```

提示输入密码，输入管理员的密码，导入成功。


#### 在KAP中配置LDAP服务器信息

首先，在conf/kylin.properties中，配置LDAP服务器的URL, 必要的用户名和密码（如果LDAP Server不是匿名访问）。为安全起见，这里的密码是需要加密（加密算法AES），您可以运行下面的命令来获得加密后的密码：
```shell
${KYLIN_HOME}/bin/kylin.sh io.kyligence.kap.tool.general.CryptTool AES kylin
# YeqVr9MakSFbgxEec9sBwg==
```
然后填写在kylin.properties中，如下：

```properties
# ldap.server=ldap://<your_ldap_host>:<port>
# ldap.username=<your_user_name>
# ldap.password=<your_password_hash>

ldap.server=ldap://127.0.0.1:389
ldap.username=cn=Manager,dc=example,dc=com
ldap.password=YeqVr9MakSFbgxEec9sBwg==
```

其次，提供检索用户信息的模式, 例如从某个节点开始查询，需要满足哪些条件等。下面是一个例子，供参考:

```properties
# LDAP user account directory
ldap.user.searchBase=ou=People,dc=example,dc=com
ldap.user.searchPattern=(&(cn={0}))
ldap.user.groupSearchBase=ou=Groups,dc=example,dc=com
```

如果您需要服务账户（供系统集成）可以访问KAP，那么依照上面的例子配置`ldap.service.*`，否则请将它们留空。
```properties
# LDAP service account directory
ldap.service.searchBase=ou=People,dc=example,dc=com
ldap.service.searchPattern=(&(cn={0}))
ldap.service.groupSearchBase=ou=Groups,dc=example,dc=com
```

### 配置管理员群组和默认角色

KAP允许您将一个LDAP群组映射成管理员角色：在kylin.properties中，将"acl.adminRole"设置为"ROLE_" + GROUP_NAME形式. 例如，在LDAP中使用群组"KAP-ADMIN-GROUP"来管理所有KAP管理员，那么这里应该设置为:

```
acl.adminRole=ROLE_KAP-ADMIN-GROUP
acl.defaultRole=ROLE_ANALYST,ROLE_MODELER
```

属性"acl.defaultRole"定义了赋予登录用户的权限，默认是分析师（ANALYST）和建模人员（MODELER）.

#### 启用LDAP

在conf/kylin.properties中，设置"kylin.security.profile=ldap"，然后重启KAP。



