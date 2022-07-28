# yum指南

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

![image-20220619094404355](C:\Users\JInXiN\Desktop\Centos指南\image-20220619094404355.png)

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

## 查看安装包是否在服务器上

```
yum list | grep 要查询的安装包
```

