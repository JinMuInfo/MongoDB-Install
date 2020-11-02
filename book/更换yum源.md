1. 备份当前yun源

   ```powershell
   cd /etc/yum.repos.d/
   cp CentOS-Base.repo CentOS-Base-repo.bak
   ```

2. 使用wget下载阿里云yum源repo文件

   ```powershell
   wget http://mirrors.aliyun.com/repo/Centos-7.repo
   ```

3. 清理旧的软件源

   ```powershell
   yum clean all
   ```

4. 把下载下来阿里云repo文件设置成为默认源

   ```powershell
   mv Centos-7.repo CentOS-Base.repo
   ```

5. 生成阿里云yum源缓存并更新yum源

   ```powershell
   yum makecache
   yum update
   ```



## Yum install phpldapadmin

1. ```
   ## Steps
   
   ### Update system packages
   
   1. $ sudo yum update && sudo yum upgrade
   
   ### Install extra PHP packages
   
   You need to install php-ldap and a few other php packages needed to run phpLDAPadmin.
   
   1. $ sudo yum install php-ldap php-mbstring php-pear php-xml
   
   The Extra Packages for Enterprise Linux (EPEL) release updates have to be installed because phpLDAPadmin is not available in the main repository.
   
   1. $ sudo yum install epel-release
   
   ### Start LDAP services
   
   The ldap services need to be started and also be enabled to start automatically on boot up.
   
   1. $ sudo systemctl start sldap && sudo systemctl enable sldap
   
   ### Install the phpLDAPadmin
   
   1. $ sudo yum -y install phpldapadmin
   ```

   