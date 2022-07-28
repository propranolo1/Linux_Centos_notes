# GUI的安装

## 安装gnome包

```
yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
```

## 更新系统运行的级别

```
ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.targ
```

该命令为永久更新

```
init 5
```

该命令为临时进入gui界面