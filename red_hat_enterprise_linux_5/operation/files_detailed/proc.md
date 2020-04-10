#### /proc {#proc}

/proc

/proc and /sys 文件系统也含有总线和设备的特殊信

用/proc获得（使用cat,strings命令）内核配置信息或者进行内核设置。

/proc是虚拟文件系统：1)文件没有保存到硬盘上；2)条目没有保留，重新引导后修改会被重新初始化。

用来显示进程信息、内存资源、硬件设备、内核内存等

可用来修改网络和内存子系统或者修改内核属性

修改立即生效

/proc 文件虚拟系统是一种内核和内核模块用来向进程 (process) 发送信息的机制 (所以叫做 /proc)。这个伪文件系统让你可以和内核内部数据结构进行交互，获取 有关进程的有用信息，在运行中 (on the fly) 改变设置 (通过改变内核参数)。 与其他文件系统不同，/proc 存在于内存之中而不是硬盘。不用重新启动而去看 CMOS ，就可以知道系统信息。这就是 /proc 的妙处之一。

/proc 目录里主要文件内容：

e.g.:（修改内核配置）

使用sysctl修改 ：

[root@node ~]# sysctl -a  #查看所有内核配置

[root@node ~]# sysctl -a |grep ip_forward

net.ipv4.ip_forward = 0    #1禁用ip forward

[root@node ~]# sysctl -w net.ipv4.ip_forward=1

net.ipv4.ip_forward = 1    #1开启ip forward

[root@node ~]# sysctl -p  #重新载入/ect/sysctl.conf

++++++++++++++++++++++++++++++++++++++

net.ipv4.ip_forward = 0    #0禁用ip forward

net.ipv4.ip_forward = 1    #1开启ip forward

net.ipv4.icmp_echo_ignore_all = 0  #忽略

net.ipv4.icmp_echo_ignore_all = 1  #不忽略

++++++++++++++++++++++++++++++++++++++

修改/proc下的配置:

[root@node ~]# cat /proc/sys/net/ipv4/ip_forward

0

[root@node ~]# echo 1 &gt;&gt; /proc/sys/net/ipv4/ip_forward    

[root@node ~]# cat /proc/sys/net/ipv4/ip_forward      

1

修改/etc/sysctl.conf 配置文件（重启有效）

[root@node ~]# sysctl -p  #重新载入/ect/sysctl.conf    #重新载入配置文件，使其及时（长久）生效