**安装kafka先要创建zookeeper集群，可以自己先创建zookeeper集群，也可以使用kakfa自带的zookeeper集群**

安装kafka

官网下载地址：

```powershell
https://kafka.apache.org/downloads
```

版本下载：`2.1.1`

解压缩：

```powershell
tar -zxvf kafka_2.11-2.1.1.tgz
```

修改名称：

```powershell
mv kafka_2.11-2.1.1 kafaka
```

进入目录：

```powershell
cd kafka
```

创建日志文件：

```powershell
mkdir logs
```

进入`config`目录：

```powershell
cd config
```

修改zookeeper配置：

```powershell
vi zookeeper.properties
```



kafaka配置：

```powershell
cd config/
```

修改配置文件：

```powershell
vi server.properties
```

broker.id