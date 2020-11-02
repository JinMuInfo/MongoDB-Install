动态查询日志文件

```powershell
tail -F -n 1 日志
```

启动nginx

```
systemctl  restart nginx
```

Nginx配置：server位置

```powershell
/usr/local/nginx/conf.d
```

log输出配置：

```powershell
/var/log/nginx
```

编辑文件删除指定行数：

```powershell
在Esc后输入
:1,100d (表示删除了第一行到第100行)
```

