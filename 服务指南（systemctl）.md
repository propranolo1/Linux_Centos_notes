# 服务指南（systemctl）

systemd中的服务是由文件控制的

systemd中使用的是单元配置文件，而不是脚本

#### 关于systemctl的启动

systemd是**并行启动**的技术，所有服务尽可能并行启动，使用**缓冲池**的方式解决服务的依赖性

同时尽可能少的启动服务，提高启动的速度

systemd是**按需启动**的，只有服务被请求时才会启动服务，请求结束后便会进行关闭，节约系统资源

systemd相比system V，当父进程被结束后，子进程也会被关闭(利用Cgroup实现，由于子进程创建时会自动继承父进程的Cgroup，所以他们的Cgroup是相同的，systemd可以通过查询Cgroup，找到所有的子进程进行关闭)

#### 关于循环依赖（待完善

#### 关于systemctl日志

systemd对日志系统也进行了升级，使用二进制进行保存；

同时还具有安全性提高，可以指型高，资源消耗少，可扩展，结构简单等特性

扩展：

syslog日志不安全原因在于日志消息由进程自主产生，syslog不会验证消息的来源是否属实，可被冒充

日志消息也没有严格的格式，自动化处理日志困难

#### 关于unit

systemd将启动过程抽象为一个个的unit(单元)

```
systemctl list-units *可以查看units
```

![image-20220802090610467](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220802090610467.png)

每一个unit后都带有后缀，service表示该unit是一个后台服务进程

一般unit会储存在 /etc/systemd/system 和 /usr/lib/systemd/system 中

etc目录供系统管理员使用，usr通常是安装程序使用

可以在usr下查看unit文件

![image-20220802091736063](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220802091736063.png)

## 启动，停止，重启，重载 服务

```
systemctl start 服务名称
systemctl stop 服务名称
systemctl restart 服务名称
systemctl reload 服务名称
```

## 服务状态查询

```
systemctl status 服务名称
```

## 服务开机自启动相关

### 查看服务是否自动启动

```
systemctl is-enabled 服务名称
```

### 设置开机自启动

```
systemctl enable 服务名称
```

### 关闭开机自启动

```
systemctl disable 服务名称
```

### 在系统中查看那些服务开机自启动

```
cd /etc/systemd/system/multi-user.target.wants/
ls
```

![image-20220801093453753](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220801093453753.png)

## 设置启动模式（可参考linux启动）

```
systemctl set-default mulit-user.target *设置为完全多用户
systemctl set-default graphical.target *设置为图形化
```

