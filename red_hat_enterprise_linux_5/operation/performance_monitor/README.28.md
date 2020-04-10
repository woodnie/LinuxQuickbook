### performance monitor {#performance-monitor}

监控进程和资源

/proc提供的可用信息有时候很难理解

有些界面可格式化这些数据，并使之更容易访问

Memory: free, vmstat, swapon -s, pmap

Processes: ps, top, gnome-system-monitor

Kernel state: uname, uptime, tload

sysstat:This package provides the sar and iostat commands.

iostat  -  Report  Central  Processing Unit (CPU) statistics and input/output statistics for devices and partitions

sar - Collect, report, or save system activity information

mpstat - Report processors related statistics

sa -  summarizes accounting information

Linux性能监控的常用工具有:                                              

dstat monitoring statistics aggregator

netstat provides network statistics    

iptraf traffic monitoring dashboard            

ethtool reports on Ethernet interface configuration

1.查看cpu使用情况

CPU的利用率很大程度上决定于什么样的资源在使用它。内核有一个调度器专门负责两种资源: (单或多)线程和中断。调度器给不同的资源指定不同的优先

级。下面的列表按优先级从高到低的顺序排列,

 中断--设备通知内核完成了处理,例如网卡传送一个数据包或者硬盘完成一次I/O请求。

 内核进程--所有的内核进程都具有这个等级的优先级。

 用户进程--这块通常称为&quot;用户区&quot;。所有的应用软件有在用户块执行。在内核调度机制中这块具有最低的优先级。

为了了解内核如何管理这些资源,需要介绍一些关键的概念。下面的章节介绍了上下文切换、运行队列和CPU利用率。

1.1 上下文切换

大多数现代的处理器每次只能执行一个进程或线程。n路的超线程处理器同时可以运行n个线程。但是Linux内核仍旧把双核中每个处理器视为独立的。例如, 对于系统中双核处理器, Linux内核报道为两个独立的处理器。

标准的Linux内核可以运行50-5000个线程。对于单个CPU,内核必须调度和平衡这些线程。每个线程都被分配了一定的处理器时间单元。当线程完成了所分配的时间单元或者被一些更高优先级的线程或中断(如硬件中断)抢断时,更高优先级的线程送到处理器上执行而这个线程就被送回到运行队列。这种线程的切换就称为上下文切换。

内核每执行一次上下文切换,额外的资源用来将线程从CPU寄存器中移动到运行队列中。系统中越多的上下文切换,意味着内核需要做更多额外的工作来管理线程调度。

1.2 运行队列

每个CPU都维护着一个线程的运行队列。在理想状态下,每个调度器都在运行线程。线程的状态是休眠(被抢断或者等待I/O)或者运行。如果CPU高度负载, 调度器有可能无法及时响应系统需求。结果导致可运行的线程占满了运行队列,运行队列越庞大,线程执行所需的等待时间就越长。

一个常用的词汇&quot;负载&quot;就用来描述运行队列的状况。系统的负载就是目前正在执行的线程数量和运行队列中线程数量的总和。比如在一个双核的系统上,2个线程正在执行,4个线程在运行队列中,那么系统负载就是6。top命令显示的是1、10、15分钟内系统的平均负载。

1.3 CPU利用率

CPU利用率定义为CPU使用的百分比,是衡量系统的一个重要指标。大多数的性能监测工具把CPU利用率分为以下几类

用户时间--CPU执行用户区的线程所花费的时间百分比。

系统时间--CPU执行内核线程和中断所花费的时间百分比。

IO等待时间--所有线程被阻塞,等待I/O请求的完成。此时CPU处于空闲状态的时间百分比。

空闲时间--CPU处于完全空闲状态的时间百分比。

1.4 CPU性能监控

确定CPU运行的性能有多好涉及到理解运行队列、CPU利用率和上下文切换。前面已经提到,性能都是相对于具体的统计基准而言的。但是在任何系统上,仍有一些通用的系能预期,这包括

 运行队列--每个处理器的运行队列不应多于1-3个。例如一个双核处理器的运行队列不应当多于6个进程。

 CPU利用率--如果CPU充分利用,应该达到一下的平衡比例

     - 65-70%的用户时间

     - 30-35%的系统时间

     - 0-5%的空闲时间

 上下文切换--上下文切换的数量直接决定了CPU的利用率。如果CPU利用率保持在上述的平衡比列,那么大量的上下文切换是正常的

[root@node ~]# top  #display Linux tasks

2.

2.1

[root@node ~]# cat /proc/stat  

cpu  1380 6544 9893 1316101 11040 16 127 0

cpu0 1380 6544 9893 1316101 11040 16 127 0

intr 13628578 13453082 15 0 3 3 0 5 0 0 0 0 0 4588 0 0 77754 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 61494 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 31634 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0

ctxt 1860835

btime 1299407158

processes 67630

procs_running 1

procs_blocked 0    

cat /proc/stat命令是包含内核统计量，提供以下数据：

   CPU 以及CPU0、每行的每个参数意思（以第一行为例）为：

   user (432661) 从系统启动开始累计到当前时刻，用户态的CPU时间（单位：jiffies） ，不包含 nice值为负进程。1 jiffies=0.01秒

   nice (13295) 从系统启动开始累计到当前时刻，nice值为负的进程所占用的CPU时间（单位：jiffies）

   system (86656) 从系统启动开始累计到当前时刻，核心时间（单位：jiffies）

   idle (422145968) 从系统启动开始累计到当前时刻，除硬盘IO等待时间以外其它等待时间（单位：jiffies）

   iowait (171474) 从系统启动开始累计到当前时刻，硬盘IO等待时间（单位：jiffies） ，

   irq (233) 从系统启动开始累计到当前时刻，硬中断时间（单位：jiffies）

   softirq (5346) 从系统启动开始累计到当前时刻，软中断时间（单位：jiffies）

   CPU时间=user+system+nice+idle+iowait+irq+softirq

   &quot;intr&quot;这行给出中断的信息，第一个为自系统启动以来，发生的所有的中断的次数；然后每个数对应一个特定的中断自系统启动以来所发生的次数。

   &quot;ctxt&quot;给出了自系统启动以来CPU发生的上下文交换的次数。

   &quot;btime&quot;给出了从系统启动到现在为止的时间，单位为秒。

   &quot;processes (total_forks) 自系统启动以来所创建的任务的个数目。

   &quot;procs_running&quot;：当前运行队列的任务的数目。

     &quot;procs_blocked&quot;：当前被阻塞的任务的数目。

