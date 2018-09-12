### kickstart {#kickstart}

PXE

[root@node1 ~]# yum -y install  tftp* dhcp* nfs*

vsftp的配置：

httpd的配置：

nfs的配置：

boot:   linux  ks=ftp://10.0.0.2/pub/ks.cfg

boot:   linux  ks=http://10.0.0.2/pub/ks.cfg

boot:   linux  ks=nfs://10.0.0.2/pub/ks.cfg

tftp的配置：

[root@node1 ~]# cat /etc/xinetd.d/tftp

# default: off

# description: The tftp server serves files using the trivial file transfer \

#       protocol.  The tftp protocol is often used to boot diskless \

#       workstations, download configuration files to network-aware printers, \

#       and to start the installation process for some operating systems.

service tftp

{

       disable = no

       socket_type             = dgram

       protocol                    = udp

       wait                           = yes

       user                           = root

       server                        = /usr/sbin/in.tftpd

       server_args              = -u nobody -s /tftpboot/   ##添加nobody可以访问    -s 表示用/tftpboot作为tftp目录的根目录

       per_source               = 11

       cps                            = 100 2

       flags                           = IPv4

}

[root@node1 ~]# mount /dev/cdrom /mnt/                                                   #挂载安装盘

[root@node1 ~]# cp /mnt/images/pxeboot/vmlinuz      /tftpboot/              

[root@node1 ~]# cp /mnt/images/pxeboot/initrd.img   /tftpboot/

[root@node1 ~]# mkdir /tftpboot/pxelinux.cfg                                                             #PXE选单目录

[root@node1 ~]# cp /mnt/isolinux/isolinux.cfg /tftpboot/pxelinux.cfg/default            #PXE选单

[root@node1 ~]# cat /tftpboot/pxelinux.cfg/default

......

label PXE_NFS

 kernel vmlinz

 append initrd=initrd.img ks=nfs:10.0.0.2:/var/ftp/pub/ks.cfg

lable PXE_VSFTP

 kernel vmlinuz

 append initrd=initrd.img ks=ftp://10.0.0.2/pub/ks.cfg

[root@node1 ~]# service xinetd restart

dhcp的配置：

[root@node1 ~]# cat /etc/dhcpd.conf

ddns-update-style interim;

ignore client-updates;

allow booting;    #定义可以PXE    启动

allow bootp;      #定义支持boottp

subnet 10.0.0.2 netmask 255.255.255.0 {

option routers                        10.0.0.1;                                #定义默认网关

option subnet-mask              255.255.255.0;                    #定义子网掩码

option domain-name-servers      8.8.8.8;                          #定义NameServer  

option time-offset              -18000;                                     #Eastern Standard Time

range dynamic-bootp 10.0.0.100 10.0.0.200;                 #DHCP地址池

default-lease-time 21600;

max-lease-time 43200;

# Group the PXE bootable hosts together                       #定义可以PXE启动的主机的组

       group {

               # PXE-specific configuration directives...

               next-server 10.0.0.2;               #TFTPServer的IP

               filename &quot;/pxelinux.0&quot;;             #pxelinux loader文件位置

                   }

                                                              }

[root@node1 ~]# service dhcpd restart

boot:   linux  ks=http：//10.0.0.2/pub/ks.cfg