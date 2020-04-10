### bonding {#bonding}

1\. 编辑虚拟网络接口配置文件,指定网卡IP

vi /etc/sysconfig/ network-scripts/ ifcfg-bond0

[root@rhas-13 root]# cp /etc/sysconfig/network-scripts/ifcfg-eth0 /etc/sysconfig/network-scripts/ifcfg-bond0

#vi ifcfg-bond0

将第一行改成 DEVICE=bond0

ONBOOT=yes

USERCTL=no

PEERDNS=yes

GATEWAY=

TYPE=Ethernet

IPADDR=

DEVICE=bond0

HWADDR=00:13:72:5c:d4:dc

BOOTPROTO=none

NETMASK=

这里要注意，不要指定单个网卡的IP 地址、子网掩码或网卡 ID。将上述信息指定到虚拟适配器(bonding)中即可。

[root@rhas-13 network-scripts]# vi ifcfg-eth0

DEVICE=eth0

USERCTL=no

ONBOOT=yes

MASTER=bond0

SLAVE=yes

BOOTPROTO=none

[root@rhas-13 network-scripts]# vi ifcfg-eth1

DEVICE=eth1

USERCTL=no

ONBOOT=yes

MASTER=bond0

SLAVE=yes

BOOTPROTO=none

2.编辑 /etc/modules.conf 文件，加入如下一行内容，以使系统在启动时加载bonding模块，对外虚拟网络接口设备为 bond0  

加入下列两行&lt; BR&gt;# vi /etc/modules.conf

alias bond0 bonding

options bond0 miimon=100 mode=1

3.最后编辑vi /etc/rc.local文件，加入如下内容:

ifenslavebond0 eth0eth1

重启服务器，使双网卡bond0地址生效。  

所有均完成后，最后重启计算机，用ifconfig查看，并且还可查看bond0状态,cat /proc/net/bonding/bond0

说明：

miimon=100

miimon是指多久时间要检查网路一次，单位是ms(毫秒)

这边的100，是100ms，即是0.1秒

意思是假设其中有一条网路断线，会在0.1秒内自动备援

mode共有七种(0~6)

mode=0：平衡负载模式，有自动备援，但需要&quot;Switch&quot;支援及设定。

mode=1：自动备援模式，其中一条线若断线，其他线路将会自动备援。

mode=6：平衡负载模式，有自动备援，不必&quot;Switch&quot;支援及设定。

需要说明的是如果想做成mode 0的负载均衡,仅仅设置这里options bond0 miimon=100 mode=0是不够的,与网卡相连的交换机必须做特殊配置（这两个端口应该采取聚合方式），因为做bonding的这两块网卡是使用同一个MAC地址.

从原理分析一下（bond运行在mode 0下）：

mode 0下bond所绑定的网卡的IP都被修改成相同的mac地址，如果这些网卡都被接在同一个交换机，那么交换机的arp表里这个mac地址对应的端口就有多 个，那么交换机接受到发往这个mac地址的包应该往哪个端口转发呢？正常情况下mac地址是全球唯一的，一个mac地址对应多个端口肯定使交换机迷惑了。

所以mode0下的bond如果连接到交换机，交换机这几个端口应该采取聚合方式（cisco称为 ethernetchannel，foundry称为portgroup），因为交换机做了聚合后，聚合下的几个端口也被捆绑成一个mac地址.我们的解 决办法是，两个网卡接入不同的交换机即可。

mode6模式下无需配置交换机，因为做bonding的这两块网卡是使用不同的MAC地址。

##################################

#defineBOND_MODE_ROUNDROBIN       0   （balance-rr模式）网卡的负载均衡模式

#defineBOND_MODE_ACTIVEBACKUP     1   （active-backup模式）网卡的容错模式

#defineBOND_MODE_XOR              2   （balance-xor模式）需要交换机支持

#defineBOND_MODE_BROADCAST        3    （broadcast模式）

#defineBOND_MODE_8023AD           4   （IEEE 802.3ad动态链路聚合模式）需要交换机支持

#defineBOND_MODE_TLB              5   自适应传输负载均衡模式

#defineBOND_MODE_ALB              6   网卡虚拟化方式

bonding模块的所有工作模式可以分为两类：多主型工作模式和主备型工作模式，balance-rr 和broadcast属于多主型工作模式而active-backup属于主备型工作模式。（balance-xor、自适应传输负载均衡模式（balance-tlb）和自适应负载均衡模式（balance-alb）也属于多主型工作模式，IEEE 802.3ad动态链路聚合模式（802.3ad）属于主备型工作模式

######################

多个bond设备

若您需要激活多个bond设备，例如bond0、bond1对应不用的网卡。配置方法略微有点不

同。

1、ifcfg-bondX的配置和单个bond的配置没有区别

2、修改modprobe.conf

有2种修改方法：

a) 当2个或者多个bond网卡的所有参数（即bonding模块的参数，如mode、miimon 等）都相同时，加载bonding模块时设置 max_bonds参数即可。如max_bonds=2时，加载bonding驱动之后可以创建2个bond网卡bond0，bond1，修改后的 modprobe.conf和下面的情形类似：

引用

....

alias bond0 bonding

alias bond1 bonding

options bond0 miimon=100 mode=1 max_bonds=2

....

b) 当2个或者多个bond网卡的参数（即bonding模块的参数，如mode、miimon等）不同时，需要在加载bonding模块时修改模块的名称（文档中的说法是linux的模块加载系统要求系统加载的模块甚至相同模块的不同实例都需要有一个唯一的命名），修改后的modprobe.conf和下面的情形类似：

引用

....

install bond0 /sbin/modprobe --ignore-install bonding -o bond0

miimon=100 mode=0

install bond1 /sbin/modprobe --ignore-install bonding -o bond1

miimon=100 mode=1

....