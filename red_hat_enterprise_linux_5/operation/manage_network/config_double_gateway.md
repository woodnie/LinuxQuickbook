#### config double gateway {#config-double-gateway}

由于电信和网通之间互联互通的问题，很多人选择双线路机房， 所谓双线路机房就是拥有两条出口，一条电信一条网通。最近在一个双线路机房测试一台服务器，打算作为论坛的数据库服务器使用，服务器操作系统为 Linux。计划配置为双IP，双域名，使得浏览者通过电信和网通两条线路都能正常访问服务器，而且各走各的，互不影响。在配置网络的时候遇到了问题，由 于Linux默认只有一个网关，在网络上查询了很久，找到一个解决方案，因此整理了一下。感谢原文作者jac003ke。

服务器操作系统RedHat linux 9，设置两张路由表

1\. vi /etc/iproute2/rt_tables，增加网通和电信两个路由表

251 tel   电信路由表

252 cnc 网通路由表

2\. 给网卡绑定两个地址用于电信和网通两个线路

ip addr add 192.168.0.2/24 dev eth0

ip addr add 10.0.0.2/24 dev eth1

3、分别设置电信和网通的路由表

电信路由表：

＃确保找到本地子网

ip route add 192.168.0..0/24 via 192.168.0.2 dev eth0 table tel

＃内部回环网络

ip route add 127.0.0.0/8 dev lo table tel

＃192.168.0.1为电信网络默认网关地址

ip route add default via 192.168.0.1 dev eth0 table tel

网通线路路由表:

＃确保找到本地子网

ip route add 10.0.0.0/24 via 10.0.0.2 dev eth1 table cnc

＃内部回环网络

ip route add 127.0.0.0/8 dev lo table cnc

＃10.0.0.1是网通的默认网关

ip route add default via 10.0.0.1 dev eth1 table cnc

4、电信和网通各有自己的路由表，制定策略，让192.168.0.2的回应数据包走电信的路由表路由，10.0.0.2的回应数据包走网通的路由表路由

ip rule add from 192.168.0.1 table tel

ip rule add from 10.0.0.1 table cnc