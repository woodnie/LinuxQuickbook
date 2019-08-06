### tcp wrappers {#tcp-wrappers}

tcp_wrappers

1\. 支持tcpwrappers的服务

   [root@localhos ]# ldd /usr/sbin/vsftpd  |grep libwrap                  #查看vsftpd服务是否支持tcpwrappers

       libwrap.so.0 =&gt; /lib64/libwrap.so.0 (0x00002ab461696000)    #有该项证明sftpd服务支持tcpwarappers

   [root@localhos ]# ldd $(which vsftpd) |grep libwrap

   [root@locahost ]# ldd /usr/sbin/xinetd |grep libwrap    

       libwrap.so.0 =&gt; /lib64/libwrap.so.0 (0x00002b92efe43000)

2.配置文件

  tcp_wrappers防火墙有两个配置文件：

  /etc/hosts.allow               ##首先访问allow文件

  /etc/hosts.deny                ##随后访问deny文件

3.文件格式：

            daemon_list   ：client_list      [:action]

            守护进程列表    客户机列表    符合条件后做什么

3.1守护进程列表

e.g.：

/etc/hosts.allow   #allow中允许192.168.1.0/24网段

vsftpd:192.168.1.0/255.255.255.0

mountd,portmap:192.168.1.0/24 #允许192.168.1.0、/4网段的NFS访问

/etc/hosts.deny  

ALL:ALL

e.g.：主机带有多个网口

eth0 192.168.0.100

eth1 192.168.1.100

/etc/hosts.allow

in.telnetd,vsftp,httpd@192.168.0.100 : 192.168.0.0/24  

in.telnetd,vsftp,httpd@192.168.1.100 : 192.168.1.0/24

/etc/hosts.deny  

ALL:ALL

3.2客户机列表

按照 IP 地址 (192.168.0.1,10.0.0.)

按照域名 ( www.redhat.com , .example.com)

按照子网掩码 (192.168.0.0/255.255.255.0)

按照网络名(@mynetwork)

4.宏定义

主机名宏定义

   LOCAL

   KNOWN, UNKNOWN, PARANOID

主机和服务宏定义

   ALL

EXCEPT

   可用于客户和服务列表

   可嵌套

ALL：匹配所有主机和服务

LOCAL：名称中没有点的所有主机

UNKNOWN：所有无法解析的主机

KNOWN：所有能够解析的主机

PARANOID：所有正向和方向查询不匹配或者无法解析的主机名

5.spawn启动附加程序

e.g.:允许所有人使用vsftpd服务，并且对该访问进行日志记录

vi /etc/hosts.allow

vsftpd:ALL: spawn /bin/echo `date` %c %d&gt;&gt;/var/log/tcp_wrapper   #使用扩展选项记录日志

# man  hosts.allow

5.注：

   1.hosts.allow中缺省动作为allow

      hosts.deny 中缺省动作为deny

   2.只要找到匹配项，就不会经行继续查找

      没有被明确拒绝的，默认将允许访问

   3.所以安全配置应为：&quot;拒绝所有，允许个别主机或网段&quot;

   4.NFS,NIS使用tcp_wrappers需要使用底层portmap进程

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

基本语法:

daemon_list: client_list [:options]

高级语法:

daemon@host: client_list ...