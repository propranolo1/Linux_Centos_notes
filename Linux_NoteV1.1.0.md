# VMware虚拟机的网络连接方式

## 桥接模式

和主机使用相同的网段，

方便使用，可以和其他系统通讯，

但如果数量多会造成IP地址冲突

## NAT模式

虚拟机和主机使用不同的网段，

虚拟机与外网连接时使用NAT的方式，

不会造成外部环境的IP地址冲突

## 仅主机模式

虚拟机是独立的主机，

不能访问外网

#  linux目录结构及作用

树状目录文件，/为跟目录

一切介为文件

### /bin 

存放命令

### /sbin 

管理员使用的系统管理程序

### /home 

存放普通用户的主目录

### /root 

超级用户的主目录

### /lib 

系统开机所需要的最基本的动态连接共享库

### /lost+found

 当系统非法关机时，该目录会存放文件

### /etc 

所有的系统管理所需要的配置文件和子目录

### /usr 

用户的应用程序和文件都存放在该目录下，类似program files

### /boot 

启动linux的核心文件

### /proc 

虚拟的目录，是系统内存的映射，访问这个目录来获取系统的信息（和内核相关

### /srv

service，存放一些服务启动后需要提取的数据（和内核相关

### /sys 

内核相关

### /tmp 

用来存放临时文件

### /dev  

管理设备，和硬件相关

### /media 

和u盘，光驱相关

### /mnt 

用于临时挂载

### /opt 

存放安装包，默认空

### /usr/local 

安装过后存放的位置

### /var 

存放日志，不断扩充的文件存放在此

### selinux 

安全子系统

# linux的启动以及运行级别

## GRUB(待完善)

操作系统引导程序，可以让用户在多个不通操作系统中选择启动的操作系统

还可以向操作系统内核传递参数

安装在/boot中（boot保存文件主要为linux内核，内存映像

GRUB通过引导boot中的文件建立内核运行环境

/boot/grub2/grub.cfg 是grub的配置文件

![image-20220801101317454](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220801101317454.png)

GRUB界面提供了两个选项

一个是正常启动系统

一个是启动系统的救援模式

## 启动过程

1. 开机自检

2. 从MBR中读取GRUB(引导程序)

3. GRUB根据配置文件显示引导菜单

4. 如果选择linux，则加载linux内核文件

5. 当内核载入内存后，GRUB任务完成。CPU开始执行linux内核代码，建立内核运行环境

6. 内核代码执行完后，开始执行linux的第一个进程——systemd，进程号是1

   ![image-20220801091659482](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220801091659482.png)

7. 启动后，读取/etc/systemd/system/default.target(该文件设置系统的运行级别)

![image-20220801091625172](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220801091625172.png)

## linux运行级别

| 运行级别 | 名称              | 作用                 |
| -------- | ----------------- | -------------------- |
| init0    | poweroff.target   | 关机                 |
| init1    | rescue.target     | 单用户模式           |
| init2    | multi-user.target | 多用户，但是没有网络 |
| init3    | Multi-user.target | 完全多用户模式       |
| init4    | multi-user.target | 无使用               |
| init5    | graphical.target  | X11,对应图形化界面   |
| init6    | reboot.target     | 重启                 |

### 查看当前运行级别

```
runlevel
```

![image-20220801092214097](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220801092214097.png)

N的位置代表 上一次运行的级别

3的位置代表 目前运行的界别

## GUI的安装

### 安装gnome包

```
yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
```

### 更新系统运行的级别

```
ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.targ 
*开机默认以gui启动通过建立软连接的方式
```

```
init 5 *临时进入gui界面
```

# yum配置

## 本地yum配置

### 临时挂载

```
mkdir /mnt/sr0

mount /dev/sr0 /mnt/sr0

df
```

### 进入yum.repo.d备份文件

```
mkdir /etc/yum.repo.d/backup

cp -f /etc.yum.repos.d/*.repo /etc/yum.repos.d/backup
```

### 删除文件，剩下Media

```
rm /etc/yum.repos.d/*
```

### 修改media

把文件修改为挂载的位置

enabled改为1

![image-20220619094404355](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619094404355.png)

### 清除和重建缓存

```
yum clean all

yum makecache
```

## 阿里云yum源配置

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

保留阿里源，删除备份其他

## 安装

```
yum install -y vim
```

## 卸载

