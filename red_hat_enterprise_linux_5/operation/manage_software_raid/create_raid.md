#### create raid {#create-raid}

创建软RAID

1.创建RAID分区

[root@localhost ~]# fdisk /dev/sda        ##创建RAID分区

[root@localhost ~]#mkfs.ext3  /dev/sdaX    ##format 相应分区

[root@localhost ~]# partprobe                 ##重新加载分区表

2.创建RAID

[root@localhost ~]# mdadm -C /dev/md[0,1,2\. . .]  -l [0,1,5]  -n [2,3]  /dev/[...]

-C 创建

-l   RAID级别

-n  RAID磁盘数目

-x  RAID备用盘数目

e.g.:

RAID0:

[root@localhost ~]# mdadm -C /dev/md0 -l 0 -n 2 /dev/sda5 /dev/sda6

RAID1:

[root@localhost ~]# mdadm -C /dev/md1 -l 1 -n 2 /dev/sda7 /dev/sda8

RAID5:

[root@localhost ~]# mdadm -C /dev/md5 -l 5 -n 3 /dev/sda5 /dev/sda6 /dev/sda7

3.查看RAID

[root@localhost ~]# mdadm -D         /dev/md0    #查看RAID详细信息

[root@localhost ~]# mdadm -Ds                           # -s:scan

[root@localhost ~]# mdadm -Ds /dev/md0 &gt;  /etc/mdadm.conf   #创建配置文件

[root@localhost ~]# mdadm -Ds /dev/md1&gt;&gt; /etc/mdadm.conf

[root@localhost ~]# mdadm --detail /dev/md0    #查看RAID详细信息

[root@localhost ~]# cat /proc/mdstat

4.启用/停止RAID

[root@localhost ~]# mdadm -A /dev/md0                                # 参照配置文件启动RAID-A --assemble

[root@localhost ~]# mdadm -A /dev/md0  /dev/sdc[5,6]        # 丢失配置文件的情况下

[root@localhost ~]# mdadm -As                                               # -scan

[root@localhost ~]# mdadm -S /dev/md0                                # -S--stop

[root@localhost ~]# mdadm -Ss

5.替换

[root@localhost ~]# mdadm /dev/mdX -f RAID成员   #标记故障

[root@localhost ~]# mdadm /dev/mdX -r RAID成员   #移除设备

[root@localhost ~]# mdadm /dev/mdX -a RAID成员  #添加设备