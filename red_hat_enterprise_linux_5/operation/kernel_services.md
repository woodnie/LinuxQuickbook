### kernel services {#kernel-services}

内核服务

1.

内核安装位置：/boot/vmlinuz-$(uname -r)

内核模块位置：/lib/modules/$(uname -r)

2.mod

[root@node ~]# lsmod #列出系统中医装载的模块

[root@node ~]# modprobe    mod_name   #装载模块

[root@node ~]# modprobe -r mod_name   #删除模块

[root@node ~]# modinfo mod_name          #显示模块信息

e.g.:

[root@node ~]# lsmod |grep usb*                            #列出有关usb的mod，此时未包含usb_storage mod

asus_acpi              50917  0

ac97_bus               35649  1 snd_ac97_codec

[root@node ~]# modprobe usb_storage                    #添加usb_storage mod

[root@node ~]# lsmod |grep usb*                            #列出有关usb的mod，此时已包含usb_storage mod

usb_storage           123041  0        

asus_acpi              50917  0

ac97_bus               35649  1 snd_ac97_codec

scsi_mod              196953  8 usb_storage,scsi_dh,sg,libata,mptspi,mptscsih,scsi_transport_spi,sd_mod

[root@node ~]# modprobe -r usb_storage                  #删除usb_storage mod

[root@node ~]# lsmod |grep usb*                             #列出有关usb的mod，查看是否已删除

asus_acpi              50917  0

ac97_bus               35649  1 snd_ac97_codec

3.conf文件

/etc/modprobe.conf used for module configuration：

     无论何时装载模块都需要为其提供的参数

     代表模块名称的别名

     在装载或者卸载模块时需要执行的命令

4.管理initrd映像

initrd:为系统提供一系列内核映像无法提供的模块，通常和存储设备和文件系统有关

初始化内存盘提供在引导初期装载的模块.

文件位于 /boot/initrd-$(uname -r).img下

有时会由于某种原因添加额外的模块：

       系统中添加新硬件，如：SCSI控制器

       需要新的特性，例如：USB驱动

       需要在引导时自动装载模块

使用 initrd 命令 -- with  选项可重建带附加模块的系统：

mkinitrd --with=module name   /boot/initrd-$(uname -r).img  $(uname -r)    

5.管理设备

1)./dev/目录下的文件可用来访问驱动程序

2).读取数据或向其中写入数据是可行的操作：

          读取数据: cat /dev/dev/pts/0              #在一个伪终端操作，在另一个伪终端上观察结果

          写入数据: echo &quot;hello&quot; &gt; /dev/pts/0    #在一个伪终端操作，在另一个伪终端上观察结果

3).设备文件属性（这三个属性可帮助确定访问哪个驱动程序）：

设备类型            #(character or block)        

主设备号            #用于驱动程序区别不同设备

副设备号            #用于驱动程序区别形同设备的不同类型

e.g.：

[root@node ~]# ll /dev/hdc

crw-rw-rw- 1 root tty 5, 0 11-20 14:02 /dev/tty          #注意主、副设备号的不同

crw-rw---- 1 root tty  4, 0 11-20 14:01 /dev/tty0

crw------- 1 root root 4, 1 11-20 15:10 /dev/tty1

crw------- 1 root root 4, 2 11-20 14:06 /dev/tty2

crw------- 1 root root 4, 3 11-20 14:06 /dev/tty3

crw------- 1 root root 4, 4 11-20 14:06 /dev/tty4

crw------- 1 root root 4, 5 11-20 14:06 /dev/tty5

crw------- 1 root root 4, 6 11-20 14:06 /dev/tty6

brw-r----- 1 root disk 8,  0 11-20 14:02 /dev/sda      

brw-r----- 1 root disk 8,  1 11-20 14:02 /dev/sda1

brw-r----- 1 root disk 8, 16 11-20 14:02 /dev/sdb

brw-r----- 1 root disk 8, 17 11-20 14:02 /dev/sdb1

4).设备类型：

块设备：

