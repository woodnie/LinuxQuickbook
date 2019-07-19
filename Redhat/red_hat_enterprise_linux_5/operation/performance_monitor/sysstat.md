#### sysstat {#sysstat}

#yum install sysstat

　sysstat包安装完后 就会有iostat、mpstat、sar、sa的功能

      mpstat - Report processors related statistics      

      sa -  summarizes accounting information      

      sar - Collect, report, or save system activity information.

      iostat  -  Report  Central  Processing Unit (CPU) statistics and input/output statistics for devices and partitions

开始工作sysstat

/etc/init.d/sysstat start设置sysstat自开始工作

　　mpstat是Multiprocessor Statistics的缩写，是实时系统监控工具。其报告与CPU的一些统计信息，这些信息存放在/proc/stat文件中。在多CPUs系统里，其不但能查看所有CPU的平均状况信息，而且可以或者许查看特定CPU的信息。下面只介绍 mpstat与CPU相关的参量，mpstat的语法如下：

　　mpstat [-P {|ALL}] [internal [count]]

　　mpstat -P ALL 2 3

　　参量的含义如下：

　　参量 解释

　　-P {|ALL} 表示监控哪一个CPU， cpu在[0,cpu个数-1]中取值

　　internal 相邻的两次采样的间隔时间

　　count 采样的次数，count只能和delay一起使用

　　当没有参量时，mpstat则显示系统开始工作以后所有信息的平均值。有interval时，第 一行的信息自系统开始工作以来的平均信息。从第二行开始，输出为前一个interval时间段的平均信息。与CPU关于的输出的含义如下：

　　参量 解释从/proc/stat获得数据

　　CPU 处理器ID

　   user 在internal时间段里，用户态的CPU时间（%） ，不包含 nice值为负 进程 usr/total*100

     nice 在internal时间段里，nice值为负进程的CPU时间（%） nice/total*100

　　system 在internal时间段里，核心时间（%） system/total*100

　　iowait 在internal时间段里，硬盘IO等待时间（%） iowait/total*100

　　irq 在internal时间段里，软中断时间（%） irq/total*100

　　soft 在internal时间段里，软中断时间（%） softirq/total*100

　　idle 在internal时间段里，CPU祛除等待磁盘IO操作外的由于任何缘故原由而余暇的时间闲置时间（%） idle/total*100

　　intr/s 在internal时间段里，每秒CPU接收的中断的次数 intr/total*100

　　CPU总的工作时间=total_cur=user+system+nice+idle+iowait+irq+softir q

　　total_pre=pre_user+ pre_system+ pre_nice+ pre_idle+ pre_iowait+ pre_irq+ pre_softirq

　　user=user_cur - user_pre

　　total=total_cur-total_pre

　　其中_cur 表示时下值，_pre表示interval时间前的值。上表中的所有值可取到两位小数点。

　　cat /proc/stat

　　&quot;ctxt&quot;给出了自系统开始工作以来CPU发生的上下文交换的次数。

　　&quot;btime&quot;给出了从系统开始工作到现在截止的时间，单位为秒。

　　&quot;processes (total_forks) 自系统开始工作以来所创立的任务的个数目。

　　&quot;procs_running&quot;：时下运行队列的任务的数目。

　　&quot;procs_blocked&quot;：时下被阻塞的任务的数目。

============================

　　sar的最后两个参量一般是interval count

　　1、sar -u 1 5 #   每一秒钟取一次值，取五次

  　输出CPU使用环境的统计信息，每秒输出一次，一共输出100次

　　17时06分01秒 CPU %user %nice %system %iowait %idle

　　17时06分02秒 all 1.27 0.00 0.51 1.01 97.22

　　17时06分03秒 all 0.00 0.00 0.00 0.00 100.00

　　17时06分04秒 all 0.00 0.00 0.00 0.00 100.00

　　17时06分05秒 all 0.25 0.00 0.00 0.00 99.75

　　17时06分06秒 all 0.00 0.00 0.00 0.51 99.49

　　Average: all 0.30 0.00 0.10 0.30 99.29

　　CPU all 表示统计信息为所有 CPU 的平均值。

　　%user 显示在用户级别(application)运行使用 CPU 总时间的百分比。

　　%nice 显示在用户级别，用于nice操作，所占用 CPU 总时间的百分比。

　　%system 在核心级别(kernel)运行所使用 CPU 总时间的百分比。

　　%iowait 显示用于等待I/O操作占用 CPU 总时间的百分比。

　　%steal 管理程序(hypervisor)为另一个虚拟进程提供服务而等待虚拟 CPU 的百分比。

　　%idle 显示 CPU 余暇时间占用 CPU 总时间的百分比。

　　2、sar -b 1 5

　　显示I/O和传送速率的统计信息

　　17时09分07秒 tps rtps wtps bread/s bwrtn/s

　　17时09分08秒 3.12 3.12 0.00 25.00 0.00

　　17时09分09秒 89.58 6.25 83.33 141.67 733.33

　　17时09分10秒 42.71 9.38 33.33 141.67 600.00

　　17时09分11秒 2.11 2.11 0.00 16.84 0.00

　　17时09分12秒 1.04 0.00 1.04 0.00 175.00

　　Average: 27.77 4.18 23.59 65.14 302.30

　　tps 每秒钟物理装备的 I/O 传输总量

　　rtps 每秒钟从物理装备读入的数据总量

　　wtps 每秒钟向物理装备写入的数据总量

　　bread/s 每秒钟从物理装备读入的数据量，单位为块/s

　　bwrtn/s 每秒钟向物理装备写入的数据量，单位为块/s

　　3、sar -c

　　每秒钟创立的进程数

