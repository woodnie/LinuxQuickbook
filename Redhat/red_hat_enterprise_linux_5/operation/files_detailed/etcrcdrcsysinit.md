#### /etc/rc.d/rc.sysinit {#etc-rc-d-rc-sysinit}

wwwLinux 系统脚本分析之 rc.sysinit

--------------------------------------------------------------------------------

#!/bin/bash

#

# /etc/rc.d/rc.sysinit - run once at boot time

#

#

# Rerun ourselves through initlog                                                // 通过 /sbin/initlog 命令重新运行自己

if [ -z &quot;$IN_INITLOG&quot; -a -x /sbin/initlog ]; then                            // 条件是 ：如果 IN_INITLOG 变量的值不为空，且 /sbin/initlog 可执行

   exec /sbin/initlog -r /etc/rc.d/rc.sysinit                                // 调用 exec /sbin/initlog ， -r 是表示运行某个程序

fi

###########################################################################################################################

HOSTNAME=`/bin/hostname`                            # 取得主机名

HOSTTYPE=`uname -m`                                    # 取得主机类型

unamer=`uname -r`                                          # 取得内核的 release 版本（例如 2.4.9.30-8 ）

eval version=`echo $unamer | awk -F . { print &quot;(&quot; $1 &quot; &quot; $2 &quot;)&quot; }`            # 取得版本号

if [ -f /etc/sysconfig/network ]; then                # 如果存在 /etc/sysconfig/network ，则执行该文件。

   . /etc/sysconfig/network                             # network 文件主要控制是否启用网络、默认网关、主机名

fi

if [ -z &quot;$HOSTNAME&quot; -o &quot;$HOSTNAME&quot; = &quot;(none)&quot; ]; then            # 如果执行 network 文件后 HOSTNAME 为空或者为 &quot;(none)&quot; ，

   HOSTNAME=localhost                                                        # 则将主机名设置为 &quot;localhost&quot;

fi

# Mount /proc and /sys (done here so volume labels can work with fsck)        # 接下来是挂载 /proc 和 /sys ，这样 fsck 才能使用卷标

mount -n -t proc /proc /proc                                                                      #  -n 表示不写 /etc/mtab ，这在 /etc 所在的文件系统为只读时用。因为此时的 / 还是只读的

[ -d /proc/bus/usb ] &amp;&amp; mount -n -t usbfs /proc/bus/usb /proc/bus/usb        # 如果存在 /proc/bus/usb 目录则把 /proc/bus/usb 以 usbfs 挂载到 /proc/bus/usb 下

mount -n -t sysfs /sys /sys &gt;/dev/null 2&gt;&amp;1                                                    # 接下来就是把 /sys 目录以 sysfs 格式挂载到 /sys 目录下

##########################################################################################################################

. /etc/init.d/functions             # 执行 /etc/init.d/functions 文件，该文件提供了很多有用的函数 , 具体见 &quot; functions 脚本提供的函数&quot;一文

#################################################################################################################

# Check SELinux status                                                      

selinuxfs=`awk / selinuxfs / { print $2 } /proc/mounts`        

SELINUX=                                                                                                  

if [ -n &quot;$selinuxfs&quot; ] &amp;&amp; [ &quot;`cat /proc/self/attr/current`&quot; != &quot;kernel&quot; ]; then          

if [ -r $selinuxfs/enforce ] ; then

 SELINUX=`cat $selinuxfs/enforce`

else

 # assume enforcing if you cant read it

 SELINUX=1

fi

fi

if [ -x /sbin/restorecon ] &amp;&amp; LC_ALL=C fgrep -q &quot; /dev &quot; /proc/mounts ; then

/sbin/restorecon  -R /dev 2&gt;/dev/null

fi

disable_selinux() {

echo &quot;*** Warning -- SELinux is active&quot;

echo &quot;*** Disabling security enforcement for system recovery.&quot;

echo &quot;*** Run setenforce 1 to reenable.&quot;

echo &quot;0&quot; &gt; $selinuxfs/enforce

}

relabel_selinux() {

   if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then

chvt 1

   fi

   echo &quot;

        *** Warning -- SELinux relabel is required. ***

 *** Disabling security enforcement.         ***

 *** Relabeling could take a very long time, ***

 *** depending on file system size.          ***

 &quot;

   echo &quot;0&quot; &gt; $selinuxfs/enforce

   /sbin/fixfiles -F relabel &gt; /dev/null 2&gt;&amp;1

   rm -f  /.autorelabel

   echo &quot;*** Enabling security enforcement.         ***&quot;

   echo $SELINUX &gt; $selinuxfs/enforce

   if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then

chvt 8

   fi

}

###############################################################################################################################

if [ &quot;$HOSTTYPE&quot; != &quot;s390&quot; -a &quot;$HOSTTYPE&quot; != &quot;s390x&quot; ]; then

 last=0

 for i in `LC_ALL=C grep ^[0-9].*respawn:/sbin/mingetty /etc/inittab | sed s/^.* tty\([0-9][0-9]*\).*/\1/g`; do

       &gt; /dev/tty$i

       last=$i

 done

 if [ $last -gt 0 ]; then

      &gt; /dev/tty$((last+1))

      &gt; /dev/tty$((last+2))

 fi

fi

#######################################################################################################################

if [ &quot;$CONSOLETYPE&quot; = &quot;vt&quot; -a -x /sbin/setsysfont ]; then            # 下面是设置屏幕的默认字体。如果 CONSOLETYPE 变量的值为 vt 且 /sbin/setsysfont 命令可执行

  echo -n &quot;Setting default font ($SYSFONT): &quot;                            # 打印 &quot;setting deafault font xxxx &quot; , 默认字体应该是 xxxx

  /sbin/setsysfont                                                                   # 执行 /sbin/setsysfont

  if [ $? -eq 0 ]; then                                                                # 如果上述命令执行返回的 exit status 为 0

     success                                                                                # 则调用 success 函数（来自于 functions 脚本），记录一个成功的事件

  else

     failure                                                                                 # 否则调用 failure 函数

  fi

  echo ; echo                                              

fi

#################################################################################################################################

# Print a text banner.                                                                # 下面部分是打印 &quot;welcome to xxxxx&quot; 的标题栏

echo -en $&quot;\t\tWelcome to &quot;                                                      # 打印 &quot;&lt;tab&gt;&lt;tab&gt;Welcom to&quot; ，同时不换行

if LC_ALL=C fgrep -q &quot;Red Hat&quot; /etc/redhat-release ; then           # 从 /etc/redhat-release 文件中找出含有 &quot;Red Hat&quot; 的行，如果找到

