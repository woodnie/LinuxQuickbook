### vmstat {#vmstat}

vmstat - report virtual memory statistics

用法

vmstat [-a] [-n] [-S unit] [delay [ count]

vmstat [-s] [-n] [-S unit]

vmstat [-m] [-n] [delay [ count]

vmstat [-d] [-n] [delay [ count]

vmstat [-p disk partition] [-n] [delay [ count]

vmstat [-f]

vmstat [-V]

-a：显示活跃和非活跃内存

-f：显示从系统启动至今的fork数量  引申閱讀： [http://www.cnblogs.com/leoo2sk/archive/2009/12/11/talk-about-fork-in-linux.html](http://www.cnblogs.com/leoo2sk/archive/2009/12/11/talk-about-fork-in-linux.html)

-m：显示slabinfo

-n：只在开始时显示一次各字段名称。

-s：显示内存相关统计信息及多种系统活动数量。

delay：刷新时间间隔。如果不指定，只显示一条结果。

count：刷新次数。如果不指定刷新次数，但指定了刷新时间间隔，这时刷新次数为无穷。

-d：显示磁盘相关统计信息。

-p：显示指定磁盘分区统计信息

-S：使用指定单位显示。参数有 k 、K 、m 、M ，分别代表1000、1024、1000000、1048576字节（byte）。默认单位为K（1024 bytes）

-V：显示vmstat版本信息。

[root@node ~]# vmstat 5 #每隔5秒获得一个内存快照

procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------

r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st

0  0      0 251672  12736 201404    0    0    17     3 1006   24  0  0 100  0  0

0  0      0 251688  12736 201404    0    0     0     0 1008   26  0  0 100  0  0

0  0      0 251688  12736 201404    0    0     0     0 1007   22  0  0 100  0  0

0  0      0 251688  12744 201404    0    0     0    14 1008   26  0  0 100  0  0

字段说明：

procs（进程）：

r: 运行队列中进程数量

b: 等待IO的进程数量

memory（内存）：

swpd: 使用虚拟内存大小

free: 可用内存大小

buff: 用作缓冲的内存大小

cache: 用作缓存的内存大小

swap：

si: 每秒从交换区写到内存的大小

so: 每秒写入交换区的内存大小

IO：（现在的Linux版本块的大小为1024bytes）

bi: 每秒读取的块数

bo: 每秒写入的块数

system：

in: 每秒中断数，包括时钟中断。

cs: 每秒上下文切换数。

CPU（以百分比表示）：

us: 用户进程执行时间(user time)

sy: 系统进程执行时间(system time)

id: 空闲时间(包括IO等待时间)

wa: 等待IO时间

如果 r经常大于 4 ，且id经常少于40，表示cpu的负荷很重。

如果pi，po 长期不等于0，表示内存不足。

如果disk 经常不等于0， 且在 b中的队列 大于3， 表示 io性能不好。

[root@node ~]# vmstat  -a 2

procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------

r  b   swpd   free  inact active   si   so    bi    bo   in   cs us sy id wa st

0  0      0 251672 117304 107832    0    0    17     3 1006   24  0  0 100  0  0

0  0      0 251672 117304 107832    0    0     0     0 1006   22  0  0 100  0  0

1  0      0 251672 117304 107832    0    0     0     0 1006   20  0  0 100  0  0

0  0      0 251672 117304 107832    0    0     0    14 1006   23  0  0 100  0  0

0  0      0 251672 117304 107832    0    0     0     0 1007   23  0  0 100  0  0

0  0      0 251672 117304 107832    0    0     0     0 1007   20  0  0 100  0  0

使用-a选项显示活跃和非活跃内存时，所显示的内容除增加inact和active外，其他显示内容同上。

字段说明：

Memory（内存）：

inact: 非活跃内存大小（当使用-a选项时显示）

active: 活跃的内存大小（当使用-a选项时显示）