### eject {#eject}

NAME

      eject - eject removable media

功能说明：退出抽取式设备。

语　　法：eject [-dfhnqrstv][-a &lt;开关&gt;][-c &lt;光驱编号&gt;][设备]

补充说明：若设备已挂入，则eject会先将该设备卸除再退出。

参　　数：

 [设备]   设备可以是驱动程序名称，也可以是挂入点。

 -a&lt;开关&gt;或--auto&lt;开关&gt;                #控制设备的自动退出功能。

 -c&lt;光驱编号&gt;或--changerslut&lt;光驱编号&gt;  #选择光驱柜中的光驱。

 -d或--default    #显示预设的设备，而不是实际执行动作。

 -f或--floppy     #退出抽取式磁盘。

 -h或--help       #显示帮助。

 -n或--noop       #显示指定的设备。

 -q或--tape       #退出磁带。

 -r或--cdrom      #退出光盘。

 -s或--scsi       #以SCSI指令来退出设备。

 -t或--trayclose  #关闭光盘的托盘

 -v或--verbose    #执行时，显示详细的说明。