# 静态IP配置

配置文件为

/etc/sysconfig/network-scripts/ifcfg-eth

![image-20220718125343768](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220718125343768.png)

​                                                   *文件内容*

## 修改BOOTPROTO

![image-20220718125453514](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220718125453514.png)

修改为静态获取IP static

## 指定IP

IPADDR=

GATEWAY=

DNS1=

## 重启网络服务

service network restart

