#### create lv {#create-lv}

创建逻辑卷(LVM)

添加3块物理磁盘

[root@node1 ~]#sfdisk -s

/dev/sdc:   4194304

/dev/sdd:   4194304

/dev/sde:   4194304

total: 29360128 blocks

[root@node1 ~]#fdisk /dev/sdc   #分区并标记成LVM类型

[root@node1 ~]#fdisk /dev/sdd

[root@node1 ~]#fdisk /dev/sde

1.创建LVM:

1.1.创建物理卷

[root@node1 ~]# pvcreate /dev/sdc /dev/sdd /dev/sde     #创建 物理卷

 Physical volume &quot;/dev/sdc&quot; successfully created

 Physical volume &quot;/dev/sdd&quot; successfully created

 Physical volume &quot;/dev/sde&quot; successfully created

1.2.创建卷组

[root@node1 ~]# vgcreate testvg /dev/sdc /dev/sdd /dev/sde    #创建 卷组，卷组名为testvg

 Volume group &quot;testvg&quot; successfully created

1.3.创建逻辑卷

[root@node1 ~]# lvcreate -L 2G -n testlv testvg       #创建 逻辑卷，逻辑卷名为testlv,大小为2G

 Logical volume &quot;testlv&quot; created

lvcreate常用选项:

[root@node1 ~]# lvcreate -s, --physicalextentsize PhysicalExtentSize[bBsSkKmMgGtTpPeE]  #指定扩展块儿大小，默认4MB

[root@node1 ~]# lvcreate -l, --extents LogicalExtentsNumber[%{VG|PVS|FREE}]                       #指定扩展快数目

[root@node1 ~]# lvcreate -L, --size LogicalVolumeSize[bBsSkKmMgGtTpPeE]                         #指定逻辑卷大小

[root@node1 ~]# vgextend    #添加物理磁盘到卷组

[root@node1 ~]# vgreduce    #从卷组中移除物理磁盘

[root@node1 ~]# lvresize       #调整逻辑卷大小

[root@node1 ~]# pvscan

[root@node1 ~]# vgscan

[root@node1 ~]# lvscan

[root@node1 ~]# pvdisplay               #查看pv信息

[root@node1 ~]# vgdisplay testvg    #查看vg信息

[root@node1 ~]# lvdisplay   testlv     #查看lv信息

2.扩展LVM

[root@node1 ~]# fdisk -l /testvg/testlv                               #当前LVM容量为2G

[root@node1 ~]# lvresize  -L +4G /dev/testvg/testlv        #node1扩展LVM容量到6G

 Extending logical volume testlv to 6.00 GB

 Logical volume testlv successfully resized

[root@node1 ~]# resize2fs /dev/testvg/testlv                     #报错后，安装系统提示做

resize2fs 1.39 (29-May-2006)

Please run e2fsck -f /dev/testvg/testlv first.

[root@node1 ~]# e2fsck -f /dev/testvg/testlv

.....

[root@node1 ~]# resize2fs /dev/testvg/testlv

resize2fs 1.39 (29-May-2006)

Resizing the filesystem on /dev/testvg/testlv to 1572864 (4k) blocks.

The filesystem on /dev/testvg/testlv is now 1572864 blocks long.

[root@node1 ~]# fdisk -l /testvg/testlv

Disk /dev/testvg/testlv: 6442 MB, 6442217472 bytes

255 heads, 63 sectors/track, 783 cylinders

Units = cylinders of 16065 * 512 = 8225280 bytes

Disk /dev/drbd1 doesnt contain a valid partition table

3.减小LVM

# umount #umount 掉已挂载的文件系统

[root@node1 ~]# fdisk -l /dev/testvg/testlv       #查看当前LVM设备,当前设备大小为6G

Disk /dev/testvg/testlv: 6442 MB, 6442450944 bytes

255 heads, 63 sectors/track, 783 cylinders

Units = cylinders of 16065 * 512 = 8225280 bytes

Disk /dev/testvg/testlv doesnt contain a valid partition table

[root@node1 ~]# resize2fs /dev/testvg/testlv    #压缩LVM到4G

resize2fs 1.39 (29-May-2006)

Resizing the filesystem on /dev/testvg/testlv to 1048576 (4k) blocks.

The filesystem on /dev/testvg/testlv is now 1048576 blocks long.

[root@node1 ~]# lvresize -L 4G /dev/testvg/testlv       #重新设置LVM大小为4G

 WARNING: Reducing active logical volume to 4.00 GB

 THIS MAY DESTROY YOUR DATA (filesystem etc.)

Do you really want to reduce testlv? [y/n]: y

 Reducing logical volume testlv to 4.00 GB

 Logical volume testlv successfully resized

4.添加物理设备到LV

[root@node ~]# pvcreate /dev/sdc8              #创建新的物理卷

[root@node ~]# vgextend testvg /dev/sdc8  #将新的物理卷添加到卷组

[root@node ~]# lvextend                                #扩展LVM

5.移除物理设备从LV  

[root@node ~]# pvmove /dev/sdc5 /dev/sdc8            #把/dev/sdc5上的数据移动到/dev/sdc8上

 /dev/sdc5: Moved: 100.0%

注：若磁盘上没有数据，可跳过此步骤

[root@node ~]# vgreduce testvg /dev/sdc5                  #将物理设备从卷组中移除

 Removed &quot;/dev/sdc5&quot; from volume group &quot;testvg&quot;

[root@node ~]# pvremove  /dev/sdc5                            #将物理设备从物理卷组中移除

   Labels on physical volume &quot;/dev/sdc5&quot; successfully wiped

6.启用/禁用LVM

[root@node ~]# lvchange -an

[root@node ~]# lvchange -ay

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

e.g.:

在lvm中更换硬盘的方法

[root@station85 ~]# pvcreate /dev/hda10         #创建物理卷

 Physical volume &quot;/dev/hda10&quot; successfully created

[root@station85 ~]# vgextend vgname /dev/hda10      #把物理卷加到卷组

 Volume group &quot;vgname&quot; successfully extended

[root@station85 ~]# vgdisplay

........................................................

[root@station85 ~]# pvdisplay      #查看物理卷是否增多，并且知道/dev/hda8这个设备上是有数据的，

                                                         #所以要把这里面的数据移动下/dev/hda10上，才能把/dev/hda8移除，对吧

..........................................................

  --- Physical volume ---

 PV Name               /dev/hda10   #这个就是新增加的

[root@station85 ~]# pvmove /dev/hda8 /dev/hda10 （把/dev/hda8上的数据移动到/dev/hda10上面）

 /dev/sdc8: Moved: 100.0%

[root@station85 ~]# pvdisplay

..........................................................

[root@station85 ~]# vgreduce vgname /dev/hda8 （把/dev/hda8从卷组中移出）

Removed &quot;/dev/hda8&quot; from volume group &quot;vgname&quot;

[root@station85 ~]# vgdisplay

..........................................................

[root@station85 ~]# pvremove /dev/hda8 （把/dev/hda8从物理卷中移出）

 Labels on physical volume &quot;/dev/hda8&quot; successfully wiped

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++