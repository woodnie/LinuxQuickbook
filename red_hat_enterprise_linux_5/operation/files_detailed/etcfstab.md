#### /etc/fstab {#etc-fstab}

/etc/fstab

在系统初始化过程中(/etc/rc.d/rc.sysinit)，可用 /etc/fstab 来生成文件系统体系。

在fstab中创建条目后，最好使用 mount -a （在fstab没有auto选项的情况下）挂载所有条目并检查错误。

因为fstab文件可能会导致rc.sysinit失败并引导至sulogin(紧急模式)

#device                                         mount_point                 FS_type          options                     dump_freq           fsck_order

e.g.:

[root@node ~]# cat /etc/fstab

/dev/VolGroup00/LogVol00   /                ext3      defaults        1 1

LABEL=/boot               /boot             ext3      defaults        1 2

tmpfs                     /dev/shm          tmpfs     defaults        0 0

devpts                    /dev/pts          devpts    gid=5,mode=620  0 0

sysfs                     /sys              sysfs     defaults        0 0

proc                      /proc             proc      defaults        0 0

/dev/VolGroup00/LogVol01   swap             swap      defaults        0 0

#/dev/sdb1                /data             ext3      defaults        0 0

LABEL=datadisk            /data             ext3      defaults,        0 0

device：     设备，或者标签

mount_point：挂载点

FS_type：    文件系统类型

options：    等价于mount的选项

dump_freq：  零级别转储频率。0：从不转储；1：每天；2：每隔一天；等等(dump已经基本不使用了？)

fsck_order:  0：  忽略，不进行磁盘检查；（网络文件系统和光驱应该被忽略）

            1：  第一，首先对该分区进行检查；（根文件系统应该使用该值）

            2-9：第二，第三等等；

                 含有比1大的相同数值的文件系统被视为平行文件系统

                 依照数字从小到大的顺序检查磁盘

当fstab条目可用时，mount的特殊选项：

可用条目：#/dev/sdb1              /data                   ext3    defaults        0 0

[root@node ~]# mount /dev/sdb1

可用条目：#LABEL=datadisk          /data                   ext3    defaults        0 0

[root@node ~]# mount -L datadisk

[root@node ~]# mount LABEL=datadisk

[root@node ~]# mount datadisk   ??(实验未成功)