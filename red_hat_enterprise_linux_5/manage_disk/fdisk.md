### fdisk {#fdisk}

fdisk

fdisk : 磁盘分区

1.fdisk -l        ##列出当前系统中的分区

2.fdisk /dev/...

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

A BSD/SUN type disklabel can describe 8 partitions

An IRIX/SGI type disklabel can describe 16 partitions