## LDAP Authentication

KAP supports integration with LDAP servers for user authentication. This validation is achieved through the Spring Security framework, so it has a good versatility.

### Setup LDAP server
Before enabling LDAP authentication, you need an running LDAP service. If you already have it, contact the LDAP administrator to get the necessary information including connectivity inforamtion, organization structure, etc.

If you don't have LDAP server, need install one for KAP. The recommended server tool is OpenLDAP Server 2.4, as it is an open source implementation (under OpenLDAP Public License) and is the most popular LDAP server. It has been packaged in some Linux distributions like Red Hat Enterprise Linux; it can also be downloaded from http://www.openldap.org/

The installation may vary with different platform. You may need check different documents or tutorials like. Here we take CentOS 6.4 as an example:  

* Check installation
```shell
sudo find / -name openldap*
```

If not installed, install with yum:
```shell
sudo yum install -y openldap openldap-servers openldap-clients
```

* Configure after installation
```shell
cp /usr/share/openldap-servers/slapd.conf.obsolete /etc/openldap/slapd.conf
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
mv /etc/openldap/slapd.d{,.bak}
```

Edit slapd.conf, take *example.com* as example, as follows：

1．Set the directory suffix

Find this line:
```
suffix "dc=my-domain,dc=com"
```

Change it to:
```
suffix "dc=example,dc=com"
```

2．Set DN for LDAP Administrator:

Find line：
```
rootdn "cn=Manager,dc=my-domain,dc=com"
```

Change it to：
```
rootdn "cn=Manager,dc=example,dc=com"
```

3．Set password for LDAP administrator

Find line：

```
rootpw secret
```

Replacing the default value with an encrypted password string. To create an encrypted password string, type the following command:

```
slappasswd
```

When prompted, type and then re-type a password. The program prints the resulting encrypted password to the shell prompt.

Next, copy the newly created encrypted password into the /etc/openldap/slapd.conf on one of the rootpw lines and remove the hash mark (#). When finished, the line should look similar to the following example:
```
rootpw {SSHA}vv2y+i6V6esazrIv70xSSnNAJE18bb2u
```

4．Change the owner for these configuration files

```shell
chown ldap.ldap /etc/openldap/*
chown ldap.ldap /var/lib/ldap/*
```

5．Make new directory:
```shell
mkdir /etc/openldap/cacerts
```

6．Reboot the system, and then start the service:
```shell
sudo service slapd start
```

After the service is started, you can import some sample data.

7．Create new file example.ldif (three users and two groups are listed in this file)
```properties
# example.com
dn: dc=example,dc=com
objectClass: dcObject
objectClass: organization
o: Example, Inc.
dc: example

# Manager, example.com
dn: cn=Manager,dc=example,dc=com
cn: Manager
objectClass: organizationalRole

# People, example.com
dn: ou=People,dc=example,dc=com
ou: People
cn: People
objectClass: organizationalRole
objectClass: top

# johnny, People, example.com
dn: cn=johnny,ou=People,dc=example,dc=com
mail: johnny@example.io
ou: Manager
cn: johnny
sn: johnny wang
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
userPassword:: ZXhhbXBsZTEyMw==

# jenny, People, example.com
dn: cn=jenny,ou=People,dc=example,dc=com
mail: jenny@example.io
ou: Analyst
cn: jenny
sn: jenny liu
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
userPassword:: ZXhhbXBsZTEyMw==

# oliver, People, example.com
dn: cn=oliver,ou=People,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
cn: oliver
sn: oliver wang
mail: oliver@example.io
ou: Modeler
userPassword:: ZXhhbXBsZTEyMw==

# Groups, example.com
dn: ou=Groups,dc=example,dc=com
ou: Groups
objectClass: organizationalUnit
objectClass: top

# itpeople, Groups, example.com
dn: cn=itpeople,ou=Groups,dc=example,dc=com
cn: itpeople
objectClass: groupOfNames
objectClass: top
member: cn=johnny,ou=People,dc=example,dc=com
member: cn=oliver,ou=People,dc=example,dc=com

# admin, Groups, example.com
dn: cn=admin,ou=Groups,dc=example,dc=com
cn: admin
member: cn=jenny,ou=People,dc=example,dc=com
objectClass: groupOfNames
objectClass: top
```

8．Import it with command:

```shell
/usr/bin/ldapadd -x -W -D "cn=Manager,dc=example,dc=com" -f example.ldif
```

When prompt the password, enter the password of LDAP administrator. Then the import is succeed.

9．Change passwd

```shell
For example:
Change passwd for: dn: cn=jenny,ou=People,dc=example,dc=com
ldappasswd -xWD cn=Manager,dc=example,dc=com -S cn=jenny,ou=People,dc=example,dc=com
It will show like:
Input New Password
Verify New Password
LDAP Administrator password
```


### Configure LDAP Information in KAP

First, in `conf/kylin.properties`, configure the URL of the LDAP server, the necessary username and password (if the LDAP server is not anonymous). For security reason, the password here need be encrypted with AES, you can run code below to get the encrypted password:
```shell
${KYLIN_HOME}/bin/kylin.sh io.kyligence.kap.tool.general.CryptTool AES *your_password*
# ${crypted_password}
```

Then fill in kylin.properties:

```properties
# ldap.server=ldap://<your_ldap_host>:<port>
# ldap.username=<your_user_name>
# ldap.password=<your_password_hash>

ldap.server=ldap://127.0.0.1:389
ldap.username=cn=Manager,dc=example,dc=com
ldap.password=${crypted_password}
```

Second, provide user retrieval pattern, such as starting organization unit, filtering conditions etc; The following is an example for reference:

```properties
# LDAP user account directory
ldap.user.searchBase=ou=People,dc=example,dc=com
ldap.user.searchPattern=(&(cn={0}))
ldap.user.groupSearchBase=ou=Groups,dc=example,dc=com
```

If you need service accounts (for system integration) to access KAP, follow the example above to configure `ldap.service. *`, Otherwise leave them blank.
```properties
# LDAP service account directory
ldap.service.searchBase=ou=People,dc=example,dc=com
ldap.service.searchPattern=(&(cn={0}))
ldap.service.groupSearchBase=ou=Groups,dc=example,dc=com
```

### Configure Administrator Groups and Default Roles

KAP allows mapping an LDAP group to the administrator role: In kylin.properties, set "acl.adminRole" to "ROLE_" + GROUP_NAME (The GROUP_NAME need be in upper case). In this example, use the group "ADMIN" to manage all KAP administrators, then this property should be set to

```
acl.adminRole=ROLE_ADMIN
acl.defaultRole=ROLE_ANALYST,ROLE_MODELER
```

The attribute "acl.defaultRole" defines the roles that granted to any logged-on user. By default be both Analyst and Modeler.

### Enable LDAP

In conf/kylin.properties, set "kylin.security.profile=ldap"，and then restart KAP.

If we login with jenny which belongs to group `admin`, `System` menu will be displayed in the top bar:

![](images/ldap/login-with-jenny.png)

Otherwise, if we login with johnny which belongs to group `itpeople`, `System` menu won't be displayed in the top bar since the group `itpoeple` isn't admin group.

![](images/ldap/login-with-johnny.png)

### LDAP user information cache

After users login to KAP by LDAP authentication, their information will be cached to reduce overhead of accessing LDAP server. You can set configuration of cache time (seconds) and max cache entries in kylin.properties. Default values are shown below: 

```properties
kylin.server.auth-user-cache.expire-seconds=300
kylin.server.auth-user-cache.max-entries=100
```