[ &quot;$BOOTUP&quot; = &quot;color&quot; ] &amp;&amp; echo -en &quot; \\033[0;31m &quot;                        # 则变量 BOOTUP 的值为 color ，并设置输出字体输出红色

echo -en &quot;Red Hat&quot;                                                                    # 同时打印 &quot;Red Hat&quot; ，接下来打印发行版本（产品）

[ &quot;$BOOTUP&quot; = &quot;color&quot; ] &amp;&amp; echo -en &quot; \\033[0;39m &quot;                        #   如果变量 BOOTUP 的值为 color 则设置输出字体为白色

PRODUCT=`sed &quot;s/Red Hat \(.*\) release.*/\1/&quot; /etc/redhat-release`    # 从 /etc/redhat-release 中找出含有 &quot;Red Hat&quot; 且后面若干字符，然后是 &quot;release&quot; 的行，并截取中间部分给 PRODUCT

echo &quot; $PRODUCT&quot;                                                                            #   输出变量 PRODUCT 的值（白色）

elif LC_ALL=C fgrep -q &quot;Fedora&quot; /etc/redhat-release ; then             # 如果 /etc/redhat-release 中没有 Red Hat 字符串，但有 Fedora ，则执行类似过程

[ &quot;$BOOTUP&quot; = &quot;color&quot; ] &amp;&amp; echo -en &quot; \\033[0;31m &quot;

echo -en &quot;Fedora&quot;

[ &quot;$BOOTUP&quot; = &quot;color&quot; ] &amp;&amp; echo -en &quot; \\033[0;39m &quot;

PRODUCT=`sed &quot;s/Fedora \(.*\) release.*/\1/&quot; /etc/redhat-release`

echo &quot; $PRODUCT&quot;

else                                                                                            # 如果 /etc/redhat-release 中既没有含 Red Hat 也没有含 Fedora 的行，则

PRODUCT=`sed &quot;s/ release.*//g&quot; /etc/redhat-release`                    # 找到含有 release 的行，并把它前面的部分输出，作为 PRODUCT 变量的值并输出

echo &quot;$PRODUCT&quot;

fi

# 补充 ：实际效果是 Red Hat 两个字是红色，其他都是白色

###############################################################################################################################

if [ &quot;$PROMPT&quot; != &quot;no&quot; ]; then                                                        # 如果变量 PROMPT 的值不为 &quot;no&quot; （表示允许交互启动），则

echo -en $&quot;\t\tPress I to enter interactive startup.&quot;                            # 打印提示信息&quot; Press I to enter interactive startup &quot;，但此时按 I 还未起作用

echo  

fi

######################################################################################################################

# 注释 ：下面部分是设置输出到 console 的日志的详细级别

# Fix console loglevel                                                                  # 设置控制台的日志级别

if [ -n &quot;$LOGLEVEL&quot; ]; then                                                            # 如果 LOGLEVEL 变量的值不为空

/bin/dmesg -n $LOGLEVEL                                                                # 则执行 dmesg ，设置打印到 consoel 的日志的级别为 $LOGLEVEL

fi

##############################################################################################################################

# 注释 ：下面部分是启动 udev 并加载 ide 、 scsi 、 network 、 audio 以及其他类型的设备的模块的部分

[ -x /sbin/start_udev ] &amp;&amp; /sbin/start_udev                                    # 如果 /sbin/start_udev 可执行，则执行它，会在屏幕上显示 &quot; Starting udev ... [OK] &quot;

# Only read this once.

cmdline=$(cat /proc/cmdline)                                                        # 读取 /proc/cmdline ，这是内核启动的时的参数，赋予变量 cmdline

# Initialize hardware                                                                     # 下面初始化硬件

if [ -f /proc/sys/kernel/modprobe ]; then                                        # 如果 /proc/sys/kernel/modprobe 文件存在（该文件告诉内核用什么命令来加载模块），则

  if ! strstr &quot;$cmdline&quot; nomodules &amp;&amp; [ -f /proc/modules ] ; then            # 如果 $cmdline 变量的值含有 nomodules ，且存在 /proc/modules 文件，则

      sysctl -w kernel.modprobe=&quot;/sbin/modprobe&quot; &gt;/dev/null 2&gt;&amp;1            # 使用 sysctl 设置 kernel.modprobe 为 /sbin/modprobe 命令

      sysctl -w kernel.hotplug=&quot;/sbin/hotplug&quot; &gt;/dev/null 2&gt;&amp;1                    # 使用 sysctl 设置  kernel.hotplug 为 /sbin/hotplug 命令

  else                                                                                           # 如果不存在 /proc/sys/kernel/modprobe 文件，则                            

      # We used to set this to NULL, but that causes failed to exec messages&quot;  

      sysctl -w kernel.modprobe=&quot;/bin/true&quot; &gt;/dev/null 2&gt;&amp;1                # 使用 sysctl 设置 kernel.modprobe 为 /bin/true

      sysctl -w kernel.hotplug=&quot;/bin/true&quot; &gt;/dev/null 2&gt;&amp;1                    # kernel.hotplug 也一样

  fi

fi

##################################################################################################################################

# 注释 ：下面开始真正的加载各种类型的设备的驱动

# 首先是找出那些模块需要被加载，并把模块名按类分别放到 ide 、 scsi 、 network 、 audio 、 other 5 个变量中

echo -n $&quot;Initializing hardware... &quot;                                                    # 打印 &quot;initalizing hardware..&quot; （不换行），并开始初始化硬件

ide=&quot;&quot;                                                                                              # 下面这些变量都是为了存储的型号，格式为 &quot; &lt;module1&gt; &lt;module2&gt; &lt;module3&gt; ...&quot;          

scsi=&quot;&quot;

network=&quot;&quot;

audio=&quot;&quot;

other=&quot;&quot;

eval `kmodule | while read devtype mod ; do                                # 从 kmodule 命令的输出一次读入两个变量 devtype （设备类型）和 mod （模块名）

case &quot;$devtype&quot; in                                                                            # 根据 devtype 做出适当的选择

 &quot;IDE&quot;) ide=&quot;$ide $mod&quot;                                                                            # 如果 devtype 的值为 IDE ，则把 mod 加入到现有的 ide 变量的值中

    echo &quot;ide=\&quot;$ide&quot;\&quot;;;                                                                            # 输出 &quot;ide=&quot; 然后是变量 ide 的值

 &quot;SCSI&quot;) scsi=&quot;$scsi $mod&quot;                                                                          # 如果 devtype 的值为 SCSI ，则把 mod 变量的值加入到 scsi 变量的值中

    echo &quot;scsi=\&quot;$scsi&quot;\&quot;;;                                                                           # 输出 &quot;scsi=&quot; 再输出 $scsi

 &quot;NETWORK&quot;) network=&quot;$network $mod&quot;                                                    # 下面的 NETWORK 和 AUDIO 也一样

    echo &quot;network=\&quot;$network&quot;\&quot;;;

 &quot;AUDIO&quot;) audio=&quot;$audio $mod&quot;

    echo &quot;audio=\&quot;$audio&quot;\&quot;;;

 *) other=&quot;$other $mod&quot;                                                                            # 如果是属于 other 类型的则把模块名 $mod 加入到 other 变量的值列表中

    echo &quot;other=\&quot;$other&quot;\&quot;;;

esac

done`

load_module () {                    # 定义一个函数名为 load_module

LC_ALL=C fgrep -xq &quot;$1&quot; /etc/hotplug/blacklist 2&gt;/dev/null || modprobe $1 &gt;/dev/null 2&gt;&amp;1        # 在 /etc/hotplug/blacklist 文件中查找是否有指定的模块，如果没有，

}                                                                                                                                            # 再用 modprobe 加载指定的模块

# IDE                                        # 下面开始加载所有 IDE 类型的设备的模块

for module in $ide ; do                 # 从 ide 变量中每次取出 1 个值，赋予变量 module

load_module $module                 # 再用 load_module 函数加载它。如果该模块存在于 /etc/hotplug/blacklist ，则不会被加载（因为存在于黑名单中）

done

# SCSI                                    # SCSI 方面的比较特殊，除了加载 scsi 变量中的模块外，还会从 modprobe -c ( 显示所有配置）中找出 scsi 卡（ hostadapter ）的模块

for module in `/sbin/modprobe -c | awk /^alias[[:space:]]+scsi_hostadapter[[:space:]]/ { print $3 }` $scsi; do   # 从 modprobe -c 中找出所有 scsi 卡的模块别名，

load_module $module             # 并和 scsi 变量一起，通过 for 循环一一加载

done

load_module floppy                # 然后加载 floppy 模块

echo -n $&quot; storage&quot;                # 输出 &quot;storage&quot; , 表示存储方面的模块加载已经完成

# Network                            # 接下来是加载网络设备模块

pushd /etc/sysconfig/network-scripts &gt;/dev/null 2&gt;&amp;1        # pushd 是一个 bash 的内建命令，把目录名放入目录堆栈的顶部，并进入指定目录

interfaces=`ls ifcfg* | LC_ALL=C egrep -v (ifcfg-lo|:|rpmsave|rpmorig|rpmnew) | \        # 找出 network-scripts 目录下所有 lscfg 开头的文件名，排除指定备份和 loopback

           LC_ALL=C egrep -v (~|\.bak)$ | \                                                                    # 排除以 ~ 和 .bask 结尾的文件名

           LC_ALL=C egrep ifcfg-[A-Za-z0-9\._-]+$ | \                                                       # 找出所有以 ifcfg- 开头，并以多个字母 / 数字结尾的文件名

    sed s/^ifcfg-//g |                                                                                                  # 把前缀 ifcfg- 去掉，只留下后面的接口名

    sed s/[0-9]/ &amp;/ | LC_ALL=C sort -k 1,1 -k 2n | sed s/ //`                                         # 在数字前面加上空格，是类型和编号分离，便于 sort 排序，然后再组合回去

                                                                                                                                # 目的是按顺序启动接口

for i in $interfaces ; do                                                                                                # 对于在 $interfaces 变量中的每个接口

eval $(LC_ALL=C fgrep &quot;DEVICE=&quot; ifcfg-$i)                                                                     # 从对应的文件 ifcfg-$i 找出 DEVICE= 行

load_module $DEVICE                                                                                                 # 然后用 load_module 加载它，注意， DEVICE= 行可以是别名，例如 eth1.n7css

done

popd &gt;/dev/null 2&gt;&amp;1                                                                                                 # 把 network-scripts 从目录名堆栈中弹出，并返回原来的目录

for module in $network ; do                                                                                        #   剩下还有 network 变量中的模块

load_module $module                                                                                                # 也一一加载

done

echo -n $&quot; network&quot;                                                                                                    # 输出 network 字符串，表示加载网络设备模块工作完毕

# Sound

for module in `/sbin/modprobe -c | awk /^alias[[:space:]]+snd-card-[[:digit:]]+[[:space:]]/ { print $3 }` $audio; do    # 和 scsi 一样，加载在 modprobe -c 和 audio 变量

load_module $module                                                                                                                                          # 中的模块

done

echo -n $&quot; audio&quot;                                                                                                        # 输出 audio ，表示声卡设备驱动加载完毕，一般会有多个驱动被加载      

# Everything else (duck and cover)                                                                             # 剩下的就是 other 类型的设备了

for module in $other ; do

load_module $module                                                                                                # 这个直接用 load_moudle 函数加载了

done

echo -n $&quot; done&quot;                                                                                                        # 输出 done 表示完成

success                                                                                                                    # 调用 success 函数，记录该事件

echo

########################################

# 注释 ：下面是启用 Software-RAID 的检测

echo &quot;raidautorun /dev/md0&quot; | nash --quiet       # 用 nash （小型脚本解释器）执行 &quot;raidautorun /dev/md0&quot; 命令，对所有 paritition ID 为 0xFD 分区进行检查

#################################################################################################################################

# Start the graphical boot, if necessary; /usr may not be mounted yet, so we             # 显示图形模式的启动画面，但由于 rhgb 命令在 /usr 分区上，

# may have to do this again after mounting                                                             #   这时 /usr 分区还没有被挂载，所以在挂载后重新做一遍这部分脚本

RHGB_STARTED=0           # 初始化 RHGB_STARTED 变量的值为 0 ，表示尚未启动。 RHGB 是 redhat graphical boot 的含义，

mount -n /dev/pts         # 挂载 /dev/pts ，类型是 devpts ，目的是获得 pty ，这是一种伪文件系统

if strstr &quot;$cmdline&quot; rhgb &amp;&amp; [ &quot;$BOOTUP&quot; = &quot;color&quot; -a &quot;$GRAPHICAL&quot; = &quot;yes&quot; -a -x /usr/bin/rhgb ]; then        # 如果内核启动参数 $cmdline 含有 rhgb ，且

                                                                                                                                                       # BOOTUP 变量的值为 color ，且 GRPAHICAL 变量的值为 yes

                                                                                                                                                       # 且 /usr/bin/rhgb 文件存在并可执行

  LC_MESSAGES= /usr/bin/rhgb                                                                                                           # 把 &quot;/usr/sbin/rhgb&quot; 赋予变量 LC_MESSAGES

  RHGB_STARTED=1                                                                                                                            # RHGB_STARTED 变量的值为 1 。

fi

##################################################################################################################

# 注释 ：下面部分是使用 sysctl 设置内核的参数

# Configure kernel parameters                  

update_boot_stage RCkernelparam             # 调用 update_boot_stage 函数，该函数由 /etc/rc.d/init.d/functions 脚本定义，主要执行 &quot;/usr/sbin/rhgb-client --update $1&quot;

action $&quot;Configuring kernel parameters: &quot; sysctl -e -p /etc/sysctl.conf    # 调用 action 函数，执行 sysctl 命令，它读取 /etc/sysctl.conf ，同时输出 &quot; Configuring kernel

                                                                                                      # parameters &quot; 字符串 ,action 函数也是由 /etc/rc.d/init.d/functions 脚本定义

# 补充 ： action 函数的作用是把第一个参数 $1 赋予变量 string 并输出到屏幕，同时把该字符串也写入 /etc/rhgb/temp/rhgb-console

# 然后把 $1 后面的部分作为命令交给 initlog 执行，并根据结果调用 success 或者 failure 函数。而 success 或者 failure 函数又会根据结果输出 [ OK ] 或者 [ FAILED ] 。

# 除了 success 和 failure 函数外， functions 脚本还定义了 passed 和 warnning 函数，它们又分别调用 echo_passwd （） 和 echo_warnning （）函数，

# 以输出 [ PASSED ] 和 [ WARNNING ]

#############################################################################################################

# 注释 ：下面设置系统的硬件时钟

# Set the system clock.                    # 接下来设置系统时钟

update_boot_stage RCclock                # 同样是告诉 update_boot_stage 该事件（执行 /usr/bin/rhgb-client --update=&quot;$1&quot; 命令，这里 $1 就是 &quot;RCclock&quot; ）

ARC=0                                              

SRM=0

UTC=0

if [ -f /etc/sysconfig/clock ]; then        # 如果存在 /etc/sysconfig/clock 则执行该文件

  . /etc/sysconfig/clock

  # convert old style clock config to new values

  if [ &quot;${CLOCKMODE}&quot; = &quot;GMT&quot; ]; then                        # 如果变量 CLOCKMODE 为旧式的 GMT 格式

     UTC=true                                                                # 则 UTC 变量的值为 ture

  elif [ &quot;${CLOCKMODE}&quot; = &quot;ARC&quot; ]; then                     # 如果 CLOCKMODE 的值是 ARC ，则

     ARC=true                                                                # ARC 的值为 true

  fi                                                                         # 如果 CLOCKMODE 不等于 GMT 或者 UTC ，则 UTC 的值等于 /etc/sysconfig/clock 中的值，一般都是 false

fi                                                                            # 如果 CLOCKMODE 不等于 GMT 后者 ARC ，则 UTC 的值为 0

CLOCKDEF=&quot;&quot;                                                            # CLOCKDEF 变量的值默认为空

CLOCKFLAGS=&quot;$CLOCKFLAGS --hctosys&quot;                       # 而 CLOCKFLAGS 变量的值为原来的值（可能为空）加上 --hctosys ，表示把硬件时钟写入内核时钟

case &quot;$UTC&quot; in                                                          # 根据 UTC 变量的值进行选择                                            

   yes|true) CLOCKFLAGS=&quot;$CLOCKFLAGS --utc&quot;             # 如果 UTC 的值为 yes 或者 true ，则变量 CLOCKFLAGS 的值加上 --utc ，表示采用 UTC 时区

 CLOCKDEF=&quot;$CLOCKDEF (utc)&quot; ;;                                     # 同时变量 CLOCKDEF 的值加上 &quot;(utc)&quot;

   no|false) CLOCKFLAGS=&quot;$CLOCKFLAGS --localtime&quot;     # 如果变量 UTC 的值为 false 或者 no ，则

 CLOCKDEF=&quot;$CLOCKDEF (localtime)&quot; ;;                        # CLOCKDEF 的值加上 &quot;(localtime)&quot; 。由于安装 OS 时没有选择 UTC ， CLOCKDEF 的值一般最后就是 (localtime) 而已

esac

case &quot;$ARC&quot; in                                                          # 根据 ARC 变量的值来选择            

   yes|true) CLOCKFLAGS=&quot;$CLOCKFLAGS --arc&quot;            # 如果是 yes 或者 true ，则 CLOCKFLAGS 的值加上 --arc ， CLOCKDEF 的值加上 (arc)

 CLOCKDEF=&quot;$CLOCKDEF (arc)&quot; ;;                                # 如果 ARC 的值为 0 ，则 CLOCKFLAGS 和 CLOCKDEF 的值都不变

esac

case &quot;$SRM&quot; in                                                         # 如果 SRM 的值是 yes 或者 true ，则 CLOCKFLAGS 的值加上 &quot;--srm&quot; ， CLOCKDEF 加上 &quot;(srm)&quot;

   yes|true) CLOCKFLAGS=&quot;$CLOCKFLAGS --srm&quot;

 CLOCKDEF=&quot;$CLOCKDEF (srm)&quot; ;;