　　15时10分01秒 1.35

　　15时20分01秒 1.01

　　15时30分01秒 0.59

　　15时40分01秒 1.35

　　15时50分01秒 0.99

　　16时00分01秒 0.57

　　16时10分01秒 1.33

　　16时20分01秒 1.02

　　16时30分01秒 0.57

　　16时40分01秒 1.33

　　16时50分01秒 1.07

　　17时00分01秒 0.56

　　17时10分01秒 1.32

　　4、sar -n DEV 1 5

　　输出网络装备状态的统计信息

　　17时13分42秒 IFACE rxpck/s txpck/s rxbyt/s txbyt/s rxcmp/s txcmp/s rxmcst/s

　　17时13分43秒 eth1 3669.70 4156.57 368362.63 2747714.14 0.00 0.00 0.00

　　17时13分44秒 eth1 2689.11 2585.15 289661.39 701461.39 0.00 0.00 0.00

　　17时13分45秒 eth1 3746.00 4077.00 415178.00 2605720.00 0.00 0.00 0.00

　　17时13分46秒 eth1 3096.00 3241.00 327916.00 1597320.00 0.00 0.00 0.00

　　17时13分47秒 eth1 2910.00 2834.00 312632.00 957903.00 0.00 0.00 0.00

　　Average: eth1 3220.20 3375.60 342592.60 1717931.20 0.00 0.00 0.00

　　rxpck/s： 每秒钟接收的数据包

　　txpck/s： 每秒钟发送的数据包

　　rxbyt/s： 每秒钟接收的字节数

　　txbyt/s： 每秒钟发送的字节数

　　rxcmp/s： 每秒钟接收的压缩数据包

　　txcmp/s： 每秒钟发送的压缩数据包

　　rxmcst/s：每秒钟接收的多播数据包

　　rxerr/s： 每秒钟接收的坏数据包

　　txerr/s： 每秒钟发送的坏数据包

　　coll/s：  每秒冲突数

　　rxdrop/s：因为缓冲充满，每秒钟丢弃的已接收数据包数

　　txdrop/s：因为缓冲充满，每秒钟丢弃的已发送数据包数

　　txcarr/s：发送数据包时，每秒载波错误数

　　rxfram/s：每秒接收数据包的帧对齐错误数

　　rxfifo/s：接收的数据包每秒FIFO过速的错误数

　　txfifo/s：发送的数据包每秒FIFO过速的错误数

　　5、sar -q 1 5

　　输出进程队列长度和平均负载状态统计信息

　　17时16分28秒 runq-sz plist-sz ldavg-1 ldavg-5 ldavg-15

　　17时16分29秒 0 160 0.26 0.11 0.03

　　17时16分30秒 0 160 0.26 0.11 0.03

　　17时16分31秒 0 160 0.24 0.11 0.03

　　17时16分32秒 0 160 0.24 0.11 0.03

　　17时16分33秒 0 160 0.24 0.11 0.03

　　Average: 0 160 0.25 0.11 0.03

　　runq-sz 运行队列的长度（等待运行的进程数）

　　plist-sz 进程列表中进程（processes）和线程（threads）的数量

　　ldavg-1 最后1分钟的系统平均负载（System load average）

　　ldavg-5 过去5分钟的系统平均负载

　　ldavg-15 过去15分钟的系统平均负载

　　6、sar -r

　　输出内存和交换空间的统计信息

+++++++++++++++++++++++++++++++++++++++++

iostat

　　tps 每秒钟物理装备的 I/O 传输总 量。

　　Blk_read 读入的数据总量，单位为 块。

　　Blk_wrtn 写入的数据总量，单位为 块。

　　kB_read 读入的数据总量，单位为 KB。

　　kB_wrtn 写入的数据总量，单位为 KB。

　　MB_read 读入的数据总量，单位为 MB。

　　MB_wrtn 写入的数据总量，单位为 MB。

　　Blk_read/s 每秒从驱动器读入的数据量，单位为块 /s。

　　Blk_wrtn/s 每秒向驱动器写入的数据量，单位为块 /s。

　　kB_read/s 每秒从驱动器读入的数据量，单位为 KB/s。

　　kB_wrtn/s 每秒向驱动器写入的数据量，单位为 KB/s。

　　MB_read/s 每秒从驱动器读入的数据量，单位为 MB/s。

　　MB_wrtn/s 每秒向驱动器写入的数据量，单位为MB/s。

　　rrqm/s 将读入请求合并后，每秒发送到装备的读入请求数。

　　wrqm/s 将写入请求合并后，每秒发送到装备的写入请求数。

　　r/s 每秒发送到装备的读入请求 数。

　　w/s 每秒发送到装备的写入请求 数。

　　rsec/s 每秒从装备读入的扇区 数。

　　wsec/s 每秒向装备写入的扇区 数。

　　rkB/s 每秒从装备读入的数据量，单位为 KB/s。

　　wkB/s 每秒向装备写入的数据量，单位为 KB/s。

　　rMB/s 每秒从装备读入的数据量，单位为 MB/s。

　　wMB/s 每秒向装备写入的数据量，单位为 MB/s。

　　avgrq-sz 发送到装备的请求的平均大小，单位为扇 区。

　　avgqu-sz 发送到装备的请求的平均队列长 度。

　　await I/O请求平均执行时间。包括发送请乞降执行的时间。单位为毫 秒。

　　svctm 发送到装备的I/O请求的平均执行时间。单位为毫 秒。

　　%util 在I/O请求发送到装备期间，占用CPU时间的百分比。用于显示装备的带宽利用率。当这个值靠近100%时，表示装备带宽已经占满。 (IO使用率)