### mount {#mount}

mount

NAME

      mount - mount a file system

SYNOPSIS

mount [-tvfstype] [-o options] device dir

1\. -tvfstype 指定文件系统的类型。通常不必指定，mount会自动选择正确的类型。常用类型有：

光盘或光盘镜像：iso9660

LINUX文件系统：ext2,ext3

DOSfat16文件系统：msdos

Windows9xfat32文件系统：vfat

WindowsNTntfs文件系统：ntfs

UNIX(LINUX)文件网络共享：nfs

MountWindows文件网络共享：smbfs

2\. -o options主要用来描述设备或档案的挂接方式。

默认选项：

rw：采用读写方式挂接设备

suid：赋予suid文件模式

dev：允许的设备文件

exec：允许执行二进制文件

async：文件更改同步管理

acl：赋予POSIX ACL

其他选项：

loop：用来把一个文件当成硬盘分区挂接上系统

ro：采用只读方式挂接设备

sgid：赋予sgid文件模式

uid=wood ，gid=wood ：在这个例子中，所有挂载的系统中的文件都属于wood

ower：和user选项类似，但这里挂载请求和设备，或者设备特殊文件都必须有相同的EUID

3\. device要挂接(mount)的设备

4\. dir设备在系统上的挂接点(mountpoint)

参数:

[root@localhost ~]# mount -t  nfs

[root@localhost ~]# mount -t  samba

[root@localhost ~]# mount -t  iso9660

[root@localhost ~]# mount -t  vfat

[root@localhost ~]# mount -t  ntfs

[root@localhost ~]# mount  -o remount  目录            ##重新挂载系统中以挂载的目录，可以同时指定其他选项

[root@localhost ~]# mount  -o remount,acl  目录       ##重新挂载系统中以挂载的目录，同时指定支持ACL选项

[root@localhost ~]# mount -a       ##重新挂载/etc/fstab中所有条目

[root@localhost ~]# cat /etc/mtab  #mtab记录当前系统已挂载的文件系统

e.g.:

[root@localhost ~]# mount -t ext3 -o noexex /dev/hda7 /home #为了安全，主目录应该挂载为禁止执行文件

[root@localhost ~]# mount -t iso9660 -o loop /iso/document.iso /mnt

[root@localhost ~]# mount -t vfat -o uid 515,gid=520 /dev/hdc2 /mnt/proj   #指定用户挂载

[root@localhost ~]# mount -t ext3 -o noatime /dev/hda2 /data #可以减少磁盘访问，以提高I/O性能

[root@localhost ~]# mount --bind /something /anotherthing #仅kernel 2.4可用，挂载已挂载的文件系统到其他挂载点

挂载网络文件系统

/etc/init.d/netfs会挂载被配置为要在引导时挂载的任何网络文件系统

netfs支持的网络文件系统

NFS(Network File System)

SMB（Server Message Block）Microsoft使用NetBIOS实现了一个网络文件/打印服务系统，Microsoft称之为SMB

CIFS (Common Internet File System) Microsoft将SMB扩展到Internet上，成为CIFS

NCP(Network Core Protocol)网络核心协议（NCP）管理对 NetWare 服务器资源的访问

1.1手动挂载nfs

[root@localhost ~]# mount         NFS_Server:NFS目录      本地目录                                     #挂载NFS共享目录

[root@localhost ~]# mount.nfs

1.2自动挂载nfs

添加nfs条目到/etc/fstab文件，实现开机自动挂载nfs共享

[root@localhost ~]# echo &quot;nfs_server : /nfs_share    /mnt        nfs        default 0 0&quot; &gt;&gt; /etc/fstab

1.3挂载smba

[root@localhost ~]# mount.cifs  //Smba_sever/smbshare   /mnt   -o smba-user-name        #挂载Smba共享目录

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

mount命令详解

功能：加载指定的文件系统。

语法：mount [-afFhnrvVw] [-L&lt;标签&gt;] [-o&lt;选项&gt;] [-t&lt;文件系统类型&gt;] [设备名] [加载点]

用法说明：mount可将指定设备中指定的文件系统加载到Linux目录下（也就是装载点）。可将经常使用的设备写入文件

/etc/fastab,以使系统在每次启动时自动加载。mount加载设备的信息记录在/etc/mtab文件中。使用umount命令卸载设备时，记录将被清除。

常用参数和选项：

-a 加载文件/etc/fstab中设置的所有设备。

-f 不实际加载设备。可与-v等参数同时使用以查看mount的执行过程。

-F 需与-a参数同时使用。所有在/etc/fstab中设置的设备会被同时加载，可加快执行速度。

-h 显示在线帮助信息。

-L&lt;标签&gt; 加载文件系统标签为&lt;标签&gt;的设备。

-n 不将加载信息记录在/etc/mtab文件中。

-o&lt;选项&gt; 指定加载文件系统时的选项。有些选项也可在/etc/fstab中使用。

这些选项包括：

async 以非同步的方式执行文件系统的输入输出动作。料会先暂时存放在内存中，不会直接写入硬盘

sync   以同步方式执行文件系统的输入输出动作。资料同步写入存储器中。

atime 每次存取都更新inode的存取时间，默认设置，取消选项为noatime。

auto 必须在/etc/fstab文件中指定此选项。执行-a参数时，会加载设置为auto的设备，取消选取为noauto。

defaults 使用默认的选项。默认选项为rw、suid、dev、exec、anto nouser与async。

dev 可读文件系统上的字符或块设备，取消选项为nodev。

exec 可执行二进制文件，取消选项为noexec。

noatime 每次存取时不更新inode的存取时间。

noauto 无法使用-a参数来加载。

nodev 不读文件系统上的字符或块设备。

noexec 无法执行二进制文件。

nosuid 关闭set-user-identifier(设置用户ID)与set-group-identifer(设置组ID)设置位。

nouser 使一位用户无法执行加载操作，默认设置。

remount 重新加载设备。通常用于改变设备的设置状态。

ro 以只读模式加载。

rw 以可读写模式加载。

-r 以只读方式加载设备。

suid 启动set-user-identifier(设置用户ID)与set-group-identifer(设置组ID)设置位，取消选项为nosuid。

user 可以让一般用户加载设备。

特定选项：

rsize： 读文件时的块大小

wsize：写文件时的块大小

e.g.:rsize=8192 and wsize=8192: 会大大加快NFS的传输能力

soft:在I/O失败时，进程带错返回

hard:在试图进入不可达的共享时，进程会被中断

intr:若服务器不可达，允许NFS请求被中断或终止

nolock:禁用文件锁定（locak），允许和原来的NFS服务器间进行互操作

-t&lt;文件系统类型&gt; 指定设备的文件系统类型。

常用的选项说明有：

minix Linux最早使用的文件系统。

ext2 Linux目前的常用文件系统。

msdos MS-DOS 的 FAT。

vfat Win85/98 的 VFAT。

nfs 网络文件系统。

iso9660 CD-ROM光盘的标准文件系统。

ntfs Windows NT的文件系统。

hpfs OS/2文件系统。Windows NT 3.51之前版本的文件系统。

auto 自动检测文件系统。

-v 执行时显示详细的信息。

-V 显示版本信息。

-w 以可读写模式加载设备，默认设置。