esac

/sbin/hwclock $CLOCKFLAGS                    # 执行 hwclock $CLOCKFLAGS 命令，一般就是 /sbin/hwclock --hctosys

action $&quot;Setting clock $CLOCKDEF: `date`&quot; date        # 并输出 &quot;setting clock &quot; 和 $CLOCKDEF ，并执行 date 输出当前时间，然后输出 [ OK ]

##################################################################################################

# 注释 ：下面部分是设置 key map

if [ &quot;$CONSOLETYPE&quot; = &quot;vt&quot; -a -x /bin/loadkeys ]; then            # 接下来是设置 keymap ，一般都是 us

KEYTABLE=                                                                        #   设置 KEYTABLE 和 KEYMAP

KEYMAP=

if [ -f /etc/sysconfig/console/default.kmap ]; then                # 假如存在 /etc/sysconfig/console/default.kmap ，则

 KEYMAP=/etc/sysconfig/console/default.kmap                            # KEYMAP 变量的值为 /etc/sysconfig/console/default.kmap

else                                                                                    # 否则

 if [ -f /etc/sysconfig/keyboard ]; then                                        # 如果存在 /etc/sysconfig/keyboard 文件，则执行它

   . /etc/sysconfig/keyboard                                                       # 该文件设置 KEYTABLE 变量的值 , 例如 KEYTABLE=&quot;/usr/lib/kbd/keytables/us.map

 fi

 if [ -n &quot;$KEYTABLE&quot; -a -d &quot;/lib/kbd/keymaps&quot; ]; then              # 如果 KEYTABLE 的值不为空，且存在 /lib/kbd/keymaps/ 目录，则

    KEYMAP=&quot;$KEYTABLE.map&quot;                                                # KEYMAP 的值为 KEYTABLE 的值加上 &quot;.map&quot;

 fi

fi

if [ -n &quot;$KEYMAP&quot; ]; then                                                                 # 假如 KEYMAP 变量的值不为空（间接表示 KEYTABLE 不为空且存在 keymaps 目录）

 # Since this takes in/output from stdin/out, we cant use initlog

 if [ -n &quot;$KEYTABLE&quot; ]; then                                                                    # 且 KEYTABLE 的值不为空

   echo -n $&quot;Loading default keymap ($KEYTABLE): &quot;                                     # 则输出 &quot;Loading default keymap xxxx&quot;

 else

   echo -n $&quot;Loading default keymap: &quot;                                                    # 否则只输出 Loading default keymap

 fi  

 loadkeys $KEYMAP &lt; /dev/tty0 &gt; /dev/tty0 2&gt;/dev/null &amp;&amp; \            # 接下来用 loadkeys 加载 $KEYMAP 指定的键盘映射文件。

    success $&quot;Loading default keymap&quot; || failure $&quot;Loading default keymap&quot;        # 如果成功则调用 success 函数，否则调用 failure 函数

 echo

fi

fi

#######################################################

# 注释 ：下面部分是设置主机名

# Set the hostname.                                # 接下来是设置主机名

update_boot_stage RChostname                # 告诉 update_boot_stage 函数该事件，以执行 /usr/sbin/rhgb-client --update RChostname 命令

action $&quot;Setting hostname ${HOSTNAME}: &quot; hostname ${HOSTNAME}        # 打印 &quot;setting hostname xxxx&quot; 字符串，并执行 hostname ${HOSTNAME} 设置主机名

                                                                #   注意， HOSTNAME 变量前面已经有过赋值的了，就是 `/bin/hostname` 命令返回的值

##################################################################################################################################

# 注释 ：下面设置 ACPI 部分

# Initialiaze ACPI bits

