### xinetd {#xinetd}

xinetd

/etc/xinetd.conf

/etc/xinetd.d/&lt;service&gt;

以telnet为例：

1.install telnet

[root@node5 ~]# yum list telnet-server

[root@node5 ~]# yum list telnet

2.启动telnet服务

[root@node5 ~]# chkconfig telnet on

[root@node5 ~]# chkconfig --list

[root@node5 ~]# service xined restart

3\. 测试服务

默认只允许普通用户 　

若要允许root用户登入，可用下列方法：

[root@node5 ~]# vi /etc/pam.d/login

　　#auth required pam_securetty.so #将这一行加上注释！

　　或

[root@node5 ~]# mv /etc/securetty /etc/securetty.bak

4.设置Telnet端口

　　#vi /etc/services

　　进入编辑模式后查找Telnet(vi编辑方式下输入/Telnet)

　　会找到如下内容：

　　Telnet 23/tcp

　　Telnet 23/udp

　　将23修改成未使用的端口号(如：2000)，退出vi，重启Telnet服务，Telnet默认端口号就被修改了。

5.访问控制设置

5.1/etc/xinetd.d/telnet

service telnet

{

　　disable　　　　   = no #激活 Telnet 服务,no

　　bind 　　　　　   = 210.45.160.17 #your ip

　　only_from 　　　 = 210.45.0.0/16 #只允许 210.45.0.0 ~ 210.45.255.255 这个网段进入

　　only_from 　　　 = .edu.cn #只有教育网才能进入！

　　no_access 　　　= 210.45.160\. #这两个ip不可登陆

　　access_times　　= 8:00-12:00 20:00-23:59 # 每天只有这两个时间段开放服务

　　......

}

5.2/etc/xinetd.conf

disable = no xinetd服务接受该服务的连接

disable = yes xinetd服务不接受该服务的连接

server=    该项服务的二进制文件位置

only_from = &lt;host_pattern&gt;   允许访问

no_access = &lt;host_pattern&gt;  禁止访问