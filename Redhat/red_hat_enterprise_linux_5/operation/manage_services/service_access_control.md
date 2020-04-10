#### service access control {#service-access-control}

1\. 由 init 进程管理的系统资源

inittab 配置 init 程序对系统资源的管理

注：参考 inittab(5) agetty(8)

2.SysV 初始化脚本&quot;（ /etc/init.d ），用来控制系统启动和关闭， System V Interface Definition (SVID) 是一个 System V 如何工作的标准定义。

/etc/init.d/* 配置守护进程执行条件的可执行脚本

/etc/sysconfig/* 配置守护进程的执行方式

2.1chkconfig

chkconfig 仅仅维护 /etc/rc.d/rcX.d/ 目录里的符号链接和 xinetd 配置，并不控制他们

[root@node1 ~]# grep ^#[[:space:]]chkconfig:[[:space:]][[:digit:]]\+ /etc/init.d/*   # 默认在系统引导时运行

/etc/init.d/acpid:# chkconfig: 345 26 74       #start priority should be 26, and that its stop priority should be 74

................

[root@node1 ~]# grep ^#[[:space:]]chkconfig:[[:space:]]-     # 默认在系统引导时不运行

2.2xinetd

xinetd 管理瞬时服务

/etc/xinetd.conf              #xinetd 服务的配置文件

/etc/xinetd.d/&lt;service&gt;   #xinetd 服务所管理的瞬时服务配置文件

inetd 使用 /etc/serevices 文件配置对&quot;端口到服务&quot;的管理

瞬时服务，系统初始化启动：

1.xinetd 系统初始化启动

2.chkconfi &lt; 瞬间服务 &gt; on  # 瞬间服务系统初始化启动

e.g.:

[root@node1 ~]# cat /etc/xinetd.d/tftp

service tftp

{

       disable = no

       socket_type             = dgram

       protocol                = udp

       wait                    = yes

       user                    = root

       server                  = /usr/sbin/in.tftpd

       server_args             = -s /tftpboot

       per_source              = 11

       cps                     = 100 2

       flags                   = IPv4

       only_from = 192.168.0.0/24

       no_access = 192.168.0.254

}

xinetd 服务配置项：

disable = no xinetd 服务接受该服务的连接

disable = yes xinetd 服务不接受该服务的连接

server=     该项服务的二进制文件位置

xinetd 访问控制：

only_from = &lt;host_pattern&gt;   允许访问

no_access = &lt;host_pattern&gt;   禁止访问

可使用的主机掩码：

数字式地址： 192.168.1.1

ip/netmask:192.168.1.0/24

主机名或者域名： .domian.com

网络名： /etc/networks

per_source = &lt;number&gt;   并发连接数

3\. 服务的访问控制

tcp_wrappers

e.g. ：

查看某服务是否支持支持 tcpwrappers

   [root@localhost ftp]# ldd /usr/sbin/vsftpd  |grep libwrap                  ## 查看 vsftpd 服务是否支持 tcpwrappers

   libwrap.so.0 =&gt; /usr/lib/libwrap.so.0 (0x00110000)                         ## 有该项证明 sftpd 服务支持 tcpwarappers

4.iptables

6.

[root@node ~]# chkconfig --list| grep $(runlevel|cut -d&quot; &quot; -f2 ): 启用  # 查看随当期运行级别启用的服务

[root@node ~]# grep ^#[[:space:]]:chkconfig:[[:space:]][[:digit:]]\+\ /etc/init.d/*   # 默认在系统引导时运行的脚本

[root@node ~]# grep ^#[[:space:]]:chkconfig:[[:space:]]- /etc/init.d/*                 # 默认在系统引导时不运行的脚本