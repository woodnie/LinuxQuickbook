### top {#top}

top

AME

      top - display Linux tasks

交互式进程管理工具:

[root@node ~]# gnome-system-monitor     #图形化工具

[root@node ~]# /usr/X11R6/bin/xosview

[root@node ~]# /usr/X11R6/bin/xload

[root@node ~]# kpm #KDE平台top命令

[root@node ~]# top                                       #命令行工具

[root@node ~]#

?完整列表

q退出

P   #按照cpu使用排序

M  #按照MEM使用排序

N  #按照PID排序

u #查看指定用户

k #kill -15 信号kill进程

1、top -H

手册中说：-H : Threads toggle

加上这个选项启动top，top一行显示一个线程。否则，它一行显示一个进程。

2、ps xH

手册中说：H Show threads as if they were processes

这样可以查看所有存在的线程。

3、ps -mp &lt;PID&gt;

手册中说：m Show threads after processes

这样可以查看一个进程起的线程数。

更多详尽的解释还可以man ps，man top。