```
yum remove 软件名称
```

## 查看安装包是否在服务器上

```
yum list | grep 要查询的安装包
```

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

# vim使用

### 进入当前光标位置之前输入（i

### 进入当前光标位置之后输入（a

### 显示行号（:set nu

### 不显示行号（:set nonu

### 退出（:q

### 强制退出（:q！

### 保存退出（:wq

### 复制（yy

```
5yy *表示复制光标往下的5行内容
```

### 删除光标所在行（dd

### 黏贴（p

# 时间日期查询设置

## 显示当前日期（date

```
date *显示当前日期
date “+%Y-%m-%d” %H:%M:%S*显示年月日
date “+%Y" *显示完整年（2022）如果时小写y则显示22
```

### 设置日期

```
date -s ”2022-6-20 19：15：15“ *将日期修改为引号内的日期
```

## 显示日历（cal

```
cal 2077 *显示2077年的日历
```

## 硬件时间（hwclock

```
hwclock --show
```

### 修改硬件时间

```
hwclock --set --date "2022-07-31 23:57:00"
```

### 将硬件时间和系统时间进行同步

```
hwclock --hctosys
```



# 搜索查找

## 搜索（find

find 搜索范围 选项

```
find /mnt -name a.txt *在mnt目录下搜索名称为a.txt的文件
find /home -user xin *在home目录搜索属于用户xin的文件
find / -size +2M *在根目录下搜索大于2m的文件
```

## 快速定位文件路径（locate

通过数据库搜索文件路径，需要定期更新

```
updatedb *建立locate数据库
locate a.txt *定位a.txt的位置
```

## 过滤（grep，|）

```
cat a.txt | grep yes *查找a.txt中是否有yes
cat a.txt | grep -n yes *与上一条意思相同，但-n可以显示所在行号
cat a.txt | grep -ni yes *-i的含义时不区分大小写

```

# 磁盘查询

## 查看目前磁盘使用情况（df

```
df -h
```

![image-20220717084417865](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220717084417865.png)

## 查看文件或目录的磁盘占用情况（du

| 参数          | 说明               |
| ------------- | ------------------ |
| -h            | 更加直观的表示     |
| -a            | 对全部文件进行统计 |
| -s            | 只显示大小的汇总   |
| --max-depth=1 | 子目录深度         |
| -c            | 最后加上总计       |

```
du -ach
```

![image-20220717084933480](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220717084933480.png)

## 对目录下的文件进行统计（ls

```
ls -l /home | grep "-" | wc -l
*第一个字符为-的才属于文件，wc  -l是对数量进行统计
```

```
ls -l /home | grep "d" | wc -l *对目录个数进行统计
```

```
ls -lR /home | grep "-" | wc -l *进行递归统计
```

# 分区

## 分区基础

### MBR

最大支持2TB

系统只能安装在主分区

最多支持四个主分区

扩展分区要占一个主分区

### GPT

最大支持18EB

指出无限多个主分区

## 硬盘说明

```
[硬盘类型][硬盘属性][硬盘分区]
```

| 参数 | 代表硬盘类型 |
| ---- | ------------ |
| hd   | IDE硬盘      |
| sd   | SCSI硬盘     |

| 参数 | 硬盘属性   |
| ---- | ---------- |
| a    | 基本盘     |
| b    | 从属盘     |
| c    | 辅助主盘   |
| d    | 辅助从属盘 |

数字 代表分区

sda3 SCSI硬盘的第三个主分区

## 查看系统的分区和挂载的情况（lsblk

```
lsblk -f *可查看uuid
```

![image-20220717081117199](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220717081117199.png)

## 格式化硬盘(mkfs

| 参数                        | 说明                 |
| --------------------------- | -------------------- |
| -t [ext2/ext3/ext4/xfs/...] | 设定文件系统类型     |
| -c                          | 检查磁盘是否存在坏道 |

```
mkfs -t ext4 硬盘名称
```

## 临时挂载（mount

```
mount 硬盘名称 挂载位置
```

## 永久挂载

```
1:
vim /etc/fstab *进入fstab，里面储存挂载情况
```

```
2:
编辑格式
硬盘名称 挂载点 ext4 defaults 0 0
*第一个0的位置表示是否对内容进行备份
*第二个0的位置表示是否进行自检，
	0为不进行自检
	1表示每次都运行
	2表示非正常关机或达到最大加载
```

