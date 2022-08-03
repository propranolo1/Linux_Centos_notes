# RPM指南

Red Hat Package Manager

类似于window的setup.exe

## 包的查询

```
rpm -qa | grep 包的名称 *查询是否安装了该包
```

![image-20220721171459567](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220721171459567.png)

### 名称的解析

el7 代表使用于centos7的系统

firefox 软件的名称

68.10.0 版本号

## 查看已安装的软件包

```
rpm -qa | more
```

## 查询是否安装

```
rpm -q 软件名称
```

![image-20220721172406513](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220721172406513.png)

## 查询软件具体信息

```
rpm -qi 软件名称
```

![image-20220721172447339](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220721172447339.png)

## 查询软件安装位置

```
rpm -ql 软件名称
```

![image-20220721172514825](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220721172514825.png)

## 查询文件所属的软件包

```
rpm -qf 文件路径名
```

![image-20220721172534337](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220721172534337.png)

## 对软件进行卸载

```
rpm -e 软件名
rpm -e --nodeps 软件名 *对软件进行强制删除（
```

## 对软件进行安装

```
rpm -ivh rpm包的路径 
```

| 参数     | 说明                                             |
| -------- | ------------------------------------------------ |
| -i       | 安装软件时显示软件包的相关信息                   |
| -v       | 安装软件显示命令的执行过程                       |
| -h       | 安装软件时输出hash记号:#                         |
| --force  | 强制安装，当软件包已经安装但需要重新安装时可使用 |
| --nodeps | 禁止检查依赖关系                                 |

对软件进行升级

```
rpm -Uvh 安装包名称
```

| 参数 | 说明           |
| ---- | -------------- |
| -U   | 升级指定的软件 |

