### Debug {#debug}

ldd查找出哪些库将装载指定为参数的动态可执行文件。

ltrace 使您可以跟踪进程的库调用

strace  使您可以跟踪当前运行的进程的所有系统调用。

命令 file 可通过检查 /etc/magic 而确定一个文件或一个文件列表的类型。

有关 ELF 二进制文件的其他信息

用 readelf 实用程序来读取二进制文件的内容。 这甚至可用于为其他硬件体系结构生成的 ELF 文件：

tux@mercury:~&gt; readelf --file-header /bin/ls

文件属性：stat

硬件信息:

命令 lspci 列出 PCI 资源：

lsusb 命令可列出所有 USB 设备。 使用 -v 选项，可打印更加详细的列表。 详细信息从目录 /proc/bus/usb/ 中读取。 以下是连有 USB 设备集线器、内存条、硬盘和鼠标的 lsusb 的输出。

scsiinfo 命令可列出关于 SCSI 设备的信息。 使用选项 -l，可列出系统已知的所有 SCSI 设备（通过 lsscsi 命令可获取类似的信息）。 下面是 scsiinfo -i /dev/sda 的输出，它提供关于一个硬盘的信息。 选项 -a 可提供更加详细的信息。

进程间通讯：ipcs        命令 ipcs 生成当前正在使用的 IPC 资源的列表：

要检查有多少个 sshd 进程正在运行，请将选项 -p 与命令 pidof 一起使用，这将列出给定进程的进程 ID。

tux@mercury:~&gt; ps -p `pidof sshd`

命令 pstree 生成树结构的进程列表：

访问文件的用户：fuser它可用于确定当前哪些进程或用户正在访问特定的文件。 例如，假定您需要卸装已装入 /mnt 的文件系统。umount 返回&quot;设备正忙&quot;。 然后可使用 fuser 命令确定哪些进程正在访问该设备：

打开的文件的列表：lsof

要查看为具有进程 ID PID 的进程打开的所有文件的列表，请使用 -p。 例如，要查看当前 shell 使用的所有文件，请输入.

udevmonitor 可监听由 udev 规则发送的内核 uevent 和 event 并能将事件的设备路径 (DEVPATH) 打印到控制台。 当连接 USB 记忆棒时会出现一系列事件.

X11 客户机所使用的服务器资源：xrestop

xrestop 提供每个连接的 X11 客户机服务器端资源的统计信息。