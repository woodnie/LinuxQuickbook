### cdrecord {#cdrecord}

cdrecord

cdrecord 可以使用来烧录 audio 或 data, 至于烧录 iso 档可用下述步骤简易操作

1\. 找出 SCSI Addresses 在系统提示符号下 cdrecord -scanbus, 以显示 SCSI Addresses 例如 0,0,0 (bus,channel,lun)

2\. 使用 cdrecord 工具指令烧录 iso 文件

在系统提示符号下 cdrecord -v -eject speed=12 dev=0,0,0 -data Mandrake82-cd1-inst.i586.iso

-v : 显示烧录过程

-eject : 烧录结束自动退片

speed= : 指定烧录速度

dev= : 指定烧录设备

-data : 指定烧录档案

可以 , 我没记错的话 , xcdroast 是调用 cdrecord 的

cdrecord 工具程序刻写音频、数据、以及 混合模式（ mixed-mode ）（音频、视频、和数据 的组合）光盘， 它使用选项来配置刻写过程中的各种设置，包括 速度、设备、以及数据设置。

要使用 cdrecord ， 你必须首先建立你的可（重）写光盘设备的设备地址，方法是在 shell 提示下以根用户身份来运行下列命令：

cdrecord -scanbus

该命令会显示你的计算机上所有可（重）写光盘设备。 请记下你要用来刻写光盘的设备地址。 下面是一个运行 cdrecord -scanbus 后的输出范例：

Cdrecord 1.8 (i686-pc-linux-gnu) Copyright (C) 1995-2000 Jorg Schilling

Using libscg version schily-0.1

scsibus0:

0,0,0 0) *

0,1,0 1) *

0,2,0 2) *

0,3,0 3) HP CD-Writer+ 9200 1.0c Removable CD-ROM

0,4,0 4) *

0,5,0 5) *

0,6,0 6) *

0,7,0 7) *

要刻写在前面使用 mkisofs 创建的备份文件映像，切换成根用户，在 shell 提示下键入下列命令：

cdrecord -v -eject speed=4 dev=0,3,0 backup.iso

上面的命令把刻写速度设为 4 ，设备地址设为 0,3,0 ， 并把刻写输出设为 详细反馈（ verbose ） (-v) ，这对于跟踪刻写进程的状态有帮助。 -eject 参数 在刻写进程完毕后把光盘弹出。该命令还可以用来刻录从互联网上下载的 ISO 映像文件， 例如 Red Hat Linux ISO 映像。

你可以使用 cdrecord 来清除可重写光盘以便重新利用它， 方法是，键入以下命令：

cdrecord --dev=0,3,0 --blank=fast