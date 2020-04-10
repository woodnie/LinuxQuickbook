### trouble shooting {#trouble-shooting}

trouble shooting

数据收集工具：

history

grep

diff      #对比文件不同之处

find /dir -cmin -60  #查找60分钟内被修改过的文件

strace command   #收集命令的执行数据

strace -o &lt;out_file&gt; &lt;command&gt;   # -o 将输出信息保存到指定文件中

strace -p PID        #收集进程的执行数据

tail -f logfile

生成附加信息：

1.获取debug信息

#echo &quot; *.debug       /var/log/debug&quot; &gt;&gt; /etc/syslog.conf

2.调试模式状态下运行程序（--debug -d）

e.g.:调试状态下运行xinit

EXTRAOPTIONS=&quot;-d&quot;

# /etc/rc.d/init.d/xinetd restart

1.X trouble

1.在init 3中调试X

2.#system-config-display

3.#X -probeonly  //probeonly 启动X服务所需要的所有任务，但并不启动X服务

                       //Shift + up /down  翻页

                       //硬件选择性配置特性信息  /usr/share/hwdata/

4.确认xfs 字体服务器运行正常

       字体路径：/etc/X11/xorg.conf

       重建字体索引：#mkfontdir

       注释/etc/X11/fs/config中字体目录需找问题

6.X是网络服务，主机名等信息的更该可能会引起一些故障

5.文件系统配额限度会影响X的运行

2.网络trouble

1.主机名解析

2.IP等的配置

3.网关

4.网卡驱动程序配置： /etc/modprobe.conf

5.设备是否激活

3.引导过程trouble

引导顺序：

1st：Bootloader configuration

2nd：Kernel

3rd：/sbin/init

4th：/etc/rc.d/rc.sysinit

5th：/etc/rc.d/rc, /etc/rc.d/rc?.d/

    /etc/rc.d/rc.local

    X

1.Bootloader没有出现

  GRUB配置错误；引导部分(MBR)受损；BIOS设置错误

2\. 内核载入失败

   内核映像受损；GRUB传递给内核的参数错误

3.运行/sbin/init和根文件系统挂载失败

  GRUB配置错误；/sbin/init 文件受损；/etc/inittab配置错误；根文件系统受损

4.运行etc/rc.d/rc.sysinit失败

  /bin/bash受损；/etc/fstab受损；软RAID配置错误；配额配置错误；非根文件系统受损（fsck检查失败）

5.运行级别错误（通常是服务错误）

  特定服务配置错误；X相关服务错误；

4.文件系统trouble

ext2文件系统：

ext2文件系统挂载为读写时，系统会将该文件系统标记为dirty。只有umount后，才将其标记为clean。

一个没有挂载，但被标记为dirty的文件系统，可能是由于没有被正常卸载，可能导致文件系统受损。

因为在系统崩溃的时候不知道在写入什么内容，因此需要检查整个文件系统来修复问题。

ext3文件系统：

在ext2的基础上，引入了可处理的日志，该日志会记录系统崩溃时在写入什么。

ext3文件系统挂载为读写时，系统会将该文件系统标记为clean。

但其有可能发生损坏，所以在引导的时候，会按照日志信息检查并修复受损。

fsck就是这样的检查修复程序

fsck程序是系统中用来检查标准文件系统的程序的前端程序。

fsck实际运行e2fsck(fsck.ext2,fsck.ext3) disfsck等

fsck根据/etc/fstab来检查

5.修复运行级别

1                单用户模式                   # /etc/rc.sysinit /etc/rc1.d/

s,S,single   备用单用户模式             # /etc/rc.sysinit

emergency 绕过rc.sysinit，不执行任何脚本，只执行sulogin

6.rescue环境

1.文件系统重建

Anaconda 会询问挂载文件系统的方式：

       RW：  对应选项[continue]

       RO：  对应选项[ReadOnly]

       Skip：对应选项[Skip],跳过挂载文件系统

