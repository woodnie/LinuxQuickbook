### swapon {#swapon}

创建交换分区

相关命令：

NAME

      swapon, swapoff - enable/disable devices and files for paging and swapping

NAME

      mkswap - set up a Linux swap area

1.使用磁盘分区创建交换分区

1.1.添加硬盘

[root@node ~]# fdisk -l /dev/sdc

Disk /dev/sdc: 1044 cylinders, 255 heads, 63 sectors/track

1.2.创建分区

[root@node ~]# fdisk  /dev/sdc  #分区，创建1块512M的swap partion

[root@node ~]# fdisk -l /dev/sdc #查看一下/dev/sdc

Device Boot      Start         End      Blocks   Id  System

/dev/sdc1               1        1044     8385898+   5  Extended

/dev/sdc5               1          63      505984+  82  Linux swap / Solari

1.3.设置交换分区

[root@node ~]# mkswap /dev/sdc5  #将指定分区设置成交换分区

1.4激活新设置的交换分区

1.4.1.指定交换分区直接激活

[root@node ~]# swapon /dev/sdc5

1.4.2添加交换分区条目到/etc/fstab

[root@node ~]# echo &quot;/dev/sdc5 swap swap 0 0&quot; &gt;&gt; /etc/fstab

[root@node ~]# swapon -a #激活交换分区（该命令读取/etc/fstab文件，并开启它所列出的所有交换条目）

1.5检查交换分区状态

[root@node ~]# swapon -s #检查交换分区状态

1.6停用swap

[root@node ~]# swapoff

2.使用磁盘文件创建交换文件

2.1创建交换分区文件

[root@node ~]# dd if=/dev/zero of=/swapfile bs=512M count=1        #创建512M大小的交换分区文件

2.2.设置交换分区

[root@node ~]# mkswap /swapfile  #设置指定文件为交换分区

2.3激活交换分区

2.3.1.指定交换分区文件直接激活

[root@node ~]# swapon /swapfile

2.3.2 添加交换分区文件条目到/etc/fstab

[root@node ~]# echo &quot;/swapfile swap swap 0 0&quot; &gt;&gt; /etc/fstab

[root@node ~]# swapon -a #激活交换分区（该命令读取/etc/fstab文件，并开启它所列出的所有交换条目）

2.4 检查交换分区状态

[root@node ~]# swapon -s #检查交换分区状态

1.6停用swap

[root@node ~]# swapoff