```
3:
mount -a *自动挂载
```

# 压缩解压缩

## 压缩和解压缩（gzip，gunzip

```
gzip a.txt *将a.txt压缩为a.txt.gz，且不保留源文件
gunzip a.txt.gz *解压文件
```

## zip和unzip

```
zip -r a.zip /home/ *把home整个目录压缩为a.zip
unzip -d /opt/tmp/ a.zip *把a.zip解压到/opt/tmp中
```

## 打包指令（tar

| 参数 | 说明               |
| ---- | ------------------ |
| -c   | 产生tar打包文件    |
| -v   | 显示详细信息       |
| -f   | 指定压缩后的文件名 |
| -z   | 打包同时压缩       |
| -x   | 解压tar文件        |

```
tar -zcvf a.tar.gz a.txt b.txt *压缩并打包a.txt b.txt 压缩包的文件名为a.tar.gz
tar -zcvf home.tar.gz /home/ *也可以对目录进行打包压缩
tar -zxvf a.tar.gz *解压
tar -zxvf home.tar.gz -C /opt/ *指定解压到/opt/中，需要加-C，如果不加-Cz

```

# 权限指南

![image-20220715182339152](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220715182339152.png)

## 第一个字符代表含义

| 参数 | 说明                       |
| ---- | -------------------------- |
| d    | 该文件是目录类型           |
| l    | 该文件为连接               |
| b    | 设备文件，该文件已块为单位 |
| c    | 设备文件，该文件是字节单位 |
| s    | socket                     |
| p    | 管道文件（待完善           |

## rwx作用到文件

| 参数 | 说明                                   |
| ---- | -------------------------------------- |
| r(4) | read 读权限                            |
| w(2) | write 写权限，可以修改，但无法删除文件 |
| x(1) | execute 执行权限，可以运行，删除文件   |

rwx可使用数字进行表示

## rwx作用到文件（待完善

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

## 改变文件权限

```
chmod u=rwx,g=rx,o=x 文件名称
```

| 参数 | 说明         |
| ---- | ------------ |
| +    | 增加权限     |
| -    | 减少权限     |
| =    | 赋予这些权限 |

| 参数 | 说明   |
| ---- | ------ |
| u    | 所有者 |
| g    | 所有组 |
| o    | 其他人 |
| a    | 所有人 |



# 用户管理

## 登录注销

### 登录（su

```
su xin *登录到名称为xin的账号
```

### 注销（logout

```
logout *注销当前用户，该命令只在级别3有效（init 3）
```

## 家目录

每个用户都拥有一个对应的家目录，当使用用户进行登录时，就会进入其对应的家目录

![image-20220619154528871](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619154528871.png)

​                                                                        用户xin的目录

## 用户的添加和删除

### 创建用户（useradd

useradd [参数]

| 参数 | 说明                                          |
| ---- | --------------------------------------------- |
| -d   | 指定用户的其实目录，不指定的话默认为/home     |
| -g   | 指定用户所属的组，可以指定多个组              |
| -G   | 指定用户的附属群组，每个群组使用【,】进行隔开 |
| -m   | 建立用户的主目录                              |
| -M   | 不要建立用户的主目录                          |
| -s   | 指定用户的shell                               |
| -u   | 指定用户的uid                                 |

由于Linux用户必须属于一个组，所以当添加用户并且没有指定组时，系统会自动生成一个与用户同名的组

```
useradd xin *添加用户名为xin的用户（同时生成一个名为xin的组
useradd -d /user/name xin *将用户名为xin的用户添加到name这个目录下
useradd -g s404 xin 将用户xin添加到名称为s404的组中
```

### 设置用户的密码（passwd

```
passwd xin *为xin修改一个密码
```

### 对用户进行删除（userdel

```
userdel -r xin *连同xin的家目录一起删除
userdel xin *之删除用户，不删除家目录
```

## 查询用户信息（id

id 用户名

![image-20220619172105584](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619172105584.png)

uid 用户的id号

gid 所在组的id号

## 切换用户（su

su 用户名

可以切换到高权限用户，比如

```
su root *切换到root用户
```

高权限用户切换到低权限用户不需要密码

返回原来的用户，输入exit

## 查看当前用户（whoami

```
whoami *可显示当前登录的用户
```

![image-20220619172517562](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619172517562.png)

