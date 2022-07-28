# 任务调度指南（crontab）

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

