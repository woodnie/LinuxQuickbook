#### autofs {#autofs}

autofs

客户机自动挂载服务器目录

准备：

测试用户：student*

用户家目录：/rhome/student*

1.安装相关包

[root@serverA ~]# rpm -qa |grep autofs

autofs-5.0.1-0.rc2.55

2.修改配置文件

[root@client ~]# vi /etc/auto.master

/misc   /etc/auto.misc

/net    -hosts

/rhome /etc/auto.misc                          #添加/rhome目录

[root@client~]# vi /etc/auto.misc

*       192.168.1.15:/rhome/&amp;              #添加nis用户家目录

e.g.:

server /etc/exports

/nfs_share     192.168.1.0/24(rw,sync,no_root_squash)

/data/pub       192.168.1.0/24(rw,sync)

/home/          192.168.1.0/24(rw,sync)

e.g.:

client   indirect   mounts

/etc/auto.master文件:

/misc    /etc/auto.misc

/home   /etc/auto.misc

/data     /etc/auto.misc

/etc/auto.misc文件：

pub          -fstype=nfs              192.168.1.201:/data/pub      # mount     192.168.1.201:/data/pub    /data/pub

*              -rw,soft,intr              192.168.1.201:/home/&amp;        # mount     192.168.1.201:/home/*       /home/*

/etc/auto.master文件:

/misc     /etc/auto.misc

/home   /etc/auto.account

/etc/auto.account文件:

*       -rw,soft,intr   192.168.1.201:/home/&amp;

e.g.:

client  direct   mounts:

/etc/auto.master:

/-            /etc/auto.direct

/etc/auto.direct:

/nfs_share          server:/nfs_share

/data                   server:/data