## 添加用户组（groupadd

groupadd 组名

```
groupadd xin *添加一个名为xin的组
```

## 删除用户组（groupdel

groupdel 组名

```
groupdel xin *删除名为xin的组
```

## 修改用户信息（usermod

可以对已有用户信息进行修改

usermod [参数]

| 参数 | 说明                       |
| ---- | -------------------------- |
| -d   | 对用户的主目录进行修改     |
| -e   | 修改账户的有效期           |
| -g   | 修改用户的组               |
| -G   | 修改用户的附属组           |
| -l   | 修改用户的名称             |
| -L   | 锁定用户的密码，使密码无效 |
| -s   | 修改用户使用的shell        |
| -u   | 修改用户id（uid）          |
| -U   | 解除密码锁定               |

```
usermod -g 405 xin *将xin的组切换为405
```

## 配置信息相关的几个文件

### 用户配置信息

/etc/passwd

用户名：密码：用户名id：组id：家目录：shell文件

在该文档种密码由x代替，具体密码由于安全原因加密保存在了shadow中

shadow只有root用户有权限进行访问

![image-20220619180257694](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619180257694.png)

### 组配置信息

/etc/group

可以看到该组名的组id

![image-20220619180345542](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619180345542.png)

### 口令配置信息

存放密码和登录信息，是加密的

/etc/shadow

![image-20220801144833953](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220801144833953.png)

## 用户登录记录查询

### 查看当前登入主机的用户信息（who

```
who
```

![image-20220805091952821](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220805091952821.png)

### 查看登录记录（last

该记录以日志形式保存在系统中，可以篡改，只能参考

```
last
```

![image-20220805092152426](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220805092152426.png)

# 组管理

linux用户都属于一个特定的组

## 组的创建（groupadd

```
groupadd 组名
```

## 用户添加到组（useradd

```
useradd -g 组名 用户名
```

## 查看文件所有者（ls

```
ls -ahl *查看文件的所有者
```

![image-20220715154416787](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220715154416787.png)

## 修改文件的所有者（chown

```
chown 用户名 要改变所有者的文件名
```

![image-20220715154855762](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220715154855762.png)

改变了文件的所有者，但是组未发生改变

## 修改文件所在组（chgrp

```
chgrp 组名 要修改的文件
```

![image-20220715155335638](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220715155335638.png)

## 进阶修改（同时修改组和所有者）

```
chown 新的所有者：新的组 文件名 *同时修改所有者和组
-R 如果是目录，递归修改其中的所有者和所有组
```

![image-20220716010947950](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220716010947950.png)



# 文件目录类指令

## 查看当前工作路径（pwd

显示当前工作的绝对路径

![image-20220619200805542](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619200805542.png)

## 查看当前目录的所有内容（ls 

```
ls -a *查看所有文件，包括隐藏的
ls -l *以列表的形式，显示完整信息
```

![image-20220619201132407](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220619201132407.png)

## 切换目录（cd

### 绝对路径与相对路径的概念

绝对路径：完整的整一个路径，显示了从最高级目录到目标位置

相对路径：从当前的目录到目标位置（相当于不用打全，缺少了上一级

```
cd ~ *回到家目录
cd ~username *切换至某个用户的家目录
cd .. *回到上一级目录
cd - *f
```

## 创建目录（mkdir

```
mkdir /mnt/sr0 *在mnt目录创建一个新的叫做sr0的目录（只能创建一个目录
mkdir -p /mnt/sr/sr0 *相比于上一条命令，能一次性创建多级目录（一次性创建多个不存在的目录
```

## 删除目录（rmdir

```
rmdir /mnt/sr0 *删除sr0目录（无法删除存在内容的目录，即无法删除非空目录
rm -rf /mnt/sr0 *可以删除非空目录（强制删除
```

## 创建空文件（touch

```
touch hello.txt *创建一个名为hello的txt空文件
touch a.txt b.txt *可以一次性创建多个文件
```

## 拷贝（cp

```
cp a.txt /mnt/b *将当前目录里的a.txt拷贝到/mnt/b里
cp -r  test/ xin/ *拷贝目录需要加上一个-r，否则无法执行
\cp *强制覆盖不提示，否则若已存在需要输入y继续
```

## 删除文件或目录（rm

```
rm -r *递归删除整个文件夹
rm -f *强制删除不提示
rm a.txt *删除a.txt，会提示是否删除
rm -rf a/ *删除a整个目录
```

