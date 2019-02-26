### proc {#proc}

/proc 文件系统是一个伪文件系统，在该文件系统中，内核以虚拟文件的形式保留重要信息。 例如，使用以下命令显示 CPU 类型：

用下列命令查询中断的分配和使用： /proc/interrupts

/proc/devices        可用设备

/proc/modules    装载的内核模块

/proc/cmdline    内核命令行

/proc/meminfo 有关内存使用的详细信息

/proc/config.gz    当前运行的内核的 gzip 压缩配置文件

文本文件 /usr/src/linux/Documentation/filesystems/proc.txt 中提供了详细信息。 在 /proc/NNN 目录中提供了当前运行进程的信息，其中 NNN 是相关进程的进程 ID (PID)。 每个进程都可以在 /proc/self/ 中找到它自己的属性：