### start process {#start-process}

引导顺序：

1.BIOS初始化

2.引导装载程序

3.内核初始化

4.启动init,并进入预期的运行级别：

  4.1 /etc/rc.d/rc.sysinit

  4.2 /etc/rc.d/rc and /etc/rc.d/rc[012345].d/

  4.3 /etc/rc.d/rc.local

  4.4 在适当的情况下使用X显示管理器

2.引导装载程序

   1st Stage  （第一阶段） -    容量小，位于MBR或引导部分

   2nd Stage （第二阶段） -    从引导分区装载

引导装载程序对Linux的最低要求:

       标签、内核位置、根文件系统OS位置和初始ram盘位置（initrd)

        e.g.:一个简单的groub.conf

       title Red Hat Enterprise Linux Server (2.6.18-194.el5)                                                  #标签

              root (hd0,0)

              kernel /vmlinuz-2.6.18-194.el5 ro root=/dev/VolGroup00/LogVol00 rhgb quiet          #vmlinuz(virtual memory)是可执行的Linux内核

              initrd /initrd-2.6.18-194.el5.img                                                                           #初始ram盘位置（initrd)

引导装载程序对其它OS的最低要求:

        引导装置、标签

3内核引导功能

设备检测

设备驱动程序初始化

以只读形式挂载根文件系统

载入初始进程

4.启动init,并进入预期的运行级别：

Init读取其初始化文件: /etc/inittab：

1）初始运行级别

    引导时选择在/etc/inittab中指定的默认级别

    从引导装载程序传递一个参数

    使用命令init new_runlevel

/sbin/runlevel  #查看运行级别

默认定义的运行级别：

0               停止

1               单用户模式                   # /etc/rc.sysinit /etc/rc1.d/

2               不带NFS的多用户模式

3                完全多用户模式

4                无官方定义

5                图形化

6                重启

s,S,single   备用单用户模式             # /etc/rc.sysinit

emergency 绕过rc.sysinit，不执行任何脚本，只执行sulogin

2）系统初始化脚本

3）对应运行级别的脚本目录

4）捕获某个关键字顺序

5）定义UPS电源中断/恢复脚本

6）在虚拟控制台生成getty

7）在运行级别5初始化X

4.1/etc/rc.d/rc.sysinit主要任务包括:

激活udev和selinux

在/etc/sysctl.conf中设定内核参数

设定系统时钟

装载按键设置

启用交换分区

设置主机名

检查并重新挂载根文件系统

激活RAID和LVM设备

启用磁盘配额

检查并挂载其它文件系统

清理过时的锁和PID文件

4.2系统V运行级别

运行级别定义需要启动的服务

     每个运行级别都有对应的目录：

            /etc/rc.d/rc?.d

     系统V init脚本位于：

            /etc/rc.d/init.d

     运行级别目录中的符号链接使用start和stop参数调用init.d

在rc0.d ~ rc6.d目录中，以S开头的文件为执行该服务，以K开头的文件则是杀掉该服务的意思。

那些数字代表启动的顺序，例如S12syslog会比S90crond先执行。

确立这些顺序是有原因的，例如，您的主机要启动WWW，

那么网络设定应该先启动，因为，如果先启动WWW后驱动网络，那么WWW一定起不来，所以各项服务的启动顺序相当重要。

4.3/etc/rc.d/rc.local

在指定运行级别脚本后运行

通常可以进行自定义修改

在大多数情况下，建议您在/etc/rc.d/init.d中建立一个系统V init脚本

除非您启动的服务并不重要，您可以根据现有脚本进行修改