## 移动文件或重命名（mv

```
mv a.txt b.txt *将a重命名为b（前提是该目录存在a.txt）
mv a.txt /mnt/sr0 *将a移动至/mnt/sr0中，名字为a，原文件也存在
```

## 以只读的方式查看文件（cat

该命令适合查看较小的文件

```
cat -n *查看文件的内容，显示行号
cat -n 文件名称 | more *分页显示
```

## 全屏方式按页显示文本的内容（more,less

二者的区别在于：less只**加载需要显示的内容**，不是一次加载一整个文件

| 参数   | 说明                     |
| ------ | ------------------------ |
| enter  | 可以一行一行的查看       |
| ctrl+b | 查看上一页               |
| ctrl+f | 查看下一页               |
| 空格   | 查看下一页               |
| f      | 直接退出，显示跳过多少行 |
| :f     | 显示当前是该文件的第几行 |

## 输出重定向和追加（指令>和指令>>

 `>会将原本的文件内容覆盖`

`>> 不会覆盖文本的文件内容，追加到文件的尾部`

```
ls -l > a.txt *把ls -l显示的内容重定向（写入）到a.txt当中
ls >> a.txt *在a.txt中增加ls显示的内容（原本的内容还存在
echo “内容” > a.txt *将双引号中的内容写入到a.txt当中
该指令还可以和cat指令结合
```

## 输出内容到控制台（echo

![image-20220620171922604](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220620171922604.png)

```
echo $PATH *输出环境变量到控制台
```

## 显示文件的头部（head

```
head -n 5 文件名 *显示文件的前5行，默认显示前10行
```

tail（显示文件尾部）

```
tail a.txt *显示文件的尾部10行
tail -n 5 a.txt *显示文件尾部5行内容
tail -f a.txt *显示a.txt的实时所有更新，监控的作用（可用于日志监控
```

## 软连接，类似于快捷方式（ln

```
ln -s 原文件或目录 软链接名称
ln -s /mnt/sr0 guazai *在当前目录下创建一个去往/mnt/sr0的软连接
	cd guazai/ *会前往/mnt/sr0,使用pwd查询所在位置则是 /创建软连接时的目录/guazai
rm -rf guazai *删除名为guazai的软连接
注意！！删除时不能带/,否则删除整个文件夹
```

## 查看命令的输入历史（history

![image-20220620190032521](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220620190032521.png)

# 网络指南

## 修改主机名称

文件位置为/etc/hostname

直接进入该文件进行编辑

重启后配置生效

```
hostname *查看主机名称
```

## 静态IP配置（ifcfg-eth

网卡参数文件为

/etc/sysconfig/network-scripts/ifcfg-eth

![image-20220803104739214](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220803104739214.png)

​                                                   *文件内容*

### 修改BOOTPROTO

![image-20220718125453514](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220718125453514.png)

修改为静态获取IP static

### 指定IP

IPADDR=

GATEWAY=

DNS1=

### 重启网络服务

service network restart

## 查看系统网络情况（netstat

| 参数 | 说明                               |
| ---- | ---------------------------------- |
| -a   | 显示所有的socket                   |
| -n   | 直接使用IP地址                     |
| -p   | 显示正在使用socket的程序名称       |
| -l   | 显示监控中服务器的socket（显示端口 |
| -t   | 显示tcp端口                        |
| -u   | 显示udp端口                        |

socket：TCP/IP的API

```
netstat -an *按照一定顺序排列输出
netstat -p *显示哪个进程在调用
netstat -anp *查看所有网络服务
netstat -at *显示所有的tcp服务
netstat -au *显示所有的udp服务
netstat -ln *以数字的形式显示监控中的端口
netstat -plnt *以数字的形式显示监控中的tcp端口 
```

## ping

测试目标主机或域名是否可达

```
ping -f *极限测试，大量且快速的发送大量数据给目标主机，观察回应
```

![image-20220802093141388](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220802093141388.png)

```
ping —R *记录路由过程
```

![image-20220802093214659](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220802093214659.png)

## 查看当前网络接口状态（ifconfig

```
ifconfig
```

![image-20220802094537399](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220802094537399.png)

## 路由表（route

### 查看路由表

```
route -n
```

![image-20220802094719296](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220802094719296.png)

### 添加和删除路由