2.2

[root@node ~]# mpstat   #不带参数时，输出为从系统启动以来的平均值

2.3

[root@node ~]# sar

　　若 %iowait 的值过高，表示硬盘存在I/O瓶颈

　　若 %idle 的值高但系统响应慢时，有多是 CPU 等待分配内存，此时应加大内存容量

　　若 %idle 的值连续低于 10，则系统的 CPU 处理能力相对于较低，表明系统中最需要解决的资源是 CPU。

2.查看内存情况

[root@node ~]# free

            total       used       free     shared    buffers     cached

Mem:        515308     361920     153388          0      50284     213568

-/+ buffers/cache:      98068     417240

Swap:      2097144          0    2097144

total 内存总数

used 已经使用的内存

free 空闲的内存数

shared 多个进程共享的内存总额

buffers Buffer Cache和cached Page Cache 磁盘缓存的大小

-buffers/cache (已用)的内存数:used - buffers - cached

+buffers/cache(可用)的内存数:free + buffers + cached

可用的memory=free memory+buffers+cached

[root@node ~]# vmstat    #Report virtual memory statistics

e.g.:

[root@node ~]# vmstat

procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------

r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st

0  0      0  43080  56916 230392    0    0    36     9 1011  116  0  0 99  0  0

[root@node ~]# vmstat  2 5

procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------

r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st

0  0      0  43080  56980 230416    0    0    35     9 1011  116  0  0 99  0  0

0  0      0  43080  56980 230416    0    0     0     0  522   60  0  0 100  0  0

0  0      0  43080  56980 230416    0    0     0     0  523   63  0  0 100  0  0

0  0      0  43080  56988 230408    0    0     0    18  525   61  0  0 100  0  0

0  0      0  43080  56988 230416    0    0     0     0  523   58  0  0 100  0  0

procs:

r 在internal时间段里，运行队列里等待CPU的任务（任务）的个数，即不包含vmstat进程 procs_running-1

b 在internal时间段里，被资源阻塞的任务数（I/0，页面调度，等等.） ，通常情况下是接近0的 procs_blocked

system:

in 在internal时间段里，每秒发生中断的次数 ?intr/interval

cs 在internal时间段里，每秒上下文切换的次数，即每秒内核任务交换的次数 ?ctxt/interval

cpu:

us 在internal时间段里，用户态的CPU时间(%)，包含 nice值为负进程 (?user+?nice)/?total*100

sy 在internal时间段里，核心态的CPU时间(%) (?system+?irq+?softirq)/?total*100

id 在internal时间段里，cpu空闲的时间，不包括等待i/o的时间(%) ?idle/?total*100

wa 在internal时间段里，等待i/o的时间(%) ?iowait/?total*100

3.查看磁盘使用情况

[root@node ~]# fdisk -l            #查看硬盘分区情况

[root@node ~]# df -h               #查看当前硬盘使用情况

[root@node ~]# iostat              #查看硬盘性能

4.查看系统负载

1.uptime

[root@node ~]# uptime   #Tell how long the system has been running.

10:19:04 up 257 days, 18:56,  12 users,  load average: 2.10, 2.10,2.09

[root@node ~]#crontab -e   #结合crontab监控系统负载

0-59/10 * * * * * uptime &gt;&gt;/var/log/self/uptime

2 .cat /proc/loadavg

[root@localhost ~]# cat /proc/loadavg

4.61 4.36 4.15 9/84 5662

每个值的含义为：

参数 解释

lavg_1 (4.61) 1-分钟平均负载

lavg_5 (4.36) 5-分钟平均负载

lavg_15(4.15) 15-分钟平均负载

nr_running (9) 在采样时刻，运行队列的任务的数目，与/proc/stat的procs_running表示相同意思

nr_threads (84) 在采样时刻，系统中活跃的任务的个数（不包括运行已经结束的任务）

last_pid(5662) 最大的pid值，包括轻量级进程，即线程。

假设当前有两个CPU，则每个CPU的当前任务数为4.61/2=2.31

5.网络

查看网络信息

[root@node ~]# ifconfig

[root@node ~]# mii-tool      #查看active的网口（即连接了网线）

[root@node ~]# ip              #ip - show / manipulate routing, devices, policy routing and tunnels

[root@node ~]# route         #route - show / manipulate the IP routing table

[root@node ~]# netstat -n   #查看网络连接状态

[root@node ~]# tcpdump    #捕获一个SMTP会话

[root@node ~]# lsof           #lsof可以列出被进程所打开的文件的信息

[root@node ~]# fuser         #fuser - identify processes using files or sockets

查看网卡流量

[root@node ~]# iptraf

[root@node ~]# iftop

[root@node ~]# noload

[root@node ~]# ifstat

[root@node ~]# sar -n

[root@node ~]# iftop

[root@node ~]# mtr

[root@node ~]# watch ifconfig

[root@node ~]# watch more /proc/net/dev