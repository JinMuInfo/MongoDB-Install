- 操作系统：**CentOS 7.4.1708 最小安装（已关闭 SELinux 和防火墙）**
- 应用软件：**openldap 2.4.44 、phpldapadmin 1.2.3**

**mongodb需使用enterprise版。**

关闭SELiuux：

查看状态:sestets

永久关闭: vi /etc/selinux/config

将SELINUX=enforcing改成SELUNUX=disabled

重启服务器



关闭防火墙

查看状态:

```
systemctl status firewalld
```

关闭防火墙：

```
systemctl stop firewalld
```

关闭防火墙开机自启：systemctl disable firewalld



~~更改hostname~~

```
Hostnamectl set-hostname  <hostname>
```



设置域名：

```
vi /etc/hosts

jinmuopenldap.com 192.168.3.204
```



### **安装 OpenLDAP**

```text
yum install -y openldap-clients openldap-servers 
```

### **1.创建数据库配置文件**

直接套用模版即可。

```text
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG && chown ldap:ldap /var/lib/ldap/DB_CONFIG 
```

### **2. 启动 OpenLDAP 服务**

```text
systemctl start slapd 
```

如需开机启动，请执行以下命令。

```text
systemctl enable slapd 
```

如果没有出现错误，那么 OpenLDAP 的安装就算完成了，不过当前的目录数据库还是空的，下面我们要对数据库进行初始化。

### **3.设置 OpenLDAP 管理员密码**

**3.1生成密码**

```text
slappasswd 
```

执行完该命令之后，请输入您要设定的密码。然后会生成 `{SSHA}xxxxx` 这样一行东西，请把它记下来。

生成的：

```
管理员密码:{SSHA}LkrtYFLSo/164fKsg+/JH7PNudSZG4yX      123456
```

**生成 LDIF 文件**

```text
vi chrootpw.ldif   (创建并编辑)
```

```text
dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}LkrtYFLSo/164fKsg+/JH7PNudSZG4yX  #{SSHA}xxxxx 修改为管理员密码
```

**3.2执行 LDIF 文件**

```text
ldapadd -Y EXTERNAL -H ldapi:/// -f chrootpw.ldif 
```

### **4导入预设的模式**

```text
find /etc/openldap/schema/ -name "*.ldif" -exec ldapadd -Y EXTERNAL -H ldapi:/// -D "cn=config" -f {} \; 
```



### **新建一个根节点**

### **5.生成根节点管理员密码**

> **注意**

- 根节点管理员密码与 OpenLDAP 管理员密码不是同一回事！一个 LDAP 数据库可以包含多个目录树。

```text
slappasswd 
```

执行完该命令之后，请输入您要设定的密码。然后会生成 `{SSHA}xxxxx` 这样一行东西，请把它记下来。

根结点管理员密码：

```
{SSHA}nIy7Oxu722n67FP6ZmdF5Q3tSQGLSvHG    123
```

### 6 生成LDIF文件

需要用到上边设置的域名。 域名为：jimuopenldap.com

所以根结点的DN为：

```
 dc=jimuopenldap,dc=com
```

然后：

```
vi chdomain.ldif   (创建并编辑)

dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
  read by dn.base="cn=Manager,dc=xxx,dc=xxx" read by * none #修改 dc=xxx,dc=xxx 为自己的域名

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=xxx,dc=xxx    #修改 dc=xxx,dc=xxx 为自己的域名

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=xxx,dc=xxx #修改 dc=xxx,dc=xxx 为自己的域名

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}xxxxx  #{SSHA}xxxxx 修改为上一步记下来的值

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by
  dn="cn=Manager,dc=xxx,dc=xxx" write by anonymous auth by self write by * none #修改 dc=xxx,dc=xxx 为自己的域名
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=Manager,dc=xxx,dc=xxx" write by * read #修改 dc=xxx,dc=xxx 为自己的域名
```



**执行 LDIF 文件**

```text
ldapmodify -Y EXTERNAL -H ldapi:/// -f chdomain.ldif 
```



### **添加用户、组节点**

**5.8.1 生成 LDIF 文件**

```text
vi basedomain.ldif （创建并编辑）

dn: dc=xxx,dc=xxx   #修改 dc=xxx,dc=xxx 为自己的域名
objectClass: top
objectClass: dcObject
objectclass: organization
o: root_ldap
dc: xxx #修改 xxx 为自己域名第一个点左边的内容

dn: cn=Manager,dc=xxx,dc=xxx    #修改 dc=xxx,dc=xxx 为自己的域名
objectClass: organizationalRole
cn: Manager
description: Directory Manager

dn: ou=People,dc=xxx,dc=xxx #修改 dc=xxx,dc=xxx 为自己的域名
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=xxx,dc=xxx  #修改 dc=xxx,dc=xxx 为自己的域名
objectClass: organizationalUnit
ou: Group
```



**执行 LDIF 文件**

请先修改下面命令的 `dc=xxx,dc=xxx` 为自己的域名然后再执行。

```text
ldapadd -x -D cn=Manager,dc=xxx,dc=xxx -W -f basedomain.ldif 
```

然后需要输入设定的根节点管理员密码。



### 安装phpLDAPadmin界面化工具

## yun install phpldapadin

步骤：

### Update system packages

```
sudo yum update && sudo yum upgrade
```

### Install extra PHP packages

You need to install php-ldap and a few other php packages needed to run phpLDAPadmin.

```
 sudo yum install php-ldap php-mbstring php-pear php-xml
```

The Extra Packages for Enterprise Linux (EPEL) release updates have to be installed because phpLDAPadmin is not available in the main repository.

```
 sudo yum install epel-release
```

### Start LDAP services

The ldap services need to be started and also be enabled to start automatically on boot up.

```
sudo systemctl start sldap && sudo systemctl enable sldap
```

### Install the phpLDAPadmin

```
sudo yum -y install phpldapadmin
```

接下来：

```text
vi /etc/phpldapadmin/config.php 

$servers = new Datastore();
$servers->newServer('ldap_pla');
$servers->setValue('server','name','My LDAP Server');
$servers->setValue('server','host','127.0.0.1');  # host改为自己的服务器
$servers->setValue('server','port',389);  #端口自己任命
$servers->setValue('server','base',array('dc=xxx,dc=xxx')); #修改 dc=xxx,dc=xxx 为自己的域名
$servers->setValue('login','auth_type','session');
$servers->setValue('login','bind_id','cn=Manager,dc=xxx,dc=xxx');   #修改 dc=xxx,dc=xxx 为自己的域名
$servers->setValue('login','bind_pass','<密码>'); #填入 5.7.1 设定的根节点管理员密码
$servers->setValue('server','tls',false); 
```

编辑文件phpMyAdmin.conf

```
sudo vi /etc/httpd/conf.d/phpMyAdmin.conf

<Directory /usr/share/phpMyAdmin/>
   AddDefaultCharset UTF-8

   <IfModule mod_authz_core.c>
     # Apache 2.4
     <RequireAny>
       Require all granted      #(将phpadapadmin设置成全局)
     </RequireAny>
   </IfModule>
   <IfModule !mod_authz_core.c>
     # Apache 2.2
     Order Deny,Allow
     Deny from All
     Allow from 192.168.3.204
     Allow from ::1
   </IfModule>
</Directory>
```



##### 在自己的电脑里边配置hosts

然后浏览器输入：

```
192.168.3.204/phpldapadmin
```

登陆：

用户名:cn=Manager,dc=jinmuopenldap,dc=com

密码：123（根用户密码）