### umount {#umount}

umount

NAME

      umount - unmount file systems

[root@node ~]# cat /etc/mtab  #mtab 记录当前系统已挂载的文件系统

umount [options] device | mount_point

你不能卸载正在使用的文件系统

使用 fuser 检查或终止进程

[root@node ~]# fuser -v     mount_point      #查看打开文件系统的进程

[root@node ~]# fuser -km  mount_point     #终止指定文件系统中的活动