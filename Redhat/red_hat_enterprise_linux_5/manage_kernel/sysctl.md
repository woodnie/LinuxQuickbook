### sysctl {#sysctl}

NAME

      sysctl - configure kernel parameters at runtime

      配置内核

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