/dev/hda, /dev/hdc: IDE硬盘，光驱

/dev/sda, /dev/sdb  SCSI,SATA,USB等

/dev/md0, /dev/md1 软件RAID设备

字符设备：

/dev/tty[0-6]:虚拟控制台  （由内核VGA程序生成）

/dev/pst0-  :伪终端          （伪终端都是由特殊的虚拟文件系统devpts生成的，如：ssh,gnome-terminal等）

/dev/null, /dev/zero 软件设备

/dev/random, /dev/urandom 随机数字

5).用udev管理/dev

udev 可管理保存在/dev/目录下的文件。文件只有在接入相应的设备后才会生成，文件在设备被拔出后自动删除。

/etc/udev/rules.d/中的udev报告可确定：

文件名

权限

拥有者和组

当出现新硬件时需要运行的命令

e.g.:（在/dev中添加设备文件）

[root@node ~]# cat /etc/udev/rules.d/my-new.rules   #自定义文件名

KERNEL== &quot;sdc&quot;,NAME=&quot;usbdevice&quot;,SYMLINK=&quot;usbstorage&quot;    

#udev给/dev/sdc设备，生成名为&quot;usbdevice&quot;的设备文件，和设备文件的符号链接usbstorage

[root@node ~]# ll /dev/u*

brw-r----- 1 root disk   8,   32 11-20 22:55 /dev/usbdevice

lrwxrwxrwx 1 root root         9 11-20 22:55 /dev/usbstorage -&gt; usbdevice

e.g.:(使用mknod命令)

mknod /dev/usvdevice b 8 32

6.用/proc进行内核配置

用/proc获得（使用cat,strings命令）内核配置信息或者进行内核设置。

/proc是虚拟文件系统：1)文件没有保存到硬盘上；2)条目没有保留，重新引导后修改会被重新初始化。

用来显示进程信息、内存资源、硬件设备、内核内存等

可用来修改网络和内存子系统或者修改内核属性

修改立即生效

e.g.:（修改内核配置）

使用sysctl修改 ：

[root@node ~]# sysctl -a  #查看所有内核配置

[root@node ~]# sysctl -a |grep ip_forward

net.ipv4.ip_forward = 0    #1禁用ip forward

[root@node ~]# sysctl -w net.ipv4.ip_forward=1

net.ipv4.ip_forward = 1    #1开启ip forward

[root@node ~]# sysctl -p  #重新载入/ect/sysctl.conf

++++++++++++++++++++++++++++++++++++++

net.ipv4.ip_forward = 0    #0禁用ip forward

net.ipv4.ip_forward = 1    #1开启ip forward

net.ipv4.icmp_echo_ignore_all = 0  #忽略

net.ipv4.icmp_echo_ignore_all = 1  #不忽略

++++++++++++++++++++++++++++++++++++++

修改/proc下的配置:

[root@node ~]# cat /proc/sys/net/ipv4/ip_forward

0

[root@node ~]# echo 1 &gt;&gt; /proc/sys/net/ipv4/ip_forward    

[root@node ~]# cat /proc/sys/net/ipv4/ip_forward      

1

修改/etc/sysctl.conf 配置文件（重启有效）

[root@node ~]# sysctl -p  #重新载入/ect/sysctl.conf    #重新载入配置文件，使其及时（长久）生效

7.检测硬件设备

所有接入设备的快照都由HAL（硬件提取层，Hardware Abstraction Layer）管理

hal-device 以文本模式列出所有设备.

hal-device-manager 在图形窗口中显示所有设备.

lspci and lsusb 分别列出与PCI和USB总线连接的设备

/proc and /sys 文件系统也含有总线和设备的特殊信

e.g.:

[root@node ~]# lspci

[root@node ~]# lsusb

8.监控进程和资源

/proc提供的可用信息有时候很难理解

有些界面可格式化这些数据，并使之更容易访问

Memory: free, vmstat, swapon -s, pmap

Processes: ps, top, gnome-system-monitor

Kernel state: uname, uptime, tload