### mknod {#mknod}

mknod

NAME

      mknod - make block or character special files

      建立块专用或字符专用文件

管理设备

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

crw-rw-rw- 1 root tty  5,  0 11-20 14:02 /dev/tty  #注意主、副设备号的不同

crw-rw---- 1 root tty  4,  0 11-20 14:01 /dev/tty0

crw------- 1 root root 4,  1 11-20 15:10 /dev/tty1

crw------- 1 root root 4,  2 11-20 14:06 /dev/tty2

crw------- 1 root root 4,  3 11-20 14:06 /dev/tty3

crw------- 1 root root 4,  4 11-20 14:06 /dev/tty4

crw------- 1 root root 4,  5 11-20 14:06 /dev/tty5

crw------- 1 root root 4,  6 11-20 14:06 /dev/tty6

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

[root@node ~]# mknod /dev/usvdevice b 8 32

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

mknod - 建立块专用或字符专用文件  

总览

mknod [options] name {bc} major minor

mknod [options] name p  

GNU 选项（缩写）：

[-m mode] [--help] [--version] [--]  

描述

mknod 用指定名称产生一个FIFO（命名管道），字符专用或块专用文件。

文件系统中的一个专用文件存贮着三种信息（布朗型、整型、整型）。布朗型在字符文件与块文件之间作出选择，两个整型是主、次设备号。

通常，一个专用文件并不在磁盘上占用空间，仅仅是为操作系统提供交流，而不是为数据存贮服务。一般地，专用文件会指向一个硬件设备（如：磁盘、磁带、打印机、虚拟控制台）或者操作系统提供的服务（如：/dev/null, /dev/random）。

块文件通常类似于磁盘设备（在数据可以被访问的地方赋予一个块号，意味着同时设定了一个块缓存）。所有其他设备都是字符文件。（以前，两种文件类型间是有差别的。比如：字符文件I/O没有缓存，而块文件则有。）

mknod命令就是用来产生这种类型文件的。

以下参数指定了所产生文件的类型：

   pFIFO型

   b块文件

   c字符文件

GNU版本还允许使用u（unbufferd非缓冲化），以保持与C语言的一致。

当创建一个块文件或字符文件时，主、次设备号必须在文件类型参数后给出。（十进制或八进制以0开头；GNU 版本还允许使用以0x开头的十六进制）缺省地，所产生的文件模式为0666（a+rw）。  

选项

-m mode, --mode=mode 为新建立的文件设定模式，就象应用命令chmod一样，以后仍然使用缺省模式建立新目录。

GNU 标准选项:

--help 在标准输出上显示使用信息并顺利退出。

--version 在标准输出上显示版本信息并顺利退出

-- 终端选项列表。

遵循

POSIX 认为该命令不能移植而不支持这个命令，它推荐使用 mkfifo(1)来建立FIFO文件。SVID有一个命令/etc/mknod有以上语法，但没有模式选项。  

注意:

在某些linux系统上（1.3.22或之后的版本） /usr/src/linux/Documentation/devices.tex文件包含了一个设备列表，包括设备名、类型及主、次设备号。本页对mknod的描述可以在fileutils-4.0中找到；其他版本会略有差别。