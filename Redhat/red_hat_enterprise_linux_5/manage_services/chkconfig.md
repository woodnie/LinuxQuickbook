### chkconfig {#chkconfig}

chkconfig

NAME

      chkconfig - updates and queries runlevel information for system services

SYNOPSIS

      chkconfig --list [name]

      chkconfig --add name

      chkconfig --del name

      chkconfig [--level levels] name &lt;on|off|reset|resetpriorities&gt;

      chkconfig [--level levels] name

[root@node1 ~]# chkconfig   &lt; 独立服务&gt; on/off          #在runlevel2345中启用/禁用服务

[root@node1 ~]# chkconfig   &lt;独立服务&gt; reset

[root@node1 ~]# chkconfig   &lt;独立服务&gt; --add           #确保为每个运行级别都设置了停止或启动符号链接

[root@node1 ~]# chkconfig   &lt;独立服务&gt; --del            #删除 /etc/rtc.d/rc[0123456].d/ 目录层次中的所有符号链接

[root@node1 ~]# chkconfig   &lt;瞬态服务&gt; on/off          #同时重载xinetd

[root@node1 ~]# grep ^#[[:space:]]chkconfig:[[:space:]][[:digit:]]\+ /etc/init.d/*   #默认在系统引导时运行

/etc/init.d/acpid:# chkconfig: 345 26 74       #start priority should be 26, and that its stop priority should be 74

/etc/init.d/anacron:# chkconfig: 2345 95 05

/etc/init.d/apmd:# chkconfig: 2345 26 74

................

[root@node1 ~]# grep ^#[[:space:]]chkconfig:[[:space:]]-     #默认在系统引导时不运行