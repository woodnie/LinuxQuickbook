### network client tools {#network-client-tools}

网络客户端

web客户程序

1.FireFox

2.links

[root@node ~]# links               www.redhat.com

[root@node ~]# links  -dump   www.redhat.com     # -dump显示网页的文本

[root@node ~]# links -source www.redhat.com     # -source显示网页的源码

3.wget

邮件客户端程序

1.Evolution  #Gnome默认邮件客户端程序

2.Kmail            #KDE默认邮件客户端程序

3.Thunderbird #Mozilla邮件客户端程序

4.mail

5.mutt

即时消息客户程序

Gaim

OpenSSh

[root@node ~]# ssh user@host

[root@node ~]# ssh user@host  command

e.g.:

[root@node ~]# ssh wood@node1

[root@node ~]# ssh wood@node1  df -h   #通过ssh在远程主机上执行命令

scp

see scp

rsync

see rsync

see 配置rsync

FTP客户端

[root@node ~]# ftp         #基本的ftp客户端

[root@node ~]# lftp        #高级的ftp客户端

[root@node ~]# lftpget   [ftp://ftp.example.com/pub/file.txt](ftp://ftp.example.com/pub/file.txt)  #非交互的ftp客户端

gFTP  #图形化客户端

smbclient

Nautilus

Nautilus URL #浏览网络中的文件

Xorg客户程序

X客户（图形化应用程序）通过X协议和Xorg服务器通讯

X客户（图形化应用程序）完全网络透明

X客户（图形化应用程序）都能狗仔本地屏幕上显示

X客户（图形化应用程序）不加密数据，可以使用ssh的连接隧道安全进行

[root@node ~]# ssh -X root@host   xterm &amp;   #使用ssh 将xterm显示在远程主机host的X服务器上

网络分析工具

ping

tracerroute

host

dig

netstat

gnome-nettool