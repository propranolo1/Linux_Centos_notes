# Shell指南

shell是一个命令解释器

应用-shell-内核-硬件

shell为用户提供了一个想linux内核发送请求以便运行程序的程序

用户可以使用shell启动，挂起，停止，编写一些程序

shell分为两种

1. 交互式

   用户每输入一条命令就执行

2. 批处理

   执行一个shell脚本

   

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

### 转义符

当你想要输出的内容中包含shell的已经被定义的特殊符号时，为了避免输出的内容被干扰，需要使用转义符把字符去特殊化

| 参数  | 说明                             |
| ----- | -------------------------------- |
| \     | 使\后的一个变量变为单纯的字符串  |
| ' '   | 把其中所有的变量转为字符串       |
| " "   | 保留其中变量的属性，不做转移处理 |
| \` \` | 把其中的命令执行后返回结果       |



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

### 查看所有变量

```shell
env
```

### 设置一个全局环境变量

```
export 变量名称
```

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

