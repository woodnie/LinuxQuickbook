### dhcp {#dhcp}

DHCP

[root@dhserver~]# rpm -ivh dhcp-3.0.5-23.el5.i386.rpm    

[root@node1 ~]# cat /etc/dhcpd.conf

ddns-update-style interim;

ignore client-updates;

allow booting;    # 定义可以PXE    启动

allow bootp;      #定义支持boottp

subnet 10.0.0.0 netmask 255.255.255.0 {

option routers                  10.0.0.1;  #定义默认网关

option subnet-mask              255.255.255.0;  

option domain-name-servers      8.8.8.8;      #定义NameServer

option time-offset              -18000;       #Eastern Standard Time

range dynamic-bootp 10.0.0.100 10.0.0.200;

default-lease-time 21600;

max-lease-time 43200;

# Group the PXE bootable hosts together  定义可以PEX启动的主机的组

       group {

               # PXE-specific configuration directives...

               next-server 10.0.0.2;    #TFTPServer的IP

               filename &quot;/pxelinux.0&quot;;       #pxelinux loader文件位置

              }

                                       }

[root@dhclint ~]# dhclient eth0