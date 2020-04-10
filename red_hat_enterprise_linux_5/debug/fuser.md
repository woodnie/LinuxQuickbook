### fuser {#fuser}

fuser

NAME

      fuser - identify processes using files or sockets

      显示使用文件的进程信息

[root@node ~]# fuser -v     mount_point      #查看打开文件系统的进程

[root@node ~]# fuser -km  mount_point     #终止指定文件系统中的活动

比如当你想umount光驱的时候，结果系统提示你设备正在使用或者正忙，可是你又找不到到底谁使用了他。这个时候fuser可派上用场了。

[root@lancy sbin]# eject  umount: /media/cdrom: device is busy  umount: /media/cdrom: device is busy  eject: unmount of `/media/cdrom failed   [root@lancy sbin]# fuser /mnt/cdrom  /mnt/cdrom: 4561c 5382c   [root@lancy sbin]# ps -ef |egrep (4561|5382) |grep -v grep  root 4561 4227 0 20:13 pts/1 00:00:00 bash  root 5382 4561 0 21:42 pts/1 00:00:00 vim Autorun.inf