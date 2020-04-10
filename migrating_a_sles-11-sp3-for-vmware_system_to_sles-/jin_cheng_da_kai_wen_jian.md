## 进程打开文件

1 、查看进程&quot;打开&quot;的文件：

 1）pidof programe-name(获得想了解的进程(programe-name)的PID)

或ps -aux|grep programe-name(获得想了解的进程(programe-name)的PID)

 找出进程的PID

 2）cd /proc/$PID/fd（会看见文件描述符）

 3）ls -l

    得到文件描述符指向的实际文件,即当前进程打开的文件

2、查看进程&quot;打开&quot;的文件2：

1）获得想了解的进程的PID方法同上

2）lsof -c programe-name

    或lsof -p $PID

3、查看文件对应的进程：

lsof file-name

4、lsof命令用法：

lsof -c abc 显示abc进程现在打开的文件

 lsof abc 显示开启文件abc的进程

 lsof -i :22 显示22端口现在运行什么程序

 lsof -g gid 显示归属gid的进程情况

 lsof +d /usr/local/ 显示目录下被进程开启的文件

 lsof +D /usr/local/ 同上，但是会搜索目录下的目录，时间较长

 lsof -d 4 显示使用fd为4的进程

 lsof -i 用以显示符合条件的进程情况

 lsof -s 列出打开文件的大小，如果没有大小，则留下空白

 lsof -u username 以UID，列出打开的文件

5、查看网络状态：

lsof -Pnl +M -i4 显示ipv4服务及监听端情况

netstat -anp 所有监听端口及对应的进程

netstat -tlnp 功能同上

一、 glance 运行glance, 逐一选择某进程，查看该进程打开的文件（open file）. 二、 去网站 [http://hpux.cs.utah.edu/](http://hpux.cs.utah.edu/) 下载lsof软件，根据操作系统平台，安装该软件。然后通

过下列script，查看进程打开的文件情况。# ps -ef | awk {print $2} &gt; /tmp/list # cat /tmp/list | while read pid &gt;do &gt;echo $pid &gt;&gt; /tmp/lsof.out &gt;lsof -p $pid &gt;&gt; /tmp/lsof.out &gt;done # cat /tmp/lsof.out | more