### get sys info {#get-sys-info}

Memory: free, vmstat, swapon -s, pmap

Processes: ps, top, gnome-system-monitor

Kernel state: uname, uptime, tload

查看系统信息

查看当前操作系统发行版信息

[root@MagentoDBServer ~]# cat /etc/system-release

[root@node ~]# lsb_release -a    #Linux Standard Base

[root@node ~]# cat /etc/issue     #查看当前操作系统发行版信息

CentOS release 5.5 (Final)

查看当前操作系统内核信息

[root@node ~]# uname -a    #查看当前操作系统内核信息

Linux node 2.6.18-194.8.1.el5  #1 SMP Thu Jul 1 19:04:48 EDT 2010 x86_64 x86_64 x86_64 GNU/Linux

查看系统已载入的相关模块

[root@node ~]# lsmod   # - program to show the status of modules in the Linux Kernel

查看硬件信息

1.查看CPU信息

[root@node ~]# cat /proc/cpuinfo | grep &quot;physical id&quot; | sort | uniq | wc -l      #物理cpu个数

[root@node ~]# cat /proc/cpuinfo | grep &quot;cpu cores&quot; | uniq                         #core的个数(即核数) ,逻辑CPU个数=物理个数*核数

[root@node ~]# getconf LONG_BIT  #查看当前CPU运行在32/64bit模式下

32                                                 #当前CPU运行在32bit模式

[root@node ~]# cat /proc/cpuinfo | grep flags | grep lm  #若flag中有lm，说明此CPU支持64bit模式

1、 主板信息

查看主板的序列号

#使用命令

dmidecode | grep -i serial number

#查看板卡信息

cat /proc/pci

2,、cpu信息

#通过/proc文件系统

1) cat /proc/cpuinfo

#通过查看开机信息

2) dmesg | grep -i cpu

3)dmidecode -t processor

3、 硬盘信息

#查看分区情况

fdisk -l

#查看大小情况

df -h

#查看使用情况

du -h

hdparm -I /dev/sda

dmesg | grep sda

4、内存信息

1) cat /proc/meminfo

2) dmesg | grep mem

3) free -m

4) vmstat

5) dmidecode | grep -i mem

5、网卡信息

1) dmesg | grep -i eth

2) cat /etc/sysconfig/hwconf | grep -i eth

3) lspci | grep -i eth

6,、鼠标键盘和USB信息

查看键盘和鼠标：cat /proc/bus/input/devices

查看USB设备：cat /proc/bus/usb/devices

查看各设备的中断请求(IRQ):cat /proc/interrupts

7、 显卡信息

1)lspci |grep -i VGA

2)dmesg | grep -i VGA

8、 声卡信息

1)lspci |grep -i VGA

2)dmesg | grep -i VGA

9、 其他Linux硬件信息命令

.用硬件检测程序kuduz探测新硬件：service kudzu start ( or restart)

.dmesg (查看所有启动时检测到的硬件信息)

.lspci (显示外设信息, 如usb，网卡等信息)

.cat /etc/sysconfig/hwconf

.mpstat

10、 需要手动安装的工具

lshw,hwinfo,hal-device-manager

以上是一些在字符下可以查看机器硬件信息的Linux硬件信息命令。

软件：

$ uname -a linux核心版本（直接运行）

$ php -v   php版本（直接运行）

$ ./httpd -v apache版本（必须转到apache的bin目录下运行）

$ ./mysql --version   mysql版本（必须转到mysql的bin目录下运行）