```
route add -net IP地址 netmask 子网掩码 gw 网关
route del -net IP地址 netmask 子网掩码 gw 网关
```

## telnet

明文的方式远程登录客户端程序

可用于确定远程端口的服务状态

```
telnet [IP] [port]
```

## 下载网络文件（wegt

下载工具

支持断点续传（将下载的文件划分为多个部分，当出现断开下载的状况时，不用从头重新下载整个文件）FTP,HTTP协议的下载

```
wegt [参数] [目标网站,软件]
```

| 参数   | 说明     |
| ------ | -------- |
| -c     | 断点续传 |
| 待完善 |          |

## 抓包（tcpdump

| 参数   | 说明                                     |
| ------ | ---------------------------------------- |
| -vv    | 输出详细的报文信息                       |
| -w     | 报文文件                                 |
| -i     | 指定监听的网络接口                       |
| -s     | 设置数据包抓取的长度，不设置话默认68字节 |
| 待完善 |                                          |

```
tcpdump -vv -w test.pcap -i eth0 -s0 *在eth0抓取数据包并保存到test.pcap文件中
```

# 防火墙管理（待完善

内核工作原理

通过netfilter框架实现

netfilter为网络协议定义了一套钩子函数，当数据包经过协议栈的关键点时会被进行调用

然后把数据包和钩子函数作为参数传递给框架

 钩子函数：在一个事件触发的时候，系统捕获了事件，然后执行一些操作

可以访问在正常情况下无法访问的消息

本质 用于处理系统消息

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

# 任务调度指南（crontab 待完善

当有些任务需要在特定时间执行时，便可使用任务调度帮助自动执行

定期执行程序，所以必须有执行的权限

```
crontab -e *执行文字编辑器设置定时表
crontab -r *删除目前的定时表
crontab -l *列出当前的定时表
```

## 五个占位符的含义

```
f1 f2 f3 f4 f5 Program
```

f1:表示分钟

f2：表示小时

f3：表示月份中的第几日

f4：表示月份

f5：表示一个星期中的第几天

program：表示执行的程序

[来源runoob]: https://www.runoob.com/linux/linux-comm-crontab.html

## 定时表中的格式

```
*/1 * * * * 运行的脚本，或者应该执行的语句
```



# 服务（service）管理

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



# 进程指南（ps，top

## 进程介绍

每执行一个程序都会有一个进程

而每一个进程都会对应一个父进程

前台 目前看得到的

后台 看不见的

## 查看进程（ps

```
ps -a *显示当前终端所有的进程信息
ps -u *以用户的格式显示进程信息
ps -x *显示后台进程运行的参数
ps -ef *查看程序的父进程
pstree *树状图查看进程
```

### 各个名称代表的含义

![image-20220720091114127](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220720091114127.png)

| 名称 | 含义                     |
| ---- | ------------------------ |
| USER | 用户名称                 |
| PID  | 进程号                   |
| PPID | 父进程编号               |
| CPU  | 进程占用CPU的百分比      |
| MEM  | 进程占用物理内存的百分比 |
| VSZ  | 进程占用的虚拟内存大小   |
| RSS  | 进程占用的物理内存大小   |
| TTY  | 终端名称的缩写           |
| STAT | 进程的状态               |

R 正在执行 S 禁止状态 Z 僵死进程 

## 终止进程（kill

将进程进行终止，支持通配符

-9为强制终止的含义

```
kill 进程PID *终止该PID的进程
killall 进程名称 *终止所有该名称的进程
kill -9 进程pid *强制终止该PID进程
```

## 任务管理器（top

类似于任务管理器

d 修改刷新的时间 或d者

```
top -d 10 *修改为10秒刷新一次
```

u 专门监控某一个用户

k 终止进程

P 按照PID进行排序

M 按照内存使用进行排序

C 按照CPU使用进行排序

# Shell指南

shell是一个命令解释器

应用-shell-内核-硬件

shell为用户提供了一个想linux内核发送请求以便运行程序的程序

用户可以使用shell启动，挂起，停止，编写一些程序

##  编写一个shell

```shell
!#/bin/bash
echo "hello,world"
```

```shell
chmod 744 文件名.sh
./文件名.sh *运行
```

##  shell变量

### 定义一个变量

```shell
A=200
echo "A=$A"
```

![image-20220722134756384](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220722134756384.png)

