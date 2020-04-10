### manage process {#manage-process}

1\. 进程查看命令

[root@node ~]# ps

[root@node ~]# pgrep

[root@node ~]# pidof

2.给进程发送信号

[root@node ~]# kill     [信号] pid...

[root@node ~]# killall [信号] comm ...

[root@node ~]# pkill  [-信号] 模式

3.调度优先级

nice value 范围：-20 - 19

nice value越小越优先

[root@node ~]# ps -o comm,nice #查看进程优先级

[root@node ~]# nice  -n 5  命令     #通过命令名，把某个命令的优先级设为为5

[root@node ~]# nice  5  PID           #通过PID，     把某个命令的优先级设为为5

[root@node ~]# renice  -5  -p  PID  #把某个命令的优先级设为为5

[root@node ~]# nice      #仅限root用户使用

[root@node ~]# renice   #所有用户都能使用

4.交互式进程管理工具

[root@node ~]# top                                       #命令行工具

[root@node ~]# gnome-system-monitor     #图形化工具

5.进程退出状态

每一个进程完成后都会返回一个退出状态

[root@node ~]# echo $?    #查询最近一个进程的退出状态

0表示成功

1-255表示失败(非0)

[root@node ~]# ping -c3  -W1 127.0.0.1 &amp;&gt; /dev/null

[root@node ~]# echo $?

0     #退出状态为0表示ping成功                  

[root@node ~]# ping -c3  -W1 1.1.1.1 &amp;&gt; /dev/null

[root@node ~]# echo $?

1    #退出状态非0表示ping不成功

[root@node ~]# exit [num]  #设置退出状态代码，一般用于shell脚本

6.根据退出状态，命令可以有条件的运行

&amp;&amp; 代表条件的 ADN THEN

||     代表条件的 OR ELSE

当两条命令被&amp;&amp;分割：当第一条命令退出状态为0时，才会执行第二命令

当两条命令被  ||  分割：当第一条命令退出状态非0时，才会执行第二命令

e.g.：

[root@node ~]# grep -q root /etc/passwd &amp;&amp; echo user is  exist                            

user is  exist

[root@node ~]# grep -q none /etc/passwd || echo user not exist  

user not exist

8.test命令,评估布尔声明

see test

9.文件测试使用[ ]或者test

[ -f file ] &amp;&amp; echo &quot;&quot;

test -x file  #测试某文件是否存在

-d文件 如果文件是目录则为真。

-e文件 如果文件存在则为真。

-f文件 如果文件存在并是常规文件则为真。

-h文件 如果文件是符号链接则为真。

-l文件 如果文件是符号链接则为真。

-r文件 如果文件存在并可以被你读取则为真。

-s文件 如果文件存在并且不是空文件则为真。

-w文件 如果文件存在并且可以被你写入则为真。

-x文件 如果文件存在并且可以被你执行则为真。

-o文件 如果文件属于你则为真。

-G文件 如果文件属于你所在的组群则为真

++++++++++++++++++++++++++++++++++++++++++++++++++

常用SIG：

SIG  1   在不重启服务的情况下，重新加载配置文件

SIG  9   强制结束进程（即 ctrl+c）

SIG  15 默认

SIG  18把进程调入后台，继续运行

SIG  19把进程调入后台，停止运行（即 ctrl+z）

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

signal信号列表名称          默认动作                                         说明

SIGHUP              终止进程                  终端线路挂断

SIGINT              终止进程                  中断进程

SIGQUIT             建立CORE文件终止进程，并且生成core文件

SIGILL              建立CORE文件              非法指令

SIGTRAP             建立CORE文件              跟踪自陷

SIGBUS              建立CORE文件              总线错误

SIGSEGV             建立CORE文件              段非法错误

SIGFPE              建立CORE文件          浮点异常

SIGIOT              建立CORE文件          执行I/O自陷

SIGKILL             终止进程        杀死进程

SIGPIPE             终止进程        向一个没有读进程的管道写数据

SIGALARM            终止进程        计时器到时

SIGTERM             终止进程        软件终止信号

SIGSTOP             停止进程        非终端来的停止信号

SIGTSTP             停止进程        终端来的停止信号

SIGCONT             忽略信号        继续执行一个停止的进程

SIGURG              忽略信号        I/O紧急信号

SIGIO               忽略信号        描述符上可以进行I/O

SIGCHLD             忽略信号        当子进程停止或退出时通知父进程

SIGTTOU             停止进程        后台进程写终端

SIGTTIN             停止进程        后台进程读终端

SIGXGPU             终止进程        CPU时限超时

SIGXFSZ             终止进程        文件长度过长

SIGWINCH            忽略信号        窗口大小发生变化

SIGPROF             终止进程        统计分布图用计时器到时

SIGUSR1             终止进程        用户定义信号1

SIGUSR2             终止进程        用户定义信号2

SIGVTALRM           终止进程        虚拟计时器到时