用户的根文件系统会挂载在 /mnt/sysimage/*

                                        /mnt/source

用户的家目录是/temp

2.文件系统节点

rescue环境中只提供特定的设备节点

rescue环境中额外设备节点可通过mknode生成

rescue环境中的mknod版本会自动关联设备程序的major minor值：

          #mknod /dev/sd[abcd..]

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

e.g.:在rescue环境中修复MBR

[root@node ~]# mount

[root@node ~]# df

[root@node ~]# fstab     /*用于查看boot的挂载点

/dev/sda1           /boot    /*boot挂载在/dev/sda1上

[root@node ~]# dd if=/dev/sda  of=/root/mbr.bak bs=512 count=1   #备份MBR

[root@node ~]# dd if=/dev/zero of=/dev/sda  bs=446 count=1         #破坏MBR中的bootloader程序(grub)

[root@node ~]# reboot

#linux rescue  /*进入rescue环境

#选择 continue

sh3.2# chroot /mnt/sysimage  /*放弃当前的跟目录，使用/mnt/sysimange作为根目录

sh3.2# cat /boot/grub/grub.conf

         title Red Hat Enterprise Linux Server (2.6.18-194.el5)

               root (hd0,0)

               kernel /vmlinuz-2.6.18-194.el5 ro root=/dev/VolGroup00/LogVol00 rhgb quiet

               initrd /initrd-2.6.18-194.el5.img

sh3.2# grub-install  /dev/sda

sh3.2# exit         /*退出当前跟环境

sh3.2# grub        /*进入grub

grub&gt; root (hd0,0)

grub&gt; setup (hd0)

grub&gt; quit

sh3.2# reboot

e.g.:在rescue环境中安装软件包

[root@node ~]# cp -av /bin/mount /bin/mount.bak

[root@node ~]# rm /bin/mount

[root@node ~]# reboot

#linux rescue  /*进入rescue环境

#选择 continue

sh3.2# chroot /mnt/sysimage  /*放弃当前的跟目录，使用/mnt/sysimange作为根目录

sh3.2# rpm -V util-linux  /* mount被包含在util-linux包中

sh3.2# exit         /*退出当前跟环境

sh3.2# mount /dev/hdc /mnt/sysimage/mnt

sh3.2# cd mnt/sysimage/mnt

sh3.2# rpm -ivh --force --root /mnt/sysimage util-linux*

sh3.2# reboot

sh3.2# sh3.2# sh3.2#

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

[root@node ~]# rpm -ivh rhce-ts-8.0-9.noarch.rpm

gertrude

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

truble:

inittab 被修改

id:3:initdefault:

l0:0:wait:/etc/rc.d/rc 0

l1:1:wait:/etc/rc.d/rc 1

l2:2:wait:/etc/rc.d/rc 2

l3:3:wait:/etc/rc.d/rc 3

l4:4:wait:/etc/rc.d/rc 4

l5:5:wait:/etc/rc.d/rc 5

l6:6:wait:/etc/rc.d/rc 6

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

truble:

grub.conf丢失

1.进入rescue生成

2.在grub提示符中直接添加：

grub&gt;kernel....

grub&gt;initrd....

grub&gt;boot

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

truble:

rm /etc/fstab

rm /etc/mtab

rescue:

vi ../etc/fstab

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

truble:

rm /boot/

rm /etc/fstab

rm /etc/mtab

rescue:

1.vi /etc/fstab

2.cat /proc/mounts &gt;&gt; /ect/mtab

3.rpm kernel

4.rpm grub

5.vi ../boot/grub.conf

++++++++++++++++++++++++++

inittab丢失

rc.d/* 丢失

initscripts-xxxx.rpm 生成很多系统关键文件，所以这里不能直接安装该包，需要对该包copy 出来，解压后，然后copy 需要的系统文件即可！

copy initscipts-9.03.17-1.el6.i686.rpm 至 /tmp 目录下，用rpm2cipo 指令通过管道cpio -imd解压该rpm包，解压后，会发现，这个文件内容都是/ 目录下关键的目录文件

++++++++++++++++++++++++++++++