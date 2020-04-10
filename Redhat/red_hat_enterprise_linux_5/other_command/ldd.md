### ldd {#ldd}

ldd

ldd 用来查看程序运行所需的共享库,显示可执行模块的dependency

Exp:

查看某服务是否支持支持tcpwrappers

   [root@localhost ftp]# ldd /usr/sbin/vsftpd  |grep libwrap                  ##查看vsftpd服务是否支持tcpwrappers

   libwrap.so.0 =&gt; /usr/lib/libwrap.so.0 (0x00110000)                         ##有该项证明sftpd服务支持tcpwarappers