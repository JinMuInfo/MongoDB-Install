1. 进入

```powershell
/etc/sysconfig/network-scripts  目录
```

2. 编辑：

```powershell
vi ifcfg-ens192
```

配置信息：

```powershell
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"

#这是修改的
BOOTPROTO="static"

DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens192"
UUID="9e13cd48-1afc-495a-bf20-f14d0bbfd1c1"
DEVICE="ens192"
ONBOOT="yes"

#这是新增加的：
IPADDR=192.168.3.125
GATEWAY=192.168.3.1
DNS1=192.168.3.1
NETMASK=255.255.255.0
```

然后重启网络服务：

```powershell
service network restart
```

