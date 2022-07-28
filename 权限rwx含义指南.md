# 权限rwx含义指南

![image-20220715182339152](C:\Users\JInXiN\AppData\Roaming\Typora\typora-user-images\image-20220715182339152.png)

## 第一个字符

l：软连接

c：字符设备【键盘，鼠标】

b：块文件，硬盘

d：目录

## rwx作用到文件

r（4）：read 读权限

w（2）：write 写权限，可以修改，但不可删除

x（1）：execute 执行的权限

（rwx可用数字进行表示

## rwx作用到文件

r：read 可以读取，ls查看目录内容

w：write 目录内可以创建删除重命名

x：execute 可以进入该目录

## rwx再不同位置表示的含义

第一个rwx（u）：代表文件所有者权限

第二个rwx（g）：代表文件所在组的用户拥有的权限

第三个rwx（o）：代表文件其他组用户拥有的权限

## 数字的含义

如果是文件，表示硬链接的数量

如果是目录，表示子目录的个数

