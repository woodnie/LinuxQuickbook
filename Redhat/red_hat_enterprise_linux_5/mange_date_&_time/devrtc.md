### /dev/rtc {#dev-rtc}

1 ．时区的支持

（1）hwclock

为了查看硬件时钟是否为本地时间，运行命令hwclock -r。结果系统提示：&quot;Could not open RTC: No such file or directory&quot;，即找不到RTC文件。

（2）/dev/rtc

       由于内核在编译过程中没有提供RTC支持，需要重新编译内核（或为内核增加一个module）。方法为，在make menuconfig阶段，选中&quot;Character device&quot;的&quot;Enhanced Real Time Clock Support&quot;一项的支持。

为内核增加了该模块后，目录/proc/drive/下已出现了rtc文件，cat也能查看到正常的内容。但/dev/目录下仍没有rtc文件。

于是通过mknod命令在/dev目录下增加rtc文件。通过man rtc可以得知，RTC为只读字符设备，主10，从135。因此命令为&quot;mknod /dev/rtc c 10 135&quot;。命令执行完毕后，/dev下成功生成了rtc文件。

运行hwclock -r，能够看到硬件时间为本地时间。通过date命令查看系统时间，却为UTC时间。这说明系统没有进行本地时间的设置。

（3）/etc/localtime

       Linux的系统时区是通过符号连接/etc/localtime来得到的。可以通过tzset命令来设置时区。如果没有该命令，可以通过命令&quot;ln -s /etc/localtime /usr/share/zoneinfo/Asia/Shanghai&quot;来将时区设置为亚洲的上海。

由于最初构建系统的时候没有包含zoneinfo信息，因此/usr/share目录下不存在zoneinfo目录及其文件。所以将包含zoneinfo信息的机器的/usr/share目录下的整个zoneinfo目录复制到本机的/usr/share目录下。

通过date命令检查时间，发现已变成了正常的本地时间：

Mon Aug 29 13:14:29 CST 2005

（4）/etc/sysconfig/clock

       该配置文件可用来设置用户选择何种方式显示时间。如果硬件时钟为本地时间，则UTC设为0，并且不用设置环境变量TZ。如果硬件时钟为UTC时间，则要设置UTC为1，并设置环境变量TZ（或配置文件/etc/TZ）为时区信息，如&quot;Asia/Shanghai&quot;。

我机器的硬件时间为本地时间，因此该配置文件内容为：

ZONE=&quot;Asia/Shanghai&quot;

UTC=0

ARC=0