### 取消一个变量

```shell
A=200
echo "A=$A"
unset A
echo "A=$A"
```

![image-20220722134949104](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220722134949104.png)

## 静态变量

```shell
readonly A=200
echo "A=$A"
unset A
echo "A=$A"
```

![image-20220722135348240](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220722135348240.png)

无法unset静态变量

## 全局环境变量

可供其他shell使用

### 在/etc/profile 文件中定义一个环境变量

```
vim /etc/profile
环境变量名称=位置
export 环境变量名称
：wq
```

### 让环境变量生效

```
source /etc/profile
```

### 接着便可以在shell脚本中使用该环境变量

## 将命令的返回值赋给变量

```
A=`命令` *使用反引号
A=$(命令) *同理于反引号
```

```shell
A=`ls -l /root`
echo $A
B=$(date)
echo $B
```

<img src="https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220722140440717.png" alt="image-20220722140440717" style="zoom:200%;" />

## 位置参数变量（相当于input

希望获取到命令行的参数信息，便可以使用到位置参数变量

```shell
$n *n未数字，代表获取命令行的第几个数字，如果获取到数字是两位数，则需要${10} $0代表命令本身
$# *显示参数的个数
$* *这个命令行终端所有参数，但$*把所有参数看作一个整体
$@ *显示命令行的所有参数，但是每一个参数区分对待
```

```shell
echo "$1 $2"
echo ""
echo "$*"
echo ""
echo "$@"
```

![image-20220726171458293](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220726171458293.png)

## 预定义变量

```shell
$$ 显示当前进程号的PID
$! 后台运行的最后一个进程的进程号
$? 最后一次执行命令的状态 0成功 非零可能失败
```

![image-20220725092113590](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220725092113590.png)

![image-20220725092130988](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220725092130988.png)

## 运算符

```shell
$((运算内容))
```

```shell
`expr m + n` *运算符中间需要空格隔开
```

```shell
$[运算内容]
```

## 条件判断

```shell
------------字符串比较
-lt *小于
-le *小于等于
-eq *等于
-ge *大于等于
-ne *不等于
if [ "ok -eq ok" ]
then
	echo "1"
fi
------------按照文件权限进行判断
-r *读的权限
-w *写的权限
-x *执行的权限
------------按照文件类型进行判断
-f *文件存在并且是一个常规的文件
-e *该文件存在
-d *文件存在并是一个目录
if [ -e /root/a.txt ]
then
	echo '1'
fi
```

```shell
[ 条件 ] *中括号里放置条件，在条件前后需要使用空格进行隔开
```

## 流程控制

### if

--------

```shell
A=100
if [ $A -ge 60 ]
then
	echo "及格"
elif [ $A -lt 60 ]
then
	echo "不及格"
fi
```

### case

-------------

```shell
case $1 in
"1")  *如果输入为1，执行下面的语句
echo "monday"
"2")  *如果输入为2，执行下面的语句，以此类推
echo "thusday"
*)    *如果都不是，执行下面的语句
echo "other"
esac
```

### for

--------

有两种格式

#### 第一种

```shell
sum=0
for((i=1;i<=100;i++))
do
	sum=$[$sum+$i]
done
echo "sum=$sum"
```

![image-20220726173853033](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220726173853033.png)

#### 第二种

```shell
for i in "$*"
do
	echo "$*" *由于@是当成一个参数读出，所以指挥输出一次。可对比$@
done
```

### while

--------

```shell
while [ 判断语句 ]
do
	循环执行
done
```

### read

--------

可以用于读取控制台的输入（从控制台输入到程序中

```shell
read -p -t 时间秒数 "提示" 变量名称
```

```SHELL
read -p "输入A的值" A
echo "A的值为$A"
read -t 5-p "五秒内输入B的值" B
echo "B的值为$B"
```

![image-20220728101557433](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220728101557433.png)

## 函数

### 系统函数

### basename

------

返回最后的文件名

![image-20220728102123639](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220728102123639.png)

### dirname

-------

返回路径

![image-20220728102156607](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220728102156607.png)

### 自定义函数

```shell
function 函数名(){
	内容
}
函数名 变量
```

```shell
function getsum(){
	SUM=$[$n1+$n2]
	echo "SUM=$SUM"
}
read -p "n1:" n1
read -p "n2:" n2
getsum $n1 $n2
```



