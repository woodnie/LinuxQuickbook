### kill {#kill}

kill

[root@localhost ~]# kill -l        ## 查看kill命令的信号

其中：

SIG  1   在不重启服务的情况下，重新加载配置文件

SIG  9   强制结束进程（即 ctrl+c）

SIG  15 默认

SIG  18把进程调入后台，继续运行

SIG  19把进程调入后台，停止运行（即 ctrl+z）

[root@localhost ~]# kill             PID     ==  

[root@localhost ~]# kill    -15   PID

[root@localhost ~]# kill    -9     PID                      ##强制结束进程

[root@localhost ~]# kill   -18    %后台进程ID      ##把进程调入后台，继续运行

[root@localhost ~]# kill   -17    %后台进程ID      ##把进程调入后台，停止运行（即 ctrl+z）

[root@localhost ~]# kill            %后台进程ID      ##结束后台进程

[root@localhost ~]# killall        服务名称              ##强制结束该服务所有相关进程  

[root@localhost ~]# fg            %后台进程ID        ##把进程调到前台foreground  

[root@localhost ~]# bg           %后台进程ID        ##把进程调到后台background,  继续运行

以下命令结果相同，都是给PID 3428进程发送默认的TERM信号：

kill -3428

kill -15 3428

kill -SIGTERM 3428

kill -TERM 3428

++++++++++++++++++++++++++++++++++++++++++++++

signal信号列表名称          默认动作                  说明

SIGHUP                             终止进程                  终端线路挂断

SIGINT                               终止进程                  中断进程

SIGQUIT                            建立CORE文件终止进程，并且生成core文件

SIGILL                               建立CORE文件         非法指令

SIGTRAP                          建立CORE文件         跟踪自陷

SIGBUS                            建立CORE文件         总线错误

SIGSEGV                         建立CORE文件          段非法错误

SIGFPE                            建立CORE文件          浮点异常

SIGIOT                              建立CORE文件          执行I/O自陷

SIGKILL                            终止进程    杀死进程

SIGPIPE                           终止进程    向一个没有读进程的管道写数据

SIGALARM                      终止进程    计时器到时

SIGTERM                         终止进程    软件终止信号

SIGSTOP                         停止进程    非终端来的停止信号

SIGTSTP                          停止进程    终端来的停止信号

SIGCONT                         忽略信号    继续执行一个停止的进程

SIGURG                           忽略信号    I/O紧急信号

SIGIO                                忽略信号    描述符上可以进行I/O

SIGCHLD                         忽略信号    当子进程停止或退出时通知父进程

SIGTTOU                          停止进程    后台进程写终端

SIGTTIN                            停止进程    后台进程读终端

SIGXGPU                         终止进程    CPU时限超时

SIGXFSZ                          终止进程    文件长度过长

SIGWINCH                       忽略信号    窗口大小发生变化

SIGPROF                         终止进程    统计分布图用计时器到时

SIGUSR1                         终止进程    用户定义信号1

SIGUSR2                         终止进程    用户定义信号2

SIGVTALRM                    终止进程    虚拟计时器到时