if [ -d /proc/acpi ]; then                                                                    # 如果存在 /proc/acpi 目录，则

  for module in /lib/modules/$unamer/kernel/drivers/acpi/* ; do            # 对于在 /lib/modules/&lt;kernel-version&gt;/kernel/drivers/acpi/ 目录下的所有模块文件，

     insmod $module &gt;/dev/null 2&gt;&amp;1                                                        # 都用 insmod 命令加载到内核中

  done

fi

########################################################################################################################

# 注释 ：下面是主要的部分，就是确认是否需要对 / 文件系统进行 fsck

if [ -f /fastboot ] || strstr &quot;$cmdline&quot; fastboot ; then      # 接下来是看对 / 系统进行 fsck 的时候了。如果不存在 /fastboot 文件则看 cmdline 变量是否含有 fastboot 字符串

fastboot=yes                                                              # 如果有，则 fastboot 变量的值为 yes 。 /fastboot 是由 shutdown -f 创建的，表示重启时不作 fsck

fi

if [ -f /fsckoptions ]; then                                            # 如果存在 /fsckoptions 文件，则把文件的内容赋予变量 fsckoptions 变量

fsckoptions=`cat /fsckoptions`

fi

if [ -f /forcefsck ] || strstr &quot;$cmdline&quot; forcefsck ; then        # 如果不存在 /forcefsck 文件，则检查 cmdline 变量是否含有 forcefsck 字符串，如果有

fsckoptions=&quot;-f $fsckoptions&quot;                                            # 则在 fsckoptions 的值前面加上 &quot;-f&quot; 。 /forcefsck 是由 shutdown -F 创建的，强制 fsck

elif [ -f /.autofsck ]; then                                                 # 如果存在 /.autofsck

     if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then        #   且文件 /usr/bin/rhgb-clinet 可执行，且 /usr/bin/rhgb 服务在运行 (--pnig) ，则

          chvt 1                                                                                                #   切换到虚拟控制台 #1 （主要是显示 fsck 的信息用）

      fi

      echo $&quot;Your system appears to have shut down uncleanly&quot;                            # 并显示需要 fsck 的信息 &quot; You system appears to have shut down uncleanly &quot;

      AUTOFSCK_TIMEOUT=5                                                                              # 设置超时时间为 5 秒

     [ -f /etc/sysconfig/autofsck ] &amp;&amp; . /etc/sysconfig/autofsck                           # 如果存在 /etc/sysconfig/autofsck 则执行它

     if [ &quot;$AUTOFSCK_DEF_CHECK&quot; = &quot;yes&quot; ]; then                                                 # 如果执行该文件后 AUTOFSCK_DEF_CHECK 变量的值为 yes ，则

         AUTOFSCK_OPT=-f                                                                                        # 变量 AUTOFSCK_OPT 的值为 &quot;-f&quot;

     fi

     if [ &quot;$PROMPT&quot; != &quot;no&quot; ]; then                                                                // 如果 PROMPT 的值不为 no ，则

       if [ &quot;$AUTOFSCK_DEF_CHECK&quot; = &quot;yes&quot; ]; then                                                // 且 AUTOFSCK_DEF_CHECK 的值为 yes

          if /sbin/getkey -c $AUTOFSCK_TIMEOUT -m $&quot;Press N within %d seconds to not force file system integrity check...&quot; n ; then    //   执行 getkey 命令，超时 5 秒，并

                                                                                                                                                                                      // 提示 5 秒内按下 n 键可跳过 fsck

             AUTOFSCK_OPT=        // 如果用户按下 n ，则 AUTOFSCK_OPT 的值为空 , 表示取消自动 fsck

          fi

       else                    // 如果 AUTOFSCK_DEF_CHECK 的值不为 yes ，则提示 5 秒内按 y 可以强制 fsck

         if /sbin/getkey -c $AUTOFSCK_TIMEOUT -m $&quot;Press Y within %d seconds to force file system integrity check...&quot; y ; then        // 同样是用 getkey 捕捉 y 键

             AUTOFSCK_OPT=-f        // 如果用户按下了 y 键，则 AUTOFSCK_OPT 的值为 &quot;-f&quot;

         fi

      fi

      echo

     else         // 如果 PROMPT 的值为 &quot;no&quot; ， 这是用户无法选择是否 fsck

       # PROMPT not allowed

       if [ &quot;$AUTOFSCK_DEF_CHECK&quot; = &quot;yes&quot; ]; then                                                # 取决于 AUTOFSCK_DEF_CHECK 变量的值，如果是 yes ，则

         echo $&quot;Forcing file system integrity check due to default setting&quot;     # 提示由于默认的设置强制 fsck 。可能是 mount 次数达到限制，或者多长时间没有 fsck 了

       else                                          

         echo $&quot;Not forcing file system integrity check due to default setting&quot;            # 如果 AUTOFSCK_DEF_CHECK 的值为 no ，则表示不做 fsck

       fi

     fi

     fsckoptions=&quot;$AUTOFSCK_OPT $fsckoptions&quot;

fi

# 注释 ：注意！到这里为止并没有执行 fsck 操作，只是判断是否需要执行 fsck 以及 fsck 命令的选项

if [ &quot;$BOOTUP&quot; = &quot;color&quot; ]; then       # 如果 BOOTUP 的值为 color ，则

fsckoptions=&quot;-C $fsckoptions&quot;            # -C 表示显示 fsck 进度信息

else                                              # 否则

fsckoptions=&quot;-V $fsckoptions&quot;            # -V 表示显示 verbose 信息

fi

if [ -f /etc/sysconfig/readonly-root ]; then            # 如果存在 /etc/sysconfig/readonly-root ，则

   . /etc/sysconfig/readonly-root                            # 执行该文件

   if [ &quot;$READONLY&quot; = &quot;yes&quot; ]; then                            # 如果 READONLY 变量的值为 yes ，则

       # Call rc.readonly to set up magic stuff needed for readonly root        # 执行 /etc/rc.readonly 脚本

       . /etc/rc.readonly

   fi

fi

###########################################################################################################

# 注释 ：下面开始判断 / 文件系统的类型（是本地还是 nfs 格式）。如果是 nfs 则不执行 fsck ，否则继续

_RUN_QUOTACHECK=0                                            # 初始化 RUN_QUOTACHECK 的值为 0\. 这里是作一个标记，因为后面的代码可能会导致重启。在这里做好标记

                                                                           # 如果后面没有重启，就可以把 _RUN_QUOTACHECK 设置为 1

ROOTFSTYPE=`awk / \/ / &amp;&amp; ($3 !~ /rootfs/) { print $3 } /proc/mounts`        # 从 /proc/mounts 中找出 / 文件系统的类型，并赋予变量 ROOTFSTYPE

# 注释 ：下面用到 -z &quot;$fastboot&quot; ，如果有 /fastboot ，则 fastboot 的值为 yes ， 所以就不合 if 的条件，则跳过 fsck 。这就是 shutdown -f 快速启动的效果

if [ -z &quot;$fastboot&quot; -a &quot;$READONLY&quot; != &quot;yes&quot; -a &quot;X$ROOTFSTYPE&quot; != &quot;Xnfs&quot; -a &quot;X$ROOTFSTYPE&quot; != &quot;Xnfs4&quot; ]; then     # 如果 fastboot 变量的值为空且  READONLY 变量不为 yes

                                                                                                                                                               # 且 / 文件系统的类型不是 nfs 或者 nfs4 ，则

       STRING=$&quot;Checking root filesystem&quot;    # 初始化 string 变量并输出 &quot;checking root filesystem&quot;

       echo $STRING

       rootdev=`awk / \/ / &amp;&amp; ($3 !~ /rootfs/) {print $1} /proc/mounts`        # 从 /proc/mounts 中找出 / 文件系统所在的设备，并赋予 rootdev 变量（例如 /dev/hdb2 ）

        if [ -b /initrd/&quot;$rootdev&quot; ] ; then        # 如果 /initrd/$rootdev 是一个 block 设备

             rootdev=/initrd/&quot;$rootdev&quot;                    # 则 rootdev 就是 /initrd/rootdev

        else                                                 # 否则 rootdev 就是 / ，正常情况应该是执行该步的，也就是 rootdev 最终为 /

        rootdev=/                                       # rootdev 的值为 / ，因为 fsck 支持用 LABEL 、 mount point 、 device name 来指定要检查的文件系统

        fi

       # 注释 ： 下面开始真正的 fsck 操作

        if [ &quot;${RHGB_STARTED}&quot; != &quot;0&quot; -a -w /etc/rhgb/temp/rhgb-console ]; then        # 如果 RHGB_STARTED 不为 0 且 /etc/rhgb/tmp/rhgb-console 文件可写，则

             fsck -T -a $rootdev $fsckoptions &gt; /etc/rhgb/temp/rhgb-console           # 执行 fsck 命令， -T 表示不打印标题栏， -a 表示自动修复错误，设备是 / ，后面是选项

                                                                                                                     # 且信息写入 /etc/rhgb/temp/rhgb-console

        else                                                                                                           # 否则

             initlog -c &quot;fsck -T -a $rootdev $fsckoptions&quot;                                                        # 调用 initlog 执行 fsck ，信息直接输出到屏幕， fsck 命令本身都是一样的。

       fi                                                                                                                    

       rc=$?                                    # 把 fsck 的结果送给变量 rc

       if [ &quot;$rc&quot; -eq &quot;0&quot; ]; then         # 如果 rc 的值为 0 ，表示 fsck 成功通过

             success &quot;$STRING&quot;                    # 则调用 success 打印 &quot;checking root fileysyes      [OK]&quot;

         echo

       elif [ &quot;$rc&quot; -eq &quot;1&quot; ]; then        # 如果 rc 为 1 ，则打印 passed ，表示有错误但修复

            passed &quot;$STRING&quot;                # 这时调用的是 passed 函数（由 /etc/rc.d/init.d/functions 脚本定义，输出 &quot;checking root filesystems     [ PASSED ]&quot;)

            echo

        elif [ &quot;$rc&quot; -eq &quot;2&quot; -o &quot;$rc&quot; -eq &quot;3&quot; ]; then         # 如果 rc 为 2 （表示系统应该重启）或者为 3 （为 1+2 ，表示系统错误已修复，但需要重启）

             echo $&quot;Unmounting file systems&quot;                        # 提示卸载 / 文件系统

             umount -a                                                        # 把所有卸载，当然 / 除外

             mount -n -o remount,ro /                                  # 把 / 设备已 ro 模式重新 mount

             echo $&quot;Automatic reboot in progress.&quot;                # 然后提示将要自动重启了

             reboot -f                                                         # 在这里执行 reboot -f ，强制重启

         fi     # 这个 fi 是结束 &quot;[ &quot;$rc&quot; -eq &quot;0&quot; ]; &quot; 的

         # A return of 4 or higher means there were serious problems.    # 如果 fsck 返回的值大于 4 （不含 4 ， 4 表示错误未修复），则表示 / 文件系统出现重大错误，无法继续

        if [ $rc -gt 1 ]; then    # 这里之所以用 -gt 1 ，是因为如果前面返回 2 或者 3 就会自动重启了，如果执行到这里说明 rc 的值必须至少大于等于 4

            if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then    # 如果 rhgb-client 可执行且 rhgb 服务在运行

                 chvt 1                 # 强制切换到 1# 虚拟控制台

            fi

           failure &quot;$STRING&quot;        # 调用 failure 函数，打印 &quot;checking root filesystem     [ FAILURE ]&quot;

           echo

           echo

           echo $&quot;*** An error occurred during the file system check.&quot;        # 提示 fsck 过程出错

           echo $&quot;*** Dropping you to a shell; the system will reboot&quot;         # 提示将进入紧急模式的 shell

           echo $&quot;*** when you leave the shell.&quot;                                       # 当你退出该 shell 时会自动重启

           str=$&quot;(Repair filesystem)&quot;        # 设置紧急模式下的 shell 提示符

           PS1=&quot;$str \# # &quot;; export PS1     #   设置提示符变量 PS1 为 &quot;(Repair filesystem) &lt;N&gt; # &quot; ， &lt;N&gt; 表示当前是第几个命令

           [ &quot;$SELINUX&quot; = &quot;1&quot; ] &amp;&amp; disable_selinux

           sulogin                                    # 自动执行 sulogin ，这不是 su ，而是 single-user login 的意思，自动进入单用户模式的 shell

                             # 注意这里将进入一个交互式的 shell 。只有你执行 exit 命令，才会执行下面的代码，否则下面的代码根本不会被执行

           echo $&quot;Unmounting file systems&quot;    # 提示卸载文件系统

           umount -a                                    # 卸载所有文件系统， / 除外

           mount -n -o remount,ro /              # / 以 ro 模式挂载

           echo $&quot;Automatic reboot in progress.&quot;        # 提示将自动重启

           reboot -f                                     # 立即重启

        elif [ &quot;$rc&quot; -eq &quot;1&quot; ]; then                     # 如果 rc 的值等于 1 ，则表示错误修复完毕

           _RUN_QUOTACHECK=1                    # 如果上面的 fsck 没有出错，自然就不会重启，所以这里把 _RUN_QUOTACHECK 的值设置为 1 ，表示可以进行 quotacheck 了

        fi   # 这个 fi 是结束 &quot; [ $rc -gt 1 ]; &quot; 的                          

        if [ -f /.autofsck -a -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then    #   如果 rhgb-clinet 可运行且 rhgb 服务在运行

         chvt 8                                                                                                                    # 则切换换 8# 控制台，也就是图形

        fi

fi        # 这个 fi 是结束前面的 &quot; [ -z &quot;$fastboot&quot; -a &quot;$READONLY&quot; != &quot;yes&quot; -a &quot;X$ROOTFSTYPE&quot; != &quot;Xnfs&quot; -a &quot;X$ROOTFSTYPE&quot; != &quot;Xnfs4&quot; ]; &quot; 的

      # 所以如果存在 /fastboot 文件，就直接跳转到这里

###################################################################################################################################

# 注释 ：在 initrd 的 /linuxrc 或者 /init 脚本中会把初始 / 文件系统卸载掉，如果没有卸载完全，则在这里会重新做一次

# Unmount the initrd, if necessary                    # 接下来是卸载 initrd 初始 / 文件系统，一般 /init 都会在结尾 umount /initrd/dev 的

if LC_ALL=C fgrep -q /initrd /proc/mounts &amp;&amp; ! LC_ALL=C fgrep -q /initrd/loopfs /proc/mounts ; then    # 如果 /proc/mounts 文件还有 /initrd/proc 条目，

                                                                                                                                                     # 且不存在 /initrd/loops 条目，则

  if [ -e /initrd/dev/.devfsd ]; then                                                                                                     # 如果存在 /initrd/dev/.devfsd 文件，则      

     umount /initrd/dev                                                                                                                            # 卸载 /initrd/dev 目录

  fi

  umount /initrd                                            # 最后把整个 /initrd 卸载

  /sbin/blockdev --flushbufs /dev/ram0 &gt;/dev/null 2&gt;&amp;1    # 最后用 blockdev --flushbs 命令清除 /dev/ram0 的内容，该社备就是被挂载为初始 / 的

fi

########################################################################################################################################                                                                                # 注释 ：下面是对 / 进行 quota 检查，其他文件系统暂时不执行 quotacheck

# Possibly update quotas if fsck was run on /.                    # 如果 fsck 已经对 / 进行了检查，则执行 quotacheck 更新 quota 情况

LC_ALL=C grep -E [[:space:]]+/[[:space:]]+ /etc/fstab | \        # 在 /etc/fstab 中找出 / 文件系统所在的行，并打印第 4 个字段（属性）

   awk { print $4 } | \                                                           # 这是属性字段

   LC_ALL=C fgrep -q quota                                                    # 找出是否有 quota 字符串，不管是 usrquota 还是 grpquota

_ROOT_HAS_QUOTA=$?                                                    # 把结果赋予 _ROOT_HAS_QUOTA 变量

if [ &quot;X$_RUN_QUOTACHECK&quot; = &quot;X1&quot; -a \                              #   如果 x$_RUN_QUOTACHECK 返回 X1 ，且

   &quot;X$_ROOT_HAS_QUOTA&quot; = &quot;X0&quot; -a \                                #   X$_ROOT_HAS_QUOTA 返回 X0 ，则表示当前可以执行 quotacheck ，且 / 也启用了 quota

   -x /sbin/quotacheck ]; then                                        #   如果存在 quotacheck 命令  

if [ -x /sbin/convertquota ]; then                                    # 如果存在 convertquota 命令

    if [ -f /quota.user ]; then                                                # 且存在 /quota.user 则

         action $&quot;Converting old user quota files: &quot; \                          # 并调用 action 函数，打印提示信息，用 convertquota 转换成 v2 格式的 aquota.user 文件

         /sbin/convertquota -u / &amp;&amp; rm -f /quota.user                # 然后删除 v1 格式的 /quota.user

    fi

    if [ -f /quota.group ]; then                                            # 同样对 /quota.group 转换成 v2 格式的 aquota.group ，并删除 /quota.group 文件

         action $&quot;Converting old group quota files: &quot; \

         /sbin/convertquota -g / &amp;&amp; rm -f /quota.group            # 同样在转换后删除旧的 v1 的 group quota 文件

    fi

fi

action $&quot;Checking root filesystem quotas: &quot; /sbin/quotacheck -nug /        # 然后执行 quotacheck -nug / 对 / 文件系统进行检查，并打印提示信息

fi    # 这个 fi 是结束 &quot;[ &quot;X$_RUN_QUOTACHECK&quot; = &quot;X1&quot; -a &quot;X$_ROOT_HAS_QUOTA&quot; = &quot;X0&quot; -a -x /sbin/quotacheck ]; &quot; 的

################################################################################################################################

# 注释 ：下面这个部分是设置 ISA 设备的，不过由于现在基本没有 ISA 设备了，所以可以跳过该部分，甚至可以在内核中指定不支持 ISA

if [ -x /sbin/isapnp -a -f /etc/isapnp.conf -a ! -f /proc/isapnp ]; then      

   # check for arguments passed from kernel

   if ! strstr &quot;$cmdline&quot; nopnp ; then

PNP=yes

   fi

   if [ -n &quot;$PNP&quot; ]; then

action $&quot;Setting up ISA PNP devices: &quot; /sbin/isapnp /etc/isapnp.conf

   else

action $&quot;Skipping ISA PNP configuration at users request: &quot; /bin/true

   fi

fi

##########################################################################################################

# Remount the root filesystem read-write.                                    # 现在 / 文件系统 fsck 过了，可以按照 read-write 的模式挂载了

update_boot_stage RCmountfs                                                      # 告诉 rhgb 服务器更新 RCmountfs 服务的状态

state=`awk / \/ / &amp;&amp; ($3 !~ /rootfs/) { print $4 } /proc/mounts`    # 从 /proc/mounts 中找出 / 字符串并且第 3 个字段不等于 rootfs ，则打印其第 4 个字段，赋予 state

[ &quot;$state&quot; != &quot;rw&quot; -a &quot;$READONLY&quot; != &quot;yes&quot; ] &amp;&amp; \                              # 如果 state 变量的值不为 rw 且 READONLY 变量的值为 yes ，则

 action $&quot;Remounting root filesystem in read-write mode: &quot; mount -n -o remount,rw /        # 用 -o remount -o rw 重新挂载 / 为 rw 模式，并打印提示信息

#########################################################

# 注释 ：下面开始是 LVM2 的部分了

# LVM2 initialization                                                                                        # 下面部分是 LVM2   的初始化部分

if [ -x /sbin/lvm.static ]; then                                                                          # 如果存在 /sbin/lvm.static 且可执行

   if ! LC_ALL=C fgrep -q &quot;device-mapper&quot; /proc/devices 2&gt;/dev/null ; then        # 如果 /proc/devices 文件不含有 device-mapper ，则

        modprobe dm-mod &gt;/dev/null 2&gt;&amp;1                                                                        # 调用 modprobe 加载 dm-mod

   fi

   echo &quot;mkdmnod&quot; | /sbin/nash --quiet &gt;/dev/null 2&gt;&amp;1                                    # 并调用 nash 执行 mkdmmod ，不过是以 quiet 模式执行的

   [ -n &quot;$SELINUX&quot; ] &amp;&amp; restorecon /dev/mapper/control &gt;/dev/null 2&gt;&amp;1

   if [ -c /dev/mapper/control -a -x /sbin/lvm.static ]; then                                # 假如存在 /dev/mapper/control 这个字符设备且 /sbin/lvm.static 可执行 , 则

        if /sbin/lvm.static vgscan --mknodes --ignorelockingfailure &gt; /dev/null 2&gt;&amp;1 ; then        # 调用 lvm.static 执行 vgscan 扫描所有卷组（忽略锁错误）

            action $&quot;Setting up Logical Volume Management:&quot; /sbin/lvm.static vgchange -a y --ignorelockingfailure        # 如果扫描到卷组，则调用 action 函数，

        fi                                                                                                                                                      # 打印提示信息，并执行 vgchange -ay 激活全部卷组

   fi

fi

# LVM initialization                 # 这是 LVM1 的部分，因为系统可能同时存在 lVM1 和 lvm2 的 pv

if [ -f /etc/lvmtab ]; then        # 如果存在 /etc/lvmtab 文件

   [ -e /proc/lvm ] || modprobe lvm-mod &gt; /dev/null 2&gt;&amp;1    # 且 /proc/lvm 文件存在，如果没有则调用 modprobe lvm-mod 加载 lvm-mod , 注意模块名的不同

       if [ -e /proc/lvm -a -x /sbin/vgchange ]; then                # 如果现在存在 /proc/lvm 文件且 /sbin/vgchange 可执行

            action $&quot;Setting up Logical Volume Management:&quot; /sbin/vgscan &amp;&amp; /sbin/vgchange -a y    # 则调用 action 打印提示信息，并执行 vgchange -ay 激活它们

       fi

fi

###########################################################################################################

# Clean up SELinux labels

if [ -n &quot;$SELINUX&quot; ]; then

  for file in /etc/mtab /etc/ld.so.cache ; do

   [ -r $file ] &amp;&amp; restorecon $file  &gt;/dev/null 2&gt;&amp;1

  done

fi

##############################################################################################

# Clear mtab                                                            # 由于之前的 /etc/mtab 并不准确，所以现在把它的内容清掉

(&gt; /etc/mtab) &amp;&gt; /dev/null                                              # 用 &gt;/etc/mtab 清空其内容

# Remove stale backups

rm -f /etc/mtab~ /etc/mtab~~                                    # 删除 /etc/mtab 的备份文件

# Enter root, /proc and (potentially) /proc/bus/usb and devfs into mtab.        # 把 root ， /proc ， /proc/bus/usb ， devpts 加入到 /etc/mtab 中

mount -f /                                                                    #  -f 表示只更新 /etc/mtab ，但不实际 mount ，因为这些文件系统在之前都挂载了，没有必要再挂载一次

mount -f /proc

mount -f /sys &gt;/dev/null 2&gt;&amp;1

mount -f /dev/pts

[ -f /proc/bus/usb/devices ] &amp;&amp; mount -f -t usbfs usbfs /proc/bus/usb  # 如果存在 /proc/bus/usb/devices 文件，则用 mount -f -t usbfs 挂载到 /proc/bus/usb 下    

[ -e /dev/.devfsd ] &amp;&amp; mount -f -t devfs devfs /dev         # 如果存在 /dev/.devfsd ，则同样挂载 devfs 到 /dev 下

########################################################################################################

# configure all zfcp (scsi over fibrechannel) devices before trying to mount them

# zfcpconf.sh exists only on mainframe

[ -x /sbin/zfcpconf.sh ] &amp;&amp; /sbin/zfcpconf.sh

#############################################################################################################

# The root filesystem is now read-write, so we can now log        # 由于之前的 / 是只读的，所以通过 initlog 来记录信息，现在可以通过 syslog 了

# via syslog() directly..

if [ -n &quot;$IN_INITLOG&quot; ]; then

   IN_INITLOG=                                                                            # 如果 IN_INITLOG 的值不为空则清空它

fi

if ! strstr &quot;$cmdline&quot; nomodules &amp;&amp; [ -f /proc/modules ] ; then        # 如果内核启动参数含有 &quot;nomodules&quot; , 且存在 /proc/modules 文件，则

   USEMODULES=y                                                                            # 把 USEMODULES 设置为 y

fi

# Load modules (for backward compatibility with VARs)              

if [ -f /etc/rc.modules ]; then

/etc/rc.modules

fi

##############################################################################################################

update_boot_stage RCraid                                        # 下面部分是激活 RAID 的

if [ -f /etc/mdadm.conf ]; then                                # 如果存在 /etc/mdadm.conf 则

   /sbin/mdadm -A -s                                                # mdadm -A 表示 Assemble 模式， -s 则表示搜索 /etc/mdadm.conf

fi

# 注释 ：下面解决旧式的用 raid-tools 建立的 software-raid

if [ -f /etc/raidtab ]; then                                        # 如果存在旧式的 raidtools 的 /etc/raidtab 则

   # Add raid devices

   [ -f /proc/mdstat ] || modprobe md &gt;/dev/null 2&gt;&amp;1        # 如果不存在 /proc/mdstat 文件，则用 modprobe md 先加载 md 模块

   if [ -f /proc/mdstat ]; then

        echo -n $&quot;Starting up RAID devices: &quot;                             # 并输出 &quot;Starting up RAID devices:&quot;

        rc=0                                                                           # 先把 rc 的值设置为 0 ，如果有某个 raid 激活失败，则 rc 为 1

                                                                                         # rc   和下面的 RESULT 不同， RESULT 是每个 raid 的激活状态，而 rc 是全局的激活状态

        for i in `awk {if ($1==&quot;raiddev&quot;) print $2} /etc/raidtab`     # 用 awk 从 /etc/raidtab 中找出所有 raiddev 行打印第 2 个字段也就是 raid 的设备名

        do                                                                                    # 针对每个 Software-raid 执行下面的代码

               RAIDDEV=`basename $i`                                                # 把每个 raid 设备的设备名（出去前面目录部分）赋予变量 RAIDDEV

               RAIDSTAT=`LC_ALL=C grep &quot;^$RAIDDEV : active&quot; /proc/mdstat`            # 如果 /proc/mdstat 中表明该 RAID 已经是 active 的状态

               if [ -z &quot;$RAIDSTAT&quot; ]; then                                                                # 如果状态为空，表明该 RAID 并没有激活

                   # First scan the /etc/fstab for the &quot;noauto&quot;-flag

                   # for this device. If found, skip the initialization

                   # for it to avoid dropping to a shell on errors.

                   # If not, try raidstart...if that fails then

                   # fall back to raidadd, raidrun.  If that

                   # also fails, then we drop to a shell

                   RESULT=1                                                                                    # 先把 RESULT 设置为 1 ，表示 false

                   INFSTAB=`LC_ALL=C grep -c &quot;^$i&quot; /etc/fstab`                                 # 然后检查 /etc/fstab 中是否有以指定的 md 设备开头的行，结果赋予 INFSTAB

                   if [ $INFSTAB -eq 0 ] ; then                                                           # 如果 INFSTAB 为 0 ，表示没有找到，也就是不希望自动我你挂载该 raid 设备

                          RESULT=0                                                                                # 则把 RESULT 设置为 0

                          RAIDDEV=&quot;$RAIDDEV(skipped)&quot;                                                    # 并在 RAIDDEV 后面加上 (skipped) ，表示该设备跳过不激活它

                   fi

                   NOAUTO=`LC_ALL=C grep &quot;^$i&quot; /etc/fstab | LC_ALL=C fgrep -c &quot;noauto&quot;`    # 如果该设备在 /etc/fstab 中但它的属性含有 noauto

                   if [ $NOAUTO -gt 0 ]; then                                                                            # 则和上面不在 /etc/fstab 一样处理

                          RESULT=0

                          RAIDDEV=&quot;$RAIDDEV(skipped)&quot;

                   fi

                   if [ $RESULT -gt 0 -a -x /sbin/mdadm ]; then                                            # 如果 /sbin/mdadm 文件可执行，且尝试用 mdadm 来激活它们

                           /sbin/mdadm -Ac partitions $i -m dev                                                    # mdadm 将从 /proc/partitions 文件读取并状态指定 raid($i) ， -m 表示 minor

                                                                                                                                     # number 就等于它刚才装配的 raid 设备名的数字部分

                           RESULT=$?                                                                                           # 并把 mdadm 的结果赋予 RESULT 变量

                   fi

                   if [ $RESULT -gt 0 -a -x /sbin/raidstart ]; then                                            # 如果 RESULT 大于 0 ，表示 madm 命令失败，则尝试用 /sbin/raidstart 来

                       /sbin/raidstart $i

                       RESULT=$?                                                                                  

                   fi

                   if [ $RESULT -gt 0 -a -x /sbin/raid0run ]; then                                             # 如果 raidstart 还是失败，则尝试用 raid0run 来

                       /sbin/raid0run $i

                       RESULT=$?

                   fi

                   if [ $RESULT -gt 0 -a -x /sbin/raidadd -a -x /sbin/raidrun ]; then                  # 如果 raid0run 失败则尝试用 raidadd 然后用 raidrun

                       /sbin/raidadd $i

                       /sbin/raidrun $i

                       RESULT=$?

                   fi

                   if [ $RESULT -gt 0 ]; then                                                                         # 如果还是失败，则 rc 的值最终为 1

                       rc=1

                  fi

                   echo -n &quot;$RAIDDEV &quot;                                                                                # 输出当前操作的 raid 设备名

               else            # 这个 else 是对应 &quot; if [ -z &quot;$RAIDSTAT&quot; ]; &quot;的，因为可能内核集成了 raid support ，自动激活 raid

                      echo -n &quot;$RAIDDEV &quot;                                                                             # 如果 RAID 的状态本来就是 active ，则直接输出 raid 的名称

               fi

        done

echo

# A non-zero return means there were problems.                    # 如果只要有一个 raid 激活失败，则 rc 的值就为 1

if [ $rc -gt 0 ]; then                                                                # 如果 rc 的值大于 0 ，则表示有 raid 激活失败

        if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then    # 则按照前面 fsck 失败的情况，进入紧急模式

     chvt 1

 fi

 echo

 echo

 echo $&quot;*** An error occurred during the RAID startup&quot;

 echo $&quot;*** Dropping you to a shell; the system will reboot&quot;

 echo $&quot;*** when you leave the shell.&quot;

 str=$&quot;(RAID Repair)&quot;                                                                            # 只不过提示符有所不同而已，变成了 (RAID Repair) &lt;N&gt; #

 PS1=&quot;$str \# # &quot;; export PS1

 [ &quot;$SELINUX&quot; = &quot;1&quot; ] &amp;&amp; disable_selinux

 sulogin

 echo $&quot;Unmounting file systems&quot;

 umount -a

 mount -n -o remount,ro /

 echo $&quot;Automatic reboot in progress.&quot;

 reboot -f

fi        # 注释 ；这个 fi 是结束 &quot;if [ -f /etc/raidtab ];&quot; 的，如果不存在 /etc/raidtab ，则直接跳到这里

##################################################################################################################

# LVM2 initialization, take 2                                                        # 由于 LVM 的 pv 可能是建立在 RAID 之上的，所以再次尝试扫描 vg 并激活它们

if [ -c /dev/mapper/control -a -x /sbin/lvm.static ]; then             # 步骤和前面的一样

 if /sbin/lvm.static vgscan &gt; /dev/null 2&gt;&amp;1 ; then

  action $&quot;Setting up Logical Volume Management:&quot; /sbin/lvm.static vgscan --mknodes --ignorelockingfailure &amp;&amp; /sbin/lvm.static vgchange -a y --ignorelockingfailure

 fi

fi

# LVM initialization, take 2 (it could be on top of RAID)

if [ -e /proc/lvm -a -x /sbin/vgchange -a -f /etc/lvmtab ]; then

 action $&quot;Setting up Logical Volume Management:&quot; /sbin/vgscan &amp;&amp; /sbin/vgchange -a y

fi

   fi

fi

###############################################################################################################

# 注释 ：下面对其他文件系统（ / 除外）的文件系统进行 fsck

_RUN_QUOTACHECK=0        # 还是把 _RUN_QUOTACHECK 变量的值设置为 0

# Check filesystems            # 检查其他文件系统

if [ -z &quot;$fastboot&quot; ]; then        # 如果不存在 /etc/fastboot ，则执行下面的脚本。可以看到 shutdown -r 不仅影响 / 的 fsck ，也影响其他文件系统的 fsck

       STRING=$&quot;Checking filesystems&quot;        # 打印提示信息

       echo $STRING

        if [ &quot;${RHGB_STARTED}&quot; != &quot;0&quot; -a -w /etc/rhgb/temp/rhgb-console ]; then

                 fsck -T -R -A -a $fsckoptions &gt; /etc/rhgb/temp/rhgb-console                # -R 表示跳过 / 文件系统，其他选项和之前的一样

        else

                 initlog -c &quot;fsck -T -R -A -a $fsckoptions&quot;

        fi

        rc=$?

        if [ &quot;$rc&quot; -eq &quot;0&quot; ]; then                                    #   这部分和之前处理 / 文件系统的 fsck 结果一样

          success &quot;$STRING&quot;

          echo

       elif [ &quot;$rc&quot; -eq &quot;1&quot; ]; then

          passed &quot;$STRING&quot;

          echo

       elif [ &quot;$rc&quot; -eq &quot;2&quot; -o &quot;$rc&quot; -eq &quot;3&quot; ]; then

          echo $&quot;Unmounting file systems&quot;

          umount -a

          mount -n -o remount,ro /

          echo $&quot;Automatic reboot in progress.&quot;

          reboot -f

       fi

        # A return of 4 or higher means there were serious problems.

        if [ $rc -gt 1 ]; then

            if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then

                 chvt 1

            fi

            failure &quot;$STRING&quot;

            echo

            echo

            echo $&quot;*** An error occurred during the file system check.&quot;

            echo $&quot;*** Dropping you to a shell; the system will reboot&quot;

            echo $&quot;*** when you leave the shell.&quot;

            str=$&quot;(Repair filesystem)&quot;

            PS1=&quot;$str \# # &quot;; export PS1

            [ &quot;$SELINUX&quot; = &quot;1&quot; ] &amp;&amp; disable_selinux

            sulogin

            echo $&quot;Unmounting file systems&quot;

            umount -a

            mount -n -o remount,ro /

            echo $&quot;Automatic reboot in progress.&quot;

            reboot -f

        elif [ &quot;$rc&quot; -eq &quot;1&quot; -a -x /sbin/quotacheck ]; then

            _RUN_QUOTACHECK=1                                        # 如果   fsck 修复成功，则 _RUN_QUOTACHECK 的值修改为 1 ，表示可以进行其他文件系统的 quotacheck 了

        fi        #   这个 fi 是结束  &quot;if [ $rc -gt 1 ]; &quot; 的

fi                # 这个 fi 是结束 &quot; if [ -z &quot;$fastboot&quot; ]; &quot; 的

#####################################################################################################################

# Mount all other filesystems (except for NFS and /proc, which is already            # 挂载除了 / ， /proc ， /proc/bus/usb ， devpts 之外的文件系统

# mounted). Contrary to standard usage,

# filesystems are NOT unmounted in single user mode.

action $&quot;Mounting local filesystems: &quot; mount -a -t nonfs,nfs4,smbfs,ncpfs,cifs,gfs -O no_netdev    # 挂载类型为 nonfs 、 nfs4 、 smbfs 、 ncptfs 、 cifs 、 gfs 的文件系统

                                                                                     # 但条件是这些文件系统对应在 /etc/fstab 中的属性字段没有 netdev 属性

#######################################################################################################################

# Start the graphical boot, if necessary and not done yet.                # 现在又再次检查是否可以启动图形启动界面

if strstr &quot;$cmdline&quot; rhgb &amp;&amp; [ &quot;$RHGB_STARTED&quot; -eq 0 -a &quot;$BOOTUP&quot; = &quot;color&quot; -a &quot;$GRAPHICAL&quot; = &quot;yes&quot; -a -x /usr/bin/rhgb ]; then

  LC_MESSAGES= /usr/bin/rhgb

  RHGB_STARTED=1

fi

#############################################################################################################

# check remaining quotas other than root                                                            # 对 / 系统之外的文件系统进行 quota 检查

if [ X&quot;$_RUN_QUOTACHECK&quot; = X1 -a -x /sbin/quotacheck ]; then                            # 方法和之前对 / 进行 quotachec 一样

    if [ -x /sbin/convertquota ]; then

        # try to convert old quotas

        for mountpt in `awk $4 ~ /quota/{print $2} /etc/mtab` ; do

             if [ -f &quot;$mountpt/quota.user&quot; ]; then

                 action $&quot;Converting old user quota files: &quot; \

                 /sbin/convertquota -u $mountpt &amp;&amp; \

                  rm -f $mountpt/quota.user

             fi

             if [ -f &quot;$mountpt/quota.group&quot; ]; then

                 action $&quot;Converting old group quota files: &quot; \

                 /sbin/convertquota -g $mountpt &amp;&amp; \

                  rm -f $mountpt/quota.group

             fi

    done

fi

action $&quot;Checking local filesystem quotas: &quot; /sbin/quotacheck -aRnug                # quotachk -augRn 表示对 /etc/fstab 中除了 / 之外，所有启用了 usrquota 、 grpquota 的

fi                                                                                                                 # 的文件系统都检查  

if [ -x /sbin/quotaon ]; then

   action $&quot;Enabling local filesystem quotas: &quot; /sbin/quotaon -aug                       # 执行 quotaon 启动 quota 功能

fi

##################################################################################################################

#

# Check to see if SELinux requires a relabel

#

[ -n &quot;$SELINUX&quot; ] &amp;&amp; [ -f /.autorelabel ] &amp;&amp; relabel_selinux

##################################################################################################################

# Initialize pseudo-random number generator                                                    # 初始化随机数生成器

if [ -f &quot;/var/lib/random-seed&quot; ]; then                                                                # 如果存在 /var/lib/random-seed 文件，则

cat /var/lib/random-seed &gt; /dev/urandom                                                            # 把它的内容送给 /dev/urandom

else                                                                                                                # 否则

touch /var/lib/random-seed                                                                                # 创建该文件

fi

chmod 600 /var/lib/random-seed                                                                     # 修改该文件的权限为 600 （ rw------- ）

dd if=/dev/urandom of=/var/lib/random-seed count=1 bs=512 2&gt;/dev/null           # 从 /dev/urandom 读入 512 kB 并送给 /var/lib/random-seed

# Use the hardware RNG to seed the entropy pool, if available

[ -x /sbin/rngd -a -f /dev/hw_random ] &amp;&amp; rngd                                                # 如果 /sbin/rngd 可执行且存在 /dev/hw_random 文件，则执行 rngd 为加密池生成种子

##################################################################################################################

# Configure machine if necessary.                                                                # 这部分允许你在启动时作一些配置（手工的）

if [ -f /.unconfigured ]; then                                                                        # 如果存在 /.unconfigured 文件

   if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then                    # 且 rhgb-client 可执行， rhgb 服务在运行，则

        chvt 1                                                                                                                    # 切换回 1# 控制台

   fi

   if [ -x /usr/bin/system-config-keyboard ]; then                                               # 如果 /usr/bin/sysconfig-keyboard 可执行

        /usr/bin/system-config-keyboard                                                                    # 则执行该命令

   fi

   if [ -x /usr/bin/passwd ]; then                                                                        # 如果存在 /usr/bin/passwd ，则

       /usr/bin/passwd root                                                                                        # 执行 passwd root ，修改 root 密码

   fi

   if [ -x /usr/sbin/netconfig ]; then                                                                    # 如果存在 /usr/sbin/netconfig ，则

        /usr/sbin/netconfig                                                                                            #   执行 /usr/sbin/netconfig ，重新配置网络

   fi

   if [ -x /usr/sbin/timeconfig ]; then                                                                    # 如果存在 /usr/sbin/timeconfig ，则

        /usr/sbin/timeconfig                                                                                            # 执行 /usr/sbin/timeconfig 重新配置时间和时区

   fi

   if [ -x /usr/sbin/authconfig ]; then                                                                    # 如果存在 /usr/sbin/authconfig ，则

        /usr/sbin/authconfig --nostart                                                                                # 执行 /usr/sbin/authconfig --nostart 重新配置 shadow 、 LDAP 、 kerberos 等

   fi

   if [ -x /usr/sbin/ntsysv ]; then                                                                            # 如果存在 /usr/sbin/ntsysv ，则

        /usr/sbin/ntsysv --level 35                                                                                    # 执行 /usr/sbin/ntsysv --level 35 对运行级别 3 ， 5 下的服务启动进行配置

   fi

   # Reread in network configuration data.                                                              # 重新读取 /etc/sysconfig/network 文件

   if [ -f /etc/sysconfig/network ]; then                                                                  #   如果存在该文件就执行它        

        . /etc/sysconfig/network

        # Reset the hostname.

        action $&quot;Resetting hostname ${HOSTNAME}: &quot; hostname ${HOSTNAME}        # 重新设置主机名，因为上面的 /etc/sysconfig/network 被重新执行了

   fi

   rm -f /.unconfigured                                                                          # 删除 /.unconfigured

   if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then        # 切换回 8# 控制台

        chvt 8

   fi

fi

# Clean out /.                                                                                        # 下面把 / 下的一些文件删除，包括 /fastboot 、 /fsckoptions 、 /forcefsck 、 /.autofsck

rm -f /fastboot /fsckoptions /forcefsck /.autofsck /halt /poweroff &amp;&gt; /dev/null        # /halt 、 /poweroff ，这样下次重启就会再次检测是否需要 fsck 了

# Do we need (w|u)tmpx files? We dont set them up, but the sysadmin might...        # 正常情况下是需要 /var/run/utmpx 和 /var/log/wtmpx 文件

_NEED_XFILES=                                                                                                        #  _NEED_XFILES 的值为空

[ -f /var/run/utmpx -o -f /var/log/wtmpx ] &amp;&amp; _NEED_XFILES=1                                  # 如果存在其中之 1 个文件，就把 _NEED_XFILES 的值设置为 1

##############################################################################################################

# Clean up /var.  Id use find, but /usr may not be mounted.            # 现在清理 /var 目录。

for afile in /var/lock/* /var/run/* ; do                                          # 对于在 /var/lock/ 和 /var/run 下的每个文件，执行下面的操作

if [ -d &quot;$afile&quot; ]; then                                                                        # 如果 $afile 是一个目录，则

   case &quot;$afile&quot; in                                                                                    # 根据 $afile 的值进行选择

 */news|*/mon) ;;                                                                                    # 如果该目录的路径名是以 /news 或者 /mon 结尾则跳过它们不处理

 */sudo)  rm -f $afile/*/* ;;                                                                        # 如果是以 /sudo 结尾，则用 rm -rf 把该目录下面的&quot;所有子目录&quot;下的全部东西删除

 */vmware) rm -rf $afile/*/* ;;                                                                   # 如果是以 /vmware 结尾，则用 rm -fr 把它下面的&quot;所有子目录&quot;下的内容全部删除

 */samba) rm -rf $afile/*/* ;;                                                                      # 如果是以 /samba 结尾，也是同样操作

 *)  rm -f $afile/* ;;                                                                                   # 如果不是上面 4 种类型，则把该目录下的所有内容都删除，不仅包括子目录，也包括文件

   esac

else                                                                                                 # 如果 $afile 是一个文件，不是目录，则

   rm -f $afile                                                                                            # 删除该文件

fi

done

rm -f /var/lib/rpm/__db* &amp;&gt; /dev/null                                            # 删除 /var/lib/rpm/_db*

# 补充 ：可以看到不要在 /var/run 和 /var/lock 下面放置有用的文件，否则启动时会被删除。

# 正常情况下 /var/run 有如下内容 ：

[root@mail run]# ls

acpid.socket=           dbus/           iptraf/         mysqld/     ptal-mlcd/    rpc.statd.pid  syslogd.pid

atd.pid                 dovecot/        klogd.pid       named/      ptal-printd/  saslauthd/     usb/

console/                dovecot-login/  mailman/        netreport/  pvm3/         sendmail.pid   utmp

crond.pid               gpm.pid         mdadm/          news/       quagga/       sm-client.pid  winbindd/

cups-config-daemon.pid  haldaemon.pid   mdmpd/          nscd/       radiusd/      sshd.pid       xfs.pid

cupsd.pid               iiim/           messagebus.pid  ppp/        radvd/        sudo/          xinetd.pid

[root@mail run]#

#####################################################################################################################

# Reset pam_console permissions                                                                    # 这里重置 pam_console 的权限

[ -x /sbin/pam_console_apply ] &amp;&amp; /sbin/pam_console_apply -r                        # 如果 /sbin/pam_console_apply 存在且可以执行，则执行 /sbin/pam_console_apply -r

                                                                                                                 # -r 是表示 reset 的意思，具体见 man pam_console_apply

##############################################################################################################

# 注意下面这段话，一直到后面的 &quot;kill -TERM `/sbin/pidof getkey` &gt;/dev/null 2&gt;&amp;1&quot; 才算结束，这一段放在 {} 中的代码在 bash 中被成为 command block

# 它可以把放在 {} 中的多个命令当成 1 个命令来执行，这样可以对多个命令一次使用 IO 重定向。

# 实际上，这部分是被放在后台运行的 ，因为它是以  {xxxxx}&amp; 的形式出现的

# 我们可以也可以用函数的形式来把下面的代码做成一个函数，不过 command blocks 是一种简便的方式

{                                                                                                        # command block 开始

# Clean up utmp/wtmp                                                                        # 首先清空 utmp 和 wtmp 文件的内容

&gt; /var/run/utmp                                                                                  # 首先清空 /var/run/utmp 文件

touch /var/log/wtmp                                                                           # 再用 touch 创建 /var/log/wtmp 文件

chgrp utmp /var/run/utmp /var/log/wtmp                                             # 把这两个文件的组都改为 utmp 组

chmod 0664 /var/run/utmp /var/log/wtmp                                            # 把这两个文件的权限改为 0664 （ rw-rw-r-- ）

if [ -n &quot;$_NEED_XFILES&quot; ]; then                                                               #   如果 _NEED_XFILES 的值不为空

 &gt; /var/run/utmpx                                                                                    # 则清空  /var/log/umptx

 touch /var/log/wtmpx                                                                             # 创建 /var/log/wtmpx

 chgrp utmp /var/run/utmpx /var/log/wtmpx                                              # 同样是修改 group 和 permission

 chmod 0664 /var/run/utmpx /var/log/wtmpx

fi

# Clean up various /tmp bits                                                                        # 解析来是清理 /tmp 目录了

rm -f /tmp/.X*-lock /tmp/.lock.* /tmp/.gdm_socket /tmp/.s.PGSQL.*            # 删除 /tmp 下的一些隐藏文件

rm -rf /tmp/.X*-unix /tmp/.ICE-unix /tmp/.font-unix /tmp/hsperfdata_* \

      /tmp/kde-* /tmp/ksocket-* /tmp/mc-* /tmp/mcop-* /tmp/orbit-*  \

      /tmp/scrollkeeper-*  /tmp/ssh-*

# Make ICE directory                                                                                     # 下面创建 ICE 目录

mkdir -m 1777 -p /tmp/.ICE-unix &gt;/dev/null 2&gt;&amp;1                                            # 创建 /tmp/.ICE-unix/ 目录，权限为 1777 （ sticky ）

chown root:root /tmp/.ICE-unix                                                                     # 改为属于 root 用户、 root 组

[ -n &quot;$SELINUX&quot; ] &amp;&amp; restorecon /tmp/.ICE-unix &gt;/dev/null 2&gt;&amp;1      

# Start up swapping.                                               # 下面是启动 swap 空间

update_boot_stage RCswap                                     # 执行 rhgb-client --update RCswap

action $&quot;Enabling swap space: &quot; swapon -a -e            # 启用所有 swap 分区，并跳过那些不存在的 swap 设备

# Set up binfmt_misc                                                                                    # 设置 binfmt_misc

/bin/mount -t binfmt_misc none /proc/sys/fs/binfmt_misc &gt; /dev/null 2&gt;&amp;1     # 挂载 binfmt_misc 文件系统

# Initialize the serial ports.                                    # 初始化串口

if [ -f /etc/rc.serial ]; then                                    # 如果存在 /etc/rc.serial 则执行该文件

. /etc/rc.serial

fi

# If they asked for ide-scsi, load it

if strstr &quot;$cmdline&quot; ide-scsi ; then

modprobe ide-cd &gt;/dev/null 2&gt;&amp;1

modprobe ide-scsi &gt;/dev/null 2&gt;&amp;1

fi

# Turn on harddisk optimization                                                                                        # 下面部分是用 hdparm 命令对硬盘进行优化

# There is only one file /etc/sysconfig/harddisks for all disks                                                # 默认是使用统一的优化参数文件 /etc/sysconfig/harddisks ，

# after installing the hdparm-RPM. If you need different hdparm parameters                          # 如果你想对针对不同的硬盘进行优化，把该文件拷贝并重新命名为

# for each of your disks, copy /etc/sysconfig/harddisks to                                                    # /etc/sysconfig/harddisk&lt;hda~hdt&gt; ，并修改该文件

# /etc/sysconfig/harddiskhda (hdb, hdc...) and modify it.

# Each disk which has no special parameters will use the defaults.                                        # 如果某个选项没有指定，则默认使用默认值

# Each non-disk which has no special parameters will be ignored.                                        

#

                                                                                                                                        # 下面定义一个数组，名为 disk ，共有 21 个元素

disk[0]=s;

disk[1]=hda;  disk[2]=hdb;  disk[3]=hdc;  disk[4]=hdd;

disk[5]=hde;  disk[6]=hdf;  disk[7]=hdg;  disk[8]=hdh;

disk[9]=hdi;  disk[10]=hdj; disk[11]=hdk; disk[12]=hdl;

disk[13]=hdm; disk[14]=hdn; disk[15]=hdo; disk[16]=hdp;

disk[17]=hdq; disk[18]=hdr; disk[19]=hds; disk[20]=hdt;

if [ -x /sbin/hdparm ]; then                                                                                     # 如果存在 /sbin/hdparm 且可执行，则

  for device in 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20; do                            # 从 0-20 循环，每次把一个数字赋予 device 变量

       unset MULTIPLE_IO USE_DMA EIDE_32BIT LOOKAHEAD EXTRA_PARAMS                         # 首先把 MULTIPLE_IO 、 USE_DMA 、 EIDE_32BIT 、 LOOKAHEAD 、

                                                                                                                                  # EXTRA_PARAMS 的值清空

       if [ -f /etc/sysconfig/harddisk${disk[$device]} ]; then                                              # 如果存在 /etc/sysconfig/harddisk&lt;hda~hdt&gt; 文件，则    

               . /etc/sysconfig/harddisk${disk[$device]}                                                                # 执行该文件

               HDFLAGS[$device]=                                                                                                # 把 HDFLAGS 数组对应 $device 的元素的值清空

               if [ -n &quot;$MULTIPLE_IO&quot; ]; then                                                                                  # 如果 MULTIPLE_IO 的值不为空，则

                   HDFLAGS[$device]=&quot;-q -m$MULTIPLE_IO&quot;                                                                    # HDFLAGS 数组对应该 deivce 值的元素的值为 -q -m$MULTIPE_IO

               fi

               if [ -n &quot;$USE_DMA&quot; ]; then                                                                                        # 如果 USE_DMA 的值不为空，则

                   HDFLAGS[$device]=&quot;${HDFLAGS[$device]} -q -d$USE_DMA&quot;                                        # HDFLAGS 对应该 deivce 值的元素的值再加上  -q -d$USE_DMA

               fi

               if [ -n &quot;$EIDE_32BIT&quot; ]; then                                                                                      # 如果 EIDE_32BIT 变量的值不为空，则

                   HDFLAGS[$device]=&quot;${HDFLAGS[$device]} -q -c$EIDE_32BIT&quot;                                        # HDFLAGS 对应 device 值的元素的值再加上 -q -c$EIDE_32BIT

               fi  

               if [ -n &quot;$LOOKAHEAD&quot; ]; then                                                                                    # 如果 LOOKAHEAD 变量的值不为空，则

                   HDFLAGS[$device]=&quot;${HDFLAGS[$device]} -q -A$LOOKAHEAD&quot;                                        # HDFLGAS 对应 device 值的元素的值加上 -q -A$LOOKAHEAD

               fi

               if [ -n &quot;$EXTRA_PARAMS&quot; ]; then                                                                                # 如果 EXTRA_PARAMS 变量的值不为空，则

                   HDFLAGS[$device]=&quot;${HDFLAGS[$device]} $EXTRA_PARAMS&quot;                                           # HDFLAGS 对应 device 值的元素的值加上 -q $EXTRA_PARAMS

               fi

       else                                                                                                                        # 如果不存在 /etc/sysconfig/harddisk&lt;hda~hdt&gt; 文件，则

               HDFLAGS[$device]=&quot;${HDFLAGS[0]}&quot;                                                                            # 统一使用 HDFLAGS[0] 的值作为每个硬盘的优化参数值

       fi

       if [ -e &quot;/proc/ide/${disk[$device]}/media&quot; ]; then                                                      # 如果存在 /proc/ide/&lt;hda~hdt&gt;/media 文件，则

            hdmedia=`cat /proc/ide/${disk[$device]}/media`                                                    # 则找出它的 media 类型并赋予变俩功能 hdmedia

            if [ &quot;$hdmedia&quot; = &quot;disk&quot; -o -f &quot;/etc/sysconfig/harddisk${disk[$device]}&quot; ]; then        # 如果 hdmedia 的值是 &quot;disk&quot; 或者存在 /etc/sysconfig/harddisk&lt;hda-hdt&gt; 文件

                 if [ -n &quot;${HDFLAGS[$device]}&quot; ]; then                                                                    # 且对应硬盘的参数值不为空，则

                     action $&quot;Setting hard drive parameters for ${disk[$device]}: &quot;  /sbin/hdparm ${HDFLAGS[$device]} /dev/${disk[$device]}    # 调用 action 函数，

                 fi                                                                                                                                                                                    # 执行 /sbin/hdparm 命令，

            fi                                                                                                                                                                                         # 根据给定优化参数值进行优化

       fi

  done

fi            # 注释 ：这个 fi 是结束 &quot;if [ -x /sbin/hdparm ];&quot; 的

# Boot time profiles. Yes, this should be somewhere else.                      

if [ -x /usr/sbin/system-config-network-cmd ]; then

 if strstr &quot;$cmdline&quot; netprofile= ; then

   for arg in $cmdline ; do

       if [ &quot;${arg##netprofile=}&quot; != &quot;${arg}&quot; ]; then

    /usr/sbin/system-config-network-cmd --profile ${arg##netprofile=}

       fi

   done

 fi

fi

# Now that we have all of our basic modules loaded and the kernel going,        # 现在所有基础的模块都已经被加载，内核已经开始运行了。可以把这些信息导出了

# lets dump the syslog ring somewhere so we can find it later                        # 这样在启动后可以重新查阅 .

# 补充 ：在第 125 行，我们执行了 dmesg -n  $LOGLEVEL 命令，把信息都写入 syslog 了。

# 现在把它们从 syslog 中导出来到文件中                                                                              

dmesg -s 131072 &gt; /var/log/dmesg       # 执行 dmesg -s 131072 &gt; /var/log/dmesg 文件，导出的内容是 131072 字节。

                                                       # 默认是 16392 字节                                                                

# create the crash indicator flag to warn on crashes, offer fsck with timeout        # 在这里创建 /.autofsck , 如果系统在这里崩溃了，下次重启就会出现 fsck 提示

touch /.autofsck &amp;&gt; /dev/null                    # 用 touch 命令创建 /.autofsck 。不过可以看到前面的第 785 -786 行 把 /.autofsck 连同其他 /fastboot 等都删除了

kill -TERM `/sbin/pidof getkey` &gt;/dev/null 2&gt;&amp;1        # 在这里才把 getkey 命令杀死

} &amp;                                                                        # command block 结束

###############################################################################################################################

if strstr &quot;$cmdline&quot; confirm ; then                                                # 如果内核启动参数含有 &quot;comfirm&quot; ，则

touch /var/run/confirm                                                                 # 创建 /var/run/confirm 文件

fi  

if [ &quot;$PROMPT&quot; != &quot;no&quot; ]; then                                                      # 如果 PROMPT 变量的值不为 no ，则

/sbin/getkey i &amp;&amp; touch /var/run/confirm                                    # 调用 getkey 等待用户输入 i ，如果按下 i 则创建 /var/run/confirm

fi

wait                                                                                            # 一旦用户按下任意键，则跳过该行，继续执行下面的代码

if [ -x /sbin/redhat-support-check -a -f /var/lib/supportinfo ]; then        #   如果存在 /sbin/redhat-support-check 且可执行，且存在 /var/lib/supportinfo   文件，则

/sbin/redhat-support-check || {                                                                # 执行该命令，如果不成功则输出&quot; Normal startup will continue in 10 seconds &quot; ，

  echo $&quot;Normal startup will continue in 10 seconds.&quot;                                    # 然后睡眠 10 秒钟

  sleep 10

}

fi

#########################################################################################################################

# Let rhgb know that were leaving rc.sysinit                                            # 到这里 rc.sysinit 就结束了。

if [ -x /usr/bin/rhgb-client ] &amp;&amp; /usr/bin/rhgb-client --ping ; then             # 执行 rhgb-client --sysinit ，告诉 rhgb 服务器已经完成 rc.sysinit 了

   /usr/bin/rhgb-client --sysinit

fi

#####################################################################################