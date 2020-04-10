### tzselect {#tzselect}

改变时区

[root@node ~]# tzselect

Please identify a location so that time zone rules can be set correctly.

Please select a continent or ocean.

1) Africa

2) Americas

3) Antarctica

4) Arctic Ocean

5) Asia

6) Atlantic Ocean

7) Australia

8) Europe

9) Indian Ocean

10) Pacific Ocean

11) none - I want to specify the time zone using the Posix TZ format.

#? 5

Please select a country.

1) Afghanistan           18) Israel                35) Palestine

2) Armenia               19) Japan                 36) Philippines

3) Azerbaijan            20) Jordan                37) Qatar

4) Bahrain               21) Kazakhstan            38) Russia

5) Bangladesh            22) Korea (North)         39) Saudi Arabia

6) Bhutan                23) Korea (South)         40) Singapore

7) Brunei                24) Kuwait                41) Sri Lanka

8) Cambodia              25) Kyrgyzstan            42) Syria

9) China                 26) Laos                  43) Taiwan

10) Cyprus                27) Lebanon               44) Tajikistan

11) East Timor            28) Macau                 45) Thailand

12) Georgia               29) Malaysia              46) Turkmenistan

13) Hong Kong             30) Mongolia              47) United Arab Emirates

14) India                 31) Myanmar (Burma)       48) Uzbekistan

15) Indonesia             32) Nepal                 49) Vietnam

16) Iran                  33) Oman                  50) Yemen

17) Iraq                  34) Pakistan

#? 9

Please select one of the following time zone regions.

1) east China - Beijing, Guangdong, Shanghai, etc.

2) Heilongjiang (except Mohe), Jilin

3) central China - Sichuan, Yunnan, Guangxi, Shaanxi, Guizhou, etc.

4) most of Tibet &amp; Xinjiang

5) west Tibet &amp; Xinjiang

#? 1

The following information has been given:

       China

       east China - Beijing, Guangdong, Shanghai, etc.

Therefore TZ=Asia/Shanghai will be used.

Local time is now:      Mon Nov 21 22:16:07 CST 2011.

Universal Time is now:  Mon Nov 21 14:16:07 UTC 2011.

Is the above information OK?

1) Yes

2) No

#? 1

You can make this change permanent for yourself by appending the line

       TZ=Asia/Shanghai; export TZ

to the file .profile in your home directory; then log out and log in again.

Here is that TZ value again, this time on standard output so that you

can use the /usr/bin/tzselect command in shell scripts:

Asia/Shanghai

+++++++++++++++

时区的配置文件是/etc/sysconfig/clock。用tzselect命令就可以修改这个配置文件，根据命令的提示进行修改就好了。（这是需要重启的）

重启以后需要查看是否已经修改好了如果没有成功就手动vi /etc/sysconfig/clock 为

[root@localhost ~]# more /etc/sysconfig/clock

ZONE=&quot;Asia/Shanghai&quot;

UTC=true

ARC=false

cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

然后最好使用下面的命令将更改写入bios。

hwclock

date [MMDDhhmm[[CC]YY][.ss]]

date  月日时分年

clock --localtime

clock --utc

　　Local vs. UTC

　　首先重要的问题是你使用utc还是local time.

　　UTC(Universal Time Coordinated)=GMT(Greenwich Mean Time)

　　Local time 是你手表上的时间

　　传统的POSIX计算机(Solaris,bsd,unix)使用UTC格式

　　linux可以处理UTC时间和蹩脚的Windows所使用的local time

　　到底是使用UTC还是local time可以这样来确定：

　　如果机器上同时安装有Linux和Windows，建议使用local time

　　如果机器上只安装有Linux，建议使用utc

　　确定后编辑/etc/sysconfig/clock, UTC=0 是local time; UTC=1 是UTC(GMT)

　　确定timezone

　　运行tzselect,回答问题后会告诉你时区的名称，比如&quot;Asia/Shanghai&quot;,把他记下来(后面我用$timezone代替)

　　设定timezone

　　# cp /usr/share/zoneinfo/$timezone /etc/localtime

　　重新启动或者运行时钟设置脚本使之发生作用

　　版本差异

　　由于发行版的差异，以上文件位置可能不同。

　　一般设置时钟所使用的启动脚本为/etc/rc.d/init.d/setclock

　　redhat是在/etc/rc.d/rc.sysinit中设置时钟，所以一般要重新启动。