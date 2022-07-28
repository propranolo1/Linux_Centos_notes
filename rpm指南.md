# rpm指南

类似于window的setup.exe

## 包的查询

```
rpm -qa | grep 包的名称 *查询是否安装了该包
```

![image-20220721171459567](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220721171459567.png)

### 名称的解析

el7 代表使用于centos7的系统

firefox 软件的名称

68.10.0 版本号

## 一次性查询所有包

```
rpm -qa | more
```

## 查询是否安装

```
rpm -q 软件名称
```

![image-20220721172406513](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220721172406513.png)

## 查询软件具体信息

```
rpm -qi 软件名称
```

![image-20220721172447339](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220721172447339.png)

## 查询软件安装位置

```
rpm -ql 软件名称
```

![image-20220721172514825](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220721172514825.png)

## 查询文件所属的软件包

```
rpm -qf 文件路径名
```

![image-20220721172534337](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220721172534337.png)

## 对软件进行卸载

```
rpm -e 软件名
rpm -e --nodeps 软件名 *对软件进行强制删除
```

## 对软件进行安装

```
rpm -ivh rpm包的路径 
```

