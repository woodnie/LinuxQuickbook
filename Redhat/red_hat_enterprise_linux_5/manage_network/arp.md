### arp {#arp}

用途说明

显示和修改地址解析协议(ARP)使用的&quot;IP 到物理&quot;地址转换表。手册页上的说法是&quot;操作系统ARP缓存&quot;，manipulate the system ARP cache。

常用参数

arp 显示当前的ARP缓存列表。

arp -s ip mac 添加静态ARP记录，如果需要永久保存，应该编辑/etc/ethers文件。

arp -f 使/etc/ethers中的静态ARP记录生效。

使用示例

示例一

[root@rhel55 src]# arp

Address                  HWtype  HWaddress           Flags Mask            Iface

192.168.6.106            ether   70:1A:04:CC:2B:21   C                     eth0

[root@rhel55 src]# ping 192.168.6.1

PING 192.168.6.1 (192.168.6.1) 56(84) bytes of data.

64 bytes from 192.168.6.1: icmp_seq=1 ttl=64 time=1.65 ms

64 bytes from 192.168.6.1: icmp_seq=2 ttl=64 time=0.888 ms

Ctrl+C

--- 192.168.6.1 ping statistics ---

2 packets transmitted, 2 received, 0% packet loss, time 999ms

rtt min/avg/max/mdev = 0.888/1.270/1.653/0.384 ms

[root@rhel55 src]# arp

Address                  HWtype  HWaddress           Flags Mask            Iface

192.168.6.1              ether   00:22:6B:8C:70:3D   C                     eth0

192.168.6.106            ether   70:1A:04:CC:2B:21   C                     eth0

[root@rhel55 src]# vi /etc/ethers

192.168.6.1     00:22:6B:8C:70:3D

[root@rhel55 src]# arp -f

[root@rhel55 src]# arp

Address                  HWtype  HWaddress           Flags Mask            Iface

192.168.6.1              ether   00:22:6B:8C:70:3D   CM                     eth0

192.168.6.106            ether   70:1A:04:CC:2B:21   C                     eth0

Flags Mask，C表示arp cache中的内容，M表示是静态ARP entry。

示例二

[root@web ~]# arp

Address                  HWtype  HWaddress           Flags Mask            Iface

218.23.142.41            ether   00:05:DC:E3:1F:BC   C                     eth1

192.168.6.12             ether   00:19:0F:03:37:D3   C                     eth0

218.23.142.42            ether   00:15:E9:47:AE:74   C                     eth1