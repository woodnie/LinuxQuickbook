#### config network {#config-network}

网络相关配置

[root@node ~]# system-config-network  #图形化配置工具

1.配置IP地址：

1.1临时配置IP

[root@server ~]# ifconfig &lt;interface&gt; &lt;address&gt; ## 例：ifconfig eth0 192.168.0.254

1.2修改配置文件

[root@server ~]# vi /etc/sysconfig/network-scripts/ifcfg-eth0

DEVICE=eth0                #网卡标识，eth0第一快网卡，eth0:0第一块网卡的第一个子接口

HWADDR=00:0C:29:5C:0B:E0   #HW地址

ONBOOT=yes                 #是否启用该网卡，yes启用，no禁用

BOOTPROTO=static           #static静态获取IP地址，dhcp动态获取IP地址 ，none

IPADDR=192.168.0.254       #IP地址

NETMASK=255.255.255.0      #子网掩码

GATEWAY=192.168.0.1        #默认网关

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

详细配置项：　　

DEVICE=name       #name表示物理设备的名字

IPADDR=addr       #addr表示赋给该卡的IP地址

NETMASK=mask      #mask表示网络掩码

NETWORK=addr      #addr表示网络地址

BROADCAST=addr    #addr表示广播地址

ONBOOT=yes/no     #启动时是否激活该卡

USERCTL=yes/no    #允许/不允许 非root用户控制该设备

BOOTPROTO= [none：无须启动协议 | static：静态路由协议 | dhcp：使用dhcp协议 | bootp：使用bootp协议]

PEERDNS=no        #即时在启用DHCP时，还是使用静态配置的DNS

NOZEROCONF=no     #启用零配置，即在无DHCP服务器时，使用166.254.0.0/16段地址

NOZEROCONF=yes    #禁用零配置

ETHTOOL_OPTS=&quot;autoneg off speed 100 duplex full&quot;   #自动协商关闭，速率100Mps,全双工通信

+++++++++++++++++++++++++++++++++++++++++++++++++++

[root@server ~]# service network restart  ##不要忘记重启网络

2.配置主机名：

2.1运行时配置主机名

[root@server ~]# hostname 主机名

2.2修改配置文件

[root@server ~]# vi /etc/sysconfig/network

HOSTNAME=host.domain.org

3.本地解析器:/etc/host

更改主机名同时最好更改一下/etc/hosts文件

IP_address canonical_hostname [aliases...]

192.168.0.1    node.domain.org         node

3.配置dns客户端

/etc/nsswitvh.conf  #管理解析器使用顺序

[root@server ~]# vi /etc/resolv.conf

search domain.org           #本地域(查询/etc/hosts文件)

nameserver 8.8.8.8           #主用DNS

nameserver 8.8.4.4           #备用DNS

5.route配置

5.1浏览路由表：

[root@node ~]# route

[root@node ~]# netstat -r

[root@node ~]# ip route

linux下添加路由的方法：

5.2使用 命令添加/删除静态路由

使用route 命令添加的路由，机器重启或者网卡重启后路由就失效了

//添加到主机的路由

# route add -host 192.168.168.110 dev eth0

# route add -host 192.168.168.119 gw 192.168.168.1

//添加到网络的路由

# route add -net IP netmask MASK dev ethX

# route add -net IP netmask MASK gw  IP

# route add -net IP/24 eth1

//添加默认网关

# route add default gw IP

//删除路由

# route del -host 192.168.168.110 dev eth0

5.3在linux下设置永久静态路由的方法：

1.在/etc/rc.local里添加

方法：

route add -net 192.168.3.0/24 dev eth0

route add -net 192.168.2.0/24 gw 192.168.3.254

2.在/etc/sysconfig/network里添加到末尾

方法：GATEWAY=gw-ip 或者 GATEWAY=gw-dev

3./etc/sysconfig/static-router :

any net x.x.x.x/24 gw y.y.y.y

4./etc/sysconf/network-scrip/route-ethX

192.168.0.0/24 via 192.168.0.1

e.g.:静态路由

host201 eth1    192.168.1.201

            gw      192.168.1.1

            eth1:0 10.0.201.1

host202 eth1    192.168.1.202

            gw      192.168.1.1

            eth1:0 10.0.202.1

host201# ip route add 10.0.202.0/24 via 192.168.1.1