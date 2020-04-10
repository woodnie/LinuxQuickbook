#### networ issue {#networ-issue}

网络接口：

本地接口名称：

Ethernet(以太网)：eth0，eth1...

拨号：ppp0，ppp1...

回环：lo

网卡驱动程序选择：

1.驱动程序关联：

[root@node ~]# cat /etc/modprobe.conf

...

alias eth0 e1000    #  eth0 ：(网络)接口卡逻辑名；e1000：内核模块/驱动程序

alias eth1 e1000

2.：(网络)接口卡选择：

[root@node ~]# grep HWADDR /etc/sysconfig/network-scripts/ifcfg-eth*

/etc/sysconfig/network-scripts/ifcfg-eth0:HWADDR=00:0c:29:81:f0:ed             #eth0:HWADDR=00:0c:29:81:f0:ed

/etc/sysconfig/network-scripts/ifcfg-eth1:HWADDR=00:0c:29:81:f0:f7              #eth1:HWADDR=00:0c:29:81:f0:f7

修改MAC地址

HWADDR=, 其中 以AA:BB:CC:DD:EE:FF形式的以太网设备的硬件地址.在有多个网卡设备的机器上，这个字段是非常有用的，它保证设备接口被分配了正确的设备名 ，而不考虑每个网卡模块被配置的加载顺序.这个字段不能和MACADDR一起使用.

MACADDR=, 其中 以AA:BB:CC:DD:EE:FF形式的以太网设备的硬件地址.在有多个网卡设备的机器上.这个字段用于给一个接口分配一个MAC地址，覆盖物理分配的MAC地址 . 这个字段不能和HWADDR一起使用.

4.1运行时修改

[root@node ~]# /sbin/ifconfig ethX  down

[root@node ~]# /sbin/ifconfig ethX  hw ether NEW_MAC_ADDR

[root@node ~]# /sbin/ifconfig ethX  up

[root@node ~]# ip link set ethX address NEW_MAC_ADDR

把以上内容添加到/etc/rc.loacl保证重启后不会失效

4.2修改配置文件

[root@server ~]# vi /etc/sysconfig/network-scripts/ifcfg-eth0

MACADDR=00:0C:29:5C:0B:E0     #仅使用MACADDR配置项，而不使用HWADR配置项

网卡(nic)配置别名(aliases)

使用别名(aliases),单个nic设备可具备多个地址

网卡别名被标为：

eth0:1,eth0:2等

只能使用静态的IP地址

解析器

本地解析器:/etc/host

远程解析器:/etc/resolv.conf

解析器使用顺序：/etc/nsswitvh.conf

动态路由软件包

Quagg 支持 BGP4, BGP4+, OSPFv2, OSPFv3, RIPv1, RIPv2, 和 RIPng.

Quagg 的意图是被用作路由服务器和路由反射器。它不是一个工具包，它在一个新体系下提供完全的选路能力。Quagg 设计中，每个协议都有一个进程。

Quagga 是 GNU Zebra 的一支。

开启转发等功能

[root@node ~]# vi /etc/sysctl.conf   #开启转发功能

[root@node ~]# /proc/net

[root@node ~]# sysctl -w net.ipv4.ip_forward=1

查看相关硬件信息

[root@node ~]# vi /etc/sysconfig/hwconfig  #修改相关硬件信息

网络配置侧写

查看侧写

[root@node ~]# ls /etc/sysconfig/networking/profiles/

default

1.运行时加载

[root@node ~]# system-config-network-cmd --profile ProfileName --activate # --profile  指定侧写文件，--activate启用；

2.kernel启动时加载

[root@node ~]# cat /boot/grub/grub.conf   #kernel启动时加载

title Red Hat Enterprise Linux Server (2.6.18-194.el5)

       root (hd0,0)

       kernel /vmlinuz-2.6.18-194.el5 ro root=/dev/VolGroup00/LogVol00 rhgb quiet  netprofile=ProfileName

       initrd /initrd-2.6.18-194.el5.img

3.多侧写管理

NetworkManger  服务

nm-applet          交互工具

++++++++++++++++++++++++++++++

IPv6

禁用/启用自动载入IPv6模块

/etc/modprobe.conf：

alias net-pf-10 off/on

alias ipv6 off/on

手动载入模块

#modprobe ipv6

#lsmod |grep ipv6

禁用/启用配置项

/etc/sysconfig/network                           NETWORKING_IPV6=yes

/etc/sysconfig/network-scripts/ifcfg-ethX  IPV6INIT=yes

1.无状态自动配置（和IPV4 零配置相同）：如果模块被载入，活动接口就会分配到默认地址

2.动态接口配置，有两种动态配置IPV6地址的方法:

2.1.路由器广告守护进程

           在基于LINUX的路由器上运行 - radvd守护进程来进行广告

           只指定前缀和默认网关

           使用IPV6_AUTOCONF=yes在接口配置文件中启用

2.2.DHCP version 6

            由dhcp6s  #服务器DHCP后台程序# 和 dhcp6r  #DHCPv6中间代理程序组成#

            使用DHCPV6C=yes在接口配置文件中启用

            使用/sbin/dhcp6c 客户端程序获取地址

3.静态接口配置

/etc/sysconfig/network-scripts/ifcfg-ethX

IPV6ADDR=&lt;ipv6_address&gt;[/prefix_length]

IPV6ADDR_SECONDARIES=&lt;ipv6_address&gt;[/prefix_length] [...]   #相当与IPv4的别名配置eth0:1

e.g.:

IPV6ADDR=fe80::20c:29ff:fe6a:16d2/64

IPV6ADDR_SECONDARIES=fe80::20c:29ff:fe6a:16dc/64

4.路由配置

1.默认网关

根据radvd or dhcpv6s动态进行

在 /etc/sysconfig/network中手工指定

IPV6_DEFAULTGW=&lt;IPv6_address&gt;

IPV6_DEFAULTDEV=&lt;interface&gt;  #只对点对点接口有效

2.静态路由

配置文件中添加：

/etc/sysconfig/network-scripts/route6-ethX ：

&lt;ipv6_network/prefix&gt; via &lt;ipv6_routeraddress&gt;

运行时添加：

[root@node ~]# ip -6 route add &lt;ipv6_network/prefix&gt; via &lt;ipv6_routeraddress&gt;

5.IPv6工具

ping6 -I #ping6必须使用 -I 选项指定接口

traceroute6

tracepath6

ip -6

host -t AAAA domain

::1/128 #127.0.0.1/24#[::1]

::/128  #0.0.0.0     #[::0]

++++++++++++++++++++++++++++++++

禁用ipv6

net.ipv6.conf.default.disable_ipv6 = 1

++++++++++++++++++++++++++++++++