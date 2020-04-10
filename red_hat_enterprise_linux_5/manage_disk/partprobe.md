### partprobe {#partprobe}

partprobe

在系统启动时，内核会为其自身从硬盘复制分区表。大多数 fdisk 工具会编辑分区表的磁盘副本。要更新内存中的副本，就运行 partprobe

[root@node ~]# partprobe