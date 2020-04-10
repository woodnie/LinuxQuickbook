### uptime {#uptime}

uptime

NAME

      uptime - Tell how long the system has been running.

[root@node ~]# uptime   # 系统持续运行时间

10:19:04 up 257 days, 18:56,  12 users,  load average: 2.10, 2.10,2.09

显示内容说明：

10:19:04                      #系统当前时间

up 257 days, 18:56       #主机已运行时间,时间越大，说明你的机器越稳定。

12 user                        #用户连接数，是总连接数而不是用户数

load average                 #系统平均负载，统计最近1，5，15分钟的系统平均负载

系统平均负载:是指在特定时间间隔内运行队列中的平均进程数。

如果每个CPU内核的当前活动进程数不大于3的话，那么系统的性能是良好的。如果每个CPU内核的任务数大于5，那么这台机器的性能有严重问题。

如果你的linux主机是1个双核CPU的话，当Load Average 为6的时候说明机器已经被充分使用了。