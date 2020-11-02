安装ops需要先安装Mongo服务

下载安装ops4.2的安装包：

```powershell
mongodb-mms-4.2.16.56965.20200805T1936Z-1.x86_64.tar.gz
```

解压安装包：

```powershell
tar -zxvf mongodb-mms-4.2.16.56965.20200805T1936Z-1.x86_64.tar.gz
```

将安装包修改：

```powershell
mv mongodb-mms-4.2.16.56965.20200805T1936Z-1.x86_64.tar.gz  mongodb-mms
```

进入目录：

```powershell
cd mongodb-mms
```

创建`logs`日志目录

```powershell
mkdir logs
```

进入conf目录修改配置信息

```powershell
cd conf
```

修改配置信息：

```powershell
vi conf-mms.properties
```

其中的信息：

```powershell
mongo.mongoUri=mongodb://192.168.3.125:27017/?maxPoolSize=150
mongo.ssl=false             将monogdb安装的的IP替换掉，如果是在本服务器上安装的就修改为：127.0.0.1:27017
															27017为MongoDB的默认端口号
```

启动ops服务：在bin目录下

```powershell
./mongodb-mms start
```

重新启动：

```powershell
./mongodb-mms restart
```

关闭服务：

```powershell
./mongodb-mms stop
```





**Mongoldb ops manager 部署**

依次打开Agents->Downloads&settings

自动化代理安装

下载代理:

```shell
curl -OL http://192.168.43.248:8080/download/agent/automation/mongodb-mms-automation-agent-manager-5.4.24.5565-1.x86_64.rhel7.rpm
```

安装软件包：

```shell
sudo rpm -U mongodb-mms-automation-agent-manager-5.4.24.5565-1.x86_64.rhel7.rpm
```

产生密钥：

```shell
5f3bfbc9f95e590ab3473c8c03679a28260f46ca7ce291a45912fa59
```

打开配置文件：

```shell
sudo vi /etc/mongodb-mms/automation-agent.config
```

修改内容：

```shell
mmsGroupI=5f3c5b09f95e5943b4eabb53
mmsApiKey=5f3c617df95e5943b4eabd967929c5e34224b2bbefaec2867f99bdd1
mmsBaseUrl=http://192.168.43.248:8080
```

创建` /data` 来储存mongoldb数据

```shell
sudo mkdir -p /data
sudo chown mongod:mongod /data
```

启动代理：

```shell
sudo systemctl start mongodb-mms-automation-agent.service
```

在SUSE中可能需要运行：

```shell
sudo /sbin/service mongodb-mms-automation-agent start
```





mmsGroupId=5f965916c8c22c0720b41053



mmsApiKey=**5f96596fc8c22c0720b4121dac69e28fd8fbf87f46a76fb65e2e1a94**



mmsBaseUrl=http://192.168.3.30:8080



```
public class test {
    public static void main(String[] args) {
        for (int b = 10; b > 0; b--) {
            for (int a = 1; a <= 100000; a++) {
                if (a % b == 0) {
                    System.out.println("db.coll.find({})");
                } else {
                    System.out.println("db.coll.insert({\"name\", \"MongoDB\",\"type\", \"database\",\"count\", 1,})");
                }
            }
        }

    }
}
```



2020-10-20T14:40:32.336+0800 I COMMAND [conn36979] command BUSIN.USER_UNGET_WALLET command: aggregate { aggregate: "USER_UNGET_WALLET", pipeline: [ { $match: { ACTIVITY_CODE: "201708_AXNC", END_DATE: { $gte: 20201020 }, WALLET_NUM: { $gte: 5 }, MIN_ALLOW_GET_DATE: { $lt: 20201020 }, BE_STOLEN_DATE: { $ne: 20201020 } } }, { $sort: { BE_STOLEN_DATE: -1 } }, { $limit: 50 } ], cursor: {} } planSummary: IXSCAN { ACTIVITY_CODE: 1, END_DATE: 1, WALLET_NUM: 1, MIN_ALLOW_GET_DATE: 1, BE_STOLEN_DATE: 1 } keysExamined:312507 docsExamined:312278 hasSortStage:1 cursorExhausted:1 numYields:2442 nreturned:50 reslen:11297 locks:{ Global: { acquireCount: { r: 4978 } }, Database: { acquireCount: { r: 2489 } }, Collection: { acquireCount: { r: 2488 } } } protocol:op_command 1012ms