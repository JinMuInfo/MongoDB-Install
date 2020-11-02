**安装mongoldb**

下载mongo4.0:

```powershell
mongodb-linux-x86_64-rhel70-4.0.19.tg
```

解压mongodb:

```powershell
tar -xvzf mongodb-linux-x86_64-rhel70-4.0.19.tgz
```

移动mongodb位置并修改名字：

```shell
mv mongodb-linux-x86_64-rhel70-4.0.19.tgz /home/ys/mongodb
```

创建data/db目录

```shell
mkdir -p data/db
```

创建mongo.conf文件：

```shell
touch mongo.conf
```

mongo.conf配置

```shell
   
mongod配置
#sharding:
#  clusterRole: shardsvr
#replication:
#  replSetName: shard1
net:
  bindIp: 0.0.0.0
  port: 27017
systemLog:
  destination: file
  path: /home/ys/logs/mongo.log
  logAppend: true
processManagement:
  fork: true
storage:
  dbPath: /home/ys/data/db/
  wiredTiger:
    engineConfig:
      cacheSizeGB: 1
#security:
#  authorization: enabled
#  keyFile: /var/mongodb/pki/keyfile

创建密钥文件并修改权限
openssl rand -base64 756 > <path-to-keyfile>
chmod 400 ~/appdb/conf/keyfile



mongos配置
systemLog:
  destination: file
  path: /home/liguanfei/sharding/logs/mongo-cfg.log
  logAppend: true
storage:
  dbPath: /home/liguanfei/sharding/data/mongo-cfg
  wiredTiger:
    engineConfig:
      cacheSizeGB: 1
net:
  bindIp: localhost,172.16.46.143
  port: 27001
replication:
  oplogSizeMB: 2048
  replSetName: mongo-cfg
sharding:
  clusterRole: configsvr
processManagement:
  fork: true
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/keyfile

```

配置环境变量：

```shell
vi /etc/profile  
  
export MONGODB_HOME=/home/ys/mongodb  
export PATH=$PATH:$MONGODB_HOME/bin  
```

安装完毕

启动mongoldb服务：

```powershell
方法一：mongod -dbpath /home/ys/data/db  使用数据库启动
方法二：mongod -f mongo.conf     使用配置文件启动   （推荐）
```

连接monogdb客户端：

```shell
mongo
```

查看mongodb数据库：

```powershell
show dbs
```

使用/创建数据库：

```powershell
use test   --test表示已有的数据库或者创建的数据库
```

创建数据：

```powershell
db.demo.insert({.....})   --demo表示创建的数据表
```

退出客户端：

```powershell
exit
```



**关闭mongodb服务方法:**

方法1.在客户端关闭服务

```powershell
use admin
db.shutdownServer()
```

方法2.在系统中关闭：

```powershell
1.查看进程号：
ps -ef | grep mongo
2.杀掉进程
kill -6 进程号

如果要杀掉所有MongoDB进程：
pkill mongo
```

方法3.关闭服务

```powershell
mongod -shutdown -dbpath /home/ys/data/db
```



企业版

前提安装：

```
sudo yum install cyrus-sasl cyrus-sasl-gssapi cyrus-sasl-plain krb5-libs libcurl lm_sensors-libs net-snmp net-snmp-agent-libs openldap openssl tcp_wrappers-libs
```

