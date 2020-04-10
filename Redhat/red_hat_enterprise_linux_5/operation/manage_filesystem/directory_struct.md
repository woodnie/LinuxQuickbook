#### directory struct {#directory-struct}

Linux 目录结构

..指代任何给定目录的父目录

. 指代当前目录

以. 开头的文件和目录拥有隐藏属性

[http://www.pathname.com/fhs/](http://www.pathname.com/fhs/)  #文件系统结构标准网站

一些重要的目录:

主目录： /root,/home/username

用户可执行文件目录: /bin, /usr/bin, /usr/local/bin

系统可执行文件目录: /sbin, /usr/sbin, /usr/local/sbin

共享库: /lib, /usr/lib, /usr/local/lib

配置: /etc

临时文件: /tmp

内核和引导载入程序: /boot

服务器数据: /var, /srv

系统信息： /proc, /sys

其它挂载点: /media, /mnt

/lost found:

/lost+found这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

一般重启或关闭system，可下sync;sync.避免有些message会留在硬盘之cache上，此时dirty bit为1，当再开机时，system会去检查每一个dirtybit是否为0，假如为1则会执行fsck。作fsck时，常会问要否删除 dirtybit，如选yes时，会把inode集中放在lost found?用file 指令去查寻，不重要再行删除即可（inode number）。简单言之，本目录乃记录硬盘上的partition于资料流失时作fsck寻找回来的遗失文件片段

目录结构及主要内容&quot;/&quot;根目录部分有以下子目录：

/usr 目录包含所有的命令、程序库、文档和其它文件。usr是Unix Software Resource的縮寫.这些文件在正常操作中不会被改变的。这个目录也包含你的Linux发行版本的主要的应用程序，譬如，Netscape。

/var 目录包含在正常操作中被改变的文件：假脱机文件、记录文件、加锁文件、临时文件和页格式化文件等。

/srv 目录包含服务器数据，例如数据库和网页

/home 目录包含用户的文件：参数设置文件、个性化文件、文档、数据、EMAIL、缓存数据等。这个目录在系统省级时应该保留。

/proc 目录整个包含虚幻的文件。它们实际上并不存在磁盘上，也不占用任何空间。(用ls -l 可以显示它们的大小)当查看这些文件时，实际上是在访问存在内存中的信息，这些信息用于访问系统

/bin 系统启动时需要的执行文件（二进制），这些文件可以被普通用户使用。

/sbin 系统执行文件（二进制），这些文件不打算被普通用户使用。（普通用户仍然可以使用它们，但要指定目录。）

/usr/local/bin

/sue/local/sbin

/etc 操作系统的配置文件目录。

/root 系统管理员（也叫超级用户或根用户）的Home目录。

/lib 根文件系统目录下程序和核心模块的共享库。

/boot 用于自举加载程序（LILO或GRUB）的文件。当计算机启动时（如果有多个操作系统，有可能允许你选择启动哪一个操作系统），这些文件首先被装载。这个目录也会包含LINUX核（压缩文件vmlinuz），但LINUX核也可以存在别处，只要配置LILO并且LILO知道LINUX核在哪儿。

/tmp 临时文件。该目录会自动清理存在时间超过10天的文件

/lost+found 在文件系统修复时恢复的文件

/dev 设备文件目录。

LINUX下设备被当成文件，这样一来硬件被抽象化，便于读写、网络共享以及需要临时装载到文件系统中。正常情况下，设备会有一个独立的子目 录。这些设备的内容会出现在独立的子目录下。LINUX没有所谓的驱动符。

/dev/shm  内存设备

/dev/pts    devpts

/dev/disk/  磁盘信息

# ls -l  /dev/disk/by-label/

# ls -l  /dev/disk/by-path/

# ls -l  /dev/disk/by-uuid/

/opt 可选的应用程序，譬如，REDHAT 5.2下的KDE （REDHAT 6.0下，KDE放在其它的XWINDOWS应用程序中，主执行程序在/usr/bin目录下）

/usr目录下比较重要的部分有：

/usr/X11R6 X-WINDOWS系统（version 11, release 6)

/usr/X11 同/usr/X11R6 （/usr/X11R6的符号连接）

/usr/X11R6/bin 大量的小X-WINDOWS应用程序（也可能是一些在其它子目录下大执行文件的符号连接）。

/usr/doc LINUX的文档资料（在更新的系统中，这个目录移到/usr/share/doc）。

/usr/share 独立与你计算机结构的数据，譬如，字典中的词。

/usr/bin和/usr/sbin 类似与&quot;/&quot;根目录下对应的目录（/bin和/sbin），但不用于基本的启动（譬如，在紧急维护中）。大多数命令在这个目录下。

/usr/local 本地管理员安装的应用程序（也可能每个应用程序有单独的子目录）。在&quot;main&quot;安装后，这个目录可能是空的。这个目录下的内容在重安装或升级操作系统后应该存在

/usr/local/bin 可能是用户安装的小的应用程序，和一些在/usr/local目录下大应用程序的符号连接。

/usr/src kernal源代码目录

/proc目录的内容：

/proc/cpuinfo 关于处理器的信息，如类型、厂家、型号和性能等。

/proc/devices 当前运行内核所配置的所有设备清单。

/proc/dma 当前正在使用的DMA通道

/proc/filesystems 当前运行内核所配置的文件系统。

/proc/interrupts 正在使用的中断，和曾经有多少个中断。

/proc/ioports 当前正在使用的I/O端口

/sys目录