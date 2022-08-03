# linux的启动以及运行级别

## 启动过程

#### 1

开机自检

#### 2

从MBR中读取GRUB(引导程序)

#### 3

GRUB根据配置文件显示引导菜单

#### 4

如果选择linux，则加载linux内核文件

#### 5

当内核载入内存后，GRUB任务完成。CPU开始执行linux内核代码，建立内核运行环境

#### 6

内核代码执行完后，开始执行linux的第一个进程——systemd，进程号是1

![image-20220801091659482](https://propran-img.oss-cn-hangzhou.aliyuncs.com/img/image-20220801091659482.png)

#### 7

systemd启动后，读取/etc/systemd/system/default.target(该文件设置系统的运行级别)

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
ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.targ *以GUId
```

```
init 5 *临时进入gui界面
```

