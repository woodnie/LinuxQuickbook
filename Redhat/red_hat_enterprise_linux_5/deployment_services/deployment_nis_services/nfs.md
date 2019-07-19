#### nfs {#nfs}

nfs

服务侧写：

类型: 系统 V （System V）

软件包: nfs-utils

守护进程: rpc.nfsd, rpc.lockd, rpciod, rpc.mountd, rpc.rquotad, rpc.statd

脚本: /etc/init.d/nfs, /etc/init.d/nfslock

端口: 2049(nfsd), 其它端口由portmap (111) 分配

配置文件: /etc/exports

相关软件包: portmap (必需), tcp_wrappers

NFS server 端：

1.确认系统中安装了以下两个包

[root@server ~]# yum -y install nfs-utils portmap

3.确保NFS服务启动

[root@server ~]# chkconfig nfs on

[root@server ~]# chkconfig nfslock on

4.确保portmap服务开启(客户机通过portmap服务访问共享目录)

[root@server ~]# chkconfig portmap on

[root@server ~]# service portmap status

查看portmap端口

[root@server ~]# rpcinfo -p &lt;host&gt;

[root@server ~]# netstat -ntupol|grep portmap

2.编译nfs服务的配置文件,把/nfsshare目录共享出来,允许192.168.0.0/24和192.168.1.0/24这两个网段的用户可以访问

[root@server ~]# chown /nfsshare nfsnodody:nfsnobody  #设置该目录拥有组为nfs组

[root@server ~]# chmod 770                                         #设置nfsnobody组拥有读写权限

[root@server ~]# vi /etc/exports

/nfsshare   192.168.0.0/24(rw,sync) 192.168.1.0/24(ro,async)

/nfsshare   192.168.1.0/255.255.255.0(rw,sync) 192.168.1.0/24(ro,async)

/etc/exports 文件格式：

[共享目录] [主机1或IP1（参数1，参数2）] [主机2 或IP2(参数3，参数4)]

#参数

#rw:可读写

#ro:可只读

#no_root_squash： 如果用户是root,则具有root权限

#root_squash：    如果是 root，会被压缩成nobody的权限

#all_squash：     无论是任何身份，会被压缩成nobody的权限

#anonuid：        设置匿名用户的uid

#anongid：        设置匿名用户的GID

#sync：           数据同步写磁盘

#async：          数据先暂存于内存中

5.确保rpc服务开启

5.验证客户机是否可以获得NFS服务器的共享目录

[root@server ~]# showmount -e 192.168.0.254     ##NFS服务器的IP

NFS Client端：

6.客户机挂载共享目录

NFS共享在引导时被/etc/init.d/netfs挂载

/etc/init.d/netfs会挂载被配置为引导时挂载的任何网络文件系统

6.1客户机手动挂载

[root@serverA ~]# mount 192.168.0.254:/nfsshare  /mnt

6.1客户机挂载自动挂载

添加nfs条目到/etc/fstab文件，实现开机自动挂载

[root@serverA ~]# echo &quot;192.168.0.254:/nfsshare    /mnt        nfs        default 0 0&quot; &gt;&gt; /etc/fstab

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

7.exportfs相关命令

[root@server ~]# exportfs -v                 #冗长列出/etc/exports信息

/home/nfstest   192.168.1.0/255.255.255.0(rw,wdelay,root_squash,no_subtree_check,anonuid=65534,anongid=65534)

/nfsshare       192.168.1.0/255.255.255.0(rw,wdelay,root_squash,no_subtree_check,anonuid=65534,anongid=65534)

[root@server ~]# exportfs -a  

[root@server ~]# exportfs -u                 #启用或禁用NFS当前共享目录

[root@server ~]# exportfs -e  &lt;host&gt;   #显示&lt;主机&gt;中的可用共享

[root@server ~]# exportfs -r                  #验证配置文件是否正确；在不重启服务的情况下，重新加载export文件

[root@server ~]# service nfs reload

iptable:

#查看服务使用那些端口：

#grep -E &quot;portmap|nfs&quot; /etc/services

tcp_wrappers

检查nfs程序是否链接libwrap.so

#strings $(which portmap)  |grep hosts

selinux:

查看nfs/portmap是否是selinux目标

semanage fcontext -l |grep -E &quot;nfs|portm&quot;

查看selinux布尔值

[root@node5 ~]# getsebool -a |grep -E &quot;nfs|portm&quot;

[root@node5 ~]# man -k nfs |grep selinux

troubleshooting

1.tcpwarppers

/etchosts.allow

portmap:192.168.1.0/255.255.255.0:spawn echo &quot;`date` %h allow %d access&quot; &gt;&gt; /var/log/tcp_wrapper

mountd:192.168.1.0/255.255.255.0:spawn echo &quot;`date` %h allow %d access&quot; &gt;&gt;  /var/log/tcp_wrapper

2.iptables

/etc/sysconfig/nsf 中指定portmap其他端口端口

ACCEPT     tcp  --  192.168.1.0/24       0.0.0.0/0           tcp dpt:2049   #nfsd

ACCEPT     udp  --  192.168.1.0/24       0.0.0.0/0           udp dpt:2049

ACCEPT     tcp  --  192.168.1.0/24       0.0.0.0/0           tcp dpt:111    #portmaper

ACCEPT     udp  --  192.168.1.0/24       0.0.0.0/0           udp dpt:111

3.selinux

nfs 共享目录需要有nfs selinux