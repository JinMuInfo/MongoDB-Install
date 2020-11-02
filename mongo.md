安装mongo

下载mongo4.0:

```shell
mongodb-linux-x86_64-rhel70-4.0.19.tgz
```

解压mongodb:

```shell
tar -xvzf mongodb-linux-x86_64-rhel70-4.0.19.tgz
```

移动mongoldb位置并修改名字：

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
    
#port 端口号
port=23000
#dbpath 数据库存储文件目录
dbpath=/home/mongo/mongodb-2.6.8/data
#logpath 日志路径
logpath=/home/mongo/mongodb-2.6.8/logs/mongodb.log
#logappend 日志追加形式  false:重新启动覆盖文件
logappend=true
#fork 后台启动
fork=true




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
方法一：mongod -dbpath /home/ys/data/db
方法二：mongod -f mongo.conf
```

连接monogdb客户端：

```shell
mongo
```



关闭mongodb服务方法:

方法1.在客户端关闭服务

```
use admin
db.shutdownServer()
```

方法2.在系统中关闭：

```shell
1.查看进程号：
ps aux | grep mongo
2.杀掉进程
kill -6 进程号
```

方法3.关闭服务

```shell
mongod -shutdown -dbpath /home/ys/data/db
```





修改localhost的用户名：

```shell
hostnamectl  set-hostname  新的用户名
```



关闭防火墙：

```powershell
systemctl stop firewalld
```

关闭防火墙开机自启：

```powershell
systemctl disable firewalld
```

查看防火墙状态：

```powershell
systemctl status firewalld
```

查看mms状态：

```shell
sudo systemctl status mongodb-mms-automation-agent.service
```



查看连接数：

```powershell
db.serverStatus().connections
```

输出结果：

```powershell
当前连接数    "current" : 数据
可用连接数  "available" : 数据
MongoDB一共创建线程数   "totalCreated" :数据
```



清空表数据

```
db.数据表名.remove({})
```



```
BASE_API: '"http://10.152.99.1:8000"'
```



后台jar包启动：

```
nohup java -jar filing-system-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev --jasypt.encryptor.password=SfXlqZmK4P257 &
```



Zip  -r  a.zip  a 打包

| gtjasfrz-mongodb | 复制集 | 10.181.112.201  10.181.112.202  10.181.112.203 |
| ---------------- | ------ | ---------------------------------------------- |
|                  |        |                                                |



