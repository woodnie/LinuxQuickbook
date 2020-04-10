### ethtool {#ethtool}

ethtool

NAME

      ethtool - Display or change ethernet card settings

注意：如果老网卡不支持ethtool时,需要使用options行或者执行install行中的/sbin/mii-tool指定模块参数，这两个数据都保存在/etc/modproboe.conf文件中

e.g.:修改双工设置

手动修改：

[root@node ~]# ethtool eth0  #查看eth0

Settings for eth0:

       Supported ports: [ TP ]

       Supported link modes:   10baseT/Half 10baseT/Full

                               100baseT/Half 100baseT/Full

                               1000baseT/Full

       Supports auto-negotiation: Yes                                      

       Advertised link modes:  10baseT/Half 10baseT/Full

                               100baseT/Half 100baseT/Full

                               1000baseT/Full

       Advertised auto-negotiation: Yes           #auto-negotiation:yes 启用自动协商

       Speed: 1000Mb/s

       Duplex: Full

       Port: Twisted Pair

       PHYAD: 0

       Transceiver: internal

       Auto-negotiation: on

       Supports Wake-on: d

       Wake-on: d

       Current message level: 0x00000007 (7)

[root@node ~]# ifdown eth0 #接口在down的情况，手动更改的效果更加，并非必须down

[root@node ~]# ethtool -s eth0 autonet off speed 100duplex full   ##自动协商关闭，速率100Mps,全双工通信

[root@node ~]# ifdown eth1

[root@node ~]# ethtool eth0

Settings for eth0:

       Supported ports: [ TP ]

       Supported link modes:   10baseT/Half 10baseT/Full

                               100baseT/Half 100baseT/Full

                               1000baseT/Full

       Supports auto-negotiation: Yes

       Advertised link modes:  Not reported              

       Advertised auto-negotiation: No     #auto-negotiation:no 关闭自动协商

       Speed: 1000b/s

       Duplex: Full

       Port: Twisted Pair

       PHYAD: 0

       Transceiver: internal

       Auto-negotiation: off

       Supports Wake-on: d

       Wake-on: d

       Current message level: 0x00000007 (7)

       Link detected: yes

修改配置文件：

/etc/sysconfig/network-script/ifcfg-eth0:

ETHTOOL_OPTS=&quot;autoneg off speed 100 duplex full&quot;   #自动协商关闭，速率100Mps,全双工通信

ETHTOOL_OPTS=&quot;autoneg off speed 100 duplex half&quot;   #自动协商关闭，速率100Mps,全双工通信