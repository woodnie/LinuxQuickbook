### date {#date}

date

[root@node ~]# system-config-date   # 图形化时间日期管理工具

[root@node ~]# date -s 091110      ##设置日期为2009年11月10日

[root@node ~]# date -s 20:53:00    ##设置时间为20:53:00

[root@node ~]# date -s &quot;12:12:23 2006-10-10

[root@node ~]# date +%Y%m%d

[root@node ~]# cp /etc/passwd ~/psaawd-$(date Alt-.)

[root@node ~]# cp /etc/passwd ~/psaawd-$(!date)      

CST：中国标准时间（China Standard Time），这个解释可能是针对RedHat Linux。

UTC：协调世界时，又称世界标准时间，简称UTC，从英文国际时间/法文协调时间&quot;Universal Time/Temps Cordonné&quot;而来。中国大陆、香港、澳门、台湾、蒙古国、新加坡、马来西亚、菲律宾、澳洲西部的时间与UTC的时差均为+8，也就是UTC+8。

GMT：格林尼治标准时间（旧译格林威治平均时间或格林威治标准时间；英语：Greenwich Mean Time，GMT）是指位于英国伦敦郊区的皇家格林尼治天文台的标准时间，因为本初子午线被定义在通过那里的经线。

设置完系统时间后,还需要同步到硬件时钟上

系统时钟和硬件时钟同步：

[root@node ~]# hwclock --systohc    # --systohc(SYStem clock to Hardware Clock)

[root@node ~]# clock --systohc        

硬件时钟与系统时钟同步：

[root@node ~]# hwclock --hctosys    # --systohc(SYStem clock to Hardware Clock)

[root@node ~]# clock --hctosys