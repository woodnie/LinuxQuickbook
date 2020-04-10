#### format disk {#format-disk}

创建和加载文件系统

[root@localhost ~]#fdisk -l     **查看到磁盘分区信息

系统中现有两块磁盘:

/dev/hda  **现有的分区

/dev/sda  **为本实验新添加的一块大小为8G的SCSI磁盘

我们在/dev/sda上进行相关操作

2.[root@localhost ~]#fdisk /dev/sda

直接键入m可以得到帮助信息：

a       toggle a bootable flag //设置引导扇区

b       edit bsd disklabel //编辑卷标（linux下使用的卷标bsd通用）

c       toggle the dos compatibility flag //设置和DOS兼容的扇区

d       delete a partition //删除一个分区

l        list known partition types //列出已知分区类型

m      print this menu //显示该菜单

n       add a new partition //添加一个新分区

o       create a new empty DOS partition table //创建个新的DOS分区

p       print the partition table //显示分区表

q       quit without saving changes //保存不退出

s       create a new empty Sun disklabel //创建个新的sun磁盘分区

t        change a partitions system id //修改分区类型

u       change display/entry units

v       verify the partition table

w      write table to disk and exit //写入磁盘退出分区程序

x       extra functionality (experts only)//额外功能(专家级别)

一下是创建一个分区的步骤：

a.键入 n   添加一个新分区

  e 创建一个扩展分区

  p 创建一个主分区

b.键入e创建一个扩展分区

  First cylinder (1-1044, default 1): //选择起始柱面

  Using default value 1//默认选择即从第一个柱面开始

  Last cylinder or +size or +sizeM or +sizeK (1-1044, default 1044)//选择终止柱面，默认不选即是最后一个柱面

  到此我们一个扩展分区就划分成功了，但是还需要在扩展分区里面划分逻辑分区

c. 划分逻辑分区

  这里我们使用 +sizeM  创建了一个4G的逻辑分区

  同时一个标准的Linux分区(ext3)就建立了

  不要忘了，键入ｗ对划分好的分区保存一下

  修改分区类型

  a.键入 l 查看一下Linux下支持的分区类型的Hex code:

  b.键入 t 把当前的分区类型转换成自己想要分区类型

  这里我们把ext3转换成了FAT32

  磁盘/分区格式化：

  Linux 下的格式化命令是 mkfs.[文件系统] [磁盘分区]

  [root@localhost ~]#mkfs.vfat  /dev/sda5                    ##把目标盘格式化成fat类型

  [root@localhost ~]#mkfs. ext3 /dev/sda5                   ##把目标盘格式化成ext3类型

  [root@localhost ~]#mke2fs  -j /dev/sda5                       ##把目标盘格式化成ext3类型