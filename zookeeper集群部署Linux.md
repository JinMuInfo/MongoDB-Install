1.下载zookeeper包并解压重命名为zookeeper。
2.进入zookeeper目录下创建`logs`日志目录。
3.创建`test`目录，进入`test`目录下创建zk1,zk2,zk3三个目录。
4.分别在**zk1**,**zk2**,**zk3**目录下创建`myid`文件，并分别赋值`1`，`2`，`3`.
5.在zookeeper目录下conf文件夹中创建配置文件。
6.分别创建`zoo1.cfg`,`zoo2.cfg`,`zoo3.cfg`三个配置文件，
配置信息为：

```powershell
tickTime=2000
dataDir=/root/zookeeper/test/zk1 			 分别为创建的三个目录 zk1,zk2,zk3
clientPort=2001  											 端口分别为：2001，2002，2003
initLimit=5
syncLimit=2

server.1=127.0.0.1:2887:3887        	 ip 分别为服务器的IP地址
server.2=127.0.0.1:2888:3888
server.3=127.0.0.1:2889:3889
```

7.启动zookeper集群：

```powershell
./bin/zkServer.sh start conf/zoo1.cfg
./bin/zkServer.sh start conf/zoo2.cfg
./bin/zkServer.sh start conf/zoo3.cfg
```

8.查看集群状态：

```powershell
./bin/zkServer.sh status conf/zoo1.cfg
./bin/zkServer.sh status conf/zoo2.cfg
./bin/zkServer.sh status conf/zoo3.cfg
```

其中**“leader”**为主节点，**“follower”**为从节点