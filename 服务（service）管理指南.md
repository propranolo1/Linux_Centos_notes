# 服务（service）管理指南

服务本质是一个进程，通常会监听某一些端口，等待其他程序的请求

又称守护进程

##  查看服务状态

```
service 服务名称 status
```

## 启动服务

```
service 服务名称 start
```

## 重启服务

```
service 服务名称 restart
```

## 停止服务

```
service 服务名称 stop
```

## 检测端口是否侦听

```
telnet ip 端口
```

## 查看服务

```
setup *图形化界面查看服务，并且可以设置服务的开机自启动
ls -l /etc/init.d *在命令行界面查看服务
```

![](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220720095627811.png)

![image-20220720101437877](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220720101437877.png)

##  给各个服务设置运行级别（chkconfig）

可以查看，设置服务的自启动

设置完毕后需要进行重启，才会生效

```
chkconfig --list *可以看到在各个级别下所有服务的自启动情况
chkconfig 服务名称 --list *查看该服务的自启动状态
chkconfig --level 级别 服务名称 on/off *设置服务在某个级别的自启动状态
	chkconfig --level 5 sshd on *设置sshd在级别5自启动
 
```

