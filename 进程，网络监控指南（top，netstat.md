# 进程，网络监控指南（top，netstat

## top

类似于任务管理器

d 修改刷新的时间 或者

```
top -d 10 *修改为10秒刷新一次
```

u 专门监控某一个用户

k 终止进程

P 按照PID进行排序

M 按照内存使用进行排序

C 按照CPU使用进行排序

## 查看系统网络情况（netstat

```
Netstat -an *按照一定顺序排列输出
netstat -p 显示哪个进程在调用
```

## 查看所有网络服务

```
netstat -anp
```

