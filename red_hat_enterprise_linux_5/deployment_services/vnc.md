### vnc {#vnc}

配置vnc

1.安装vnc

[root@node1 ~]# rpm -ivh vnc-server....#server端

[root@node1 ~]# rpm -ivh vnc....            #client端

[root@node1 ~]# vncserver   #启动vnc server端

You will require a password to access your desktops.

Password:

Verify:

xauth: (stdin):1:  bad display name &quot;node1:1&quot; in &quot;add&quot; command

New node1:1 (root) desktop is node1:1

Creating default startup script /root/.vnc/xstartup

Starting applications specified in /root/.vnc/xstartup

Log file is /root/.vnc/node1:1.log

2.修改默认配置

默认配置文件：（默认只支持twn桌面环境）

[root@node1 ~]# cat /root/.vnc/xstartup

#!/bin/sh

# Uncomment the following two lines for normal desktop:

# unset SESSION_MANAGER    

# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] &amp;&amp; exec /etc/vnc/xstartup

[ -r $HOME/.Xresources ] &amp;&amp; xrdb $HOME/.Xresources

xsetroot -solid grey

vncconfig -iconic &amp;

xterm -geometry 80x24+10+10 -ls -title &quot;$VNCDESKTOP Desktop&quot; &amp;

twm &amp;    

修改后的文件：

[root@node1 ~]# cat /root/.vnc/xstartup            

#!/bin/sh

# Uncomment the following two lines for normal desktop:

  unset SESSION_MANAGER   #去掉注释

  exec /etc/X11/xinit/xinitrc           #去掉注释

[ -x /etc/vnc/xstartup ] &amp;&amp; exec /etc/vnc/xstartup

[ -r $HOME/.Xresources ] &amp;&amp; xrdb $HOME/.Xresources

xsetroot -solid grey

vncconfig -iconic &amp;

xterm -geometry 80x24+10+10 -ls -title &quot;$VNCDESKTOP Desktop&quot; &amp;

gnome-session &amp;      #更改为对其他桌面环境的支持，本实验设置为gnome的支持

                                   #支持gnome桌面环境：gnome -session &amp;

                                   #支持kde桌面环境      ：kde-session &amp;

3.配置用户桌面

a)手动启动多个VNC桌面

vncserver :1

vncserver :2

vncserver :3

……

这种手工启动的方法在服务器重新启动之后将失效

b)在配置文件中添加多个VNC桌面

vi /etc/sysconfig/vncservers  #在文件末行添加

VNCSERVERS=&quot;1:root&quot;                                            #桌面号1，用户root

VNCSERVERS=&quot;2:user&quot;                                           #桌面号2，用户user

……

VNCSERVERARGS[1]=&quot;-geometry 1024x768&quot;     #用户桌面设置

VNCSERVERARGS[2]=&quot;-geometry 1024x768&quot;     #用户桌面设置

……

4.重启VNC服务

[root@client ~]# /etc/init.d/vncserver restart   #

5.设置VNC服务随系统启动自动加载

第一种方法：使用&quot;ntsysv&quot;命令启动图形化服务配置程序，在vncserver服务前加上星号，点击确定，配置完成。

第二种方法：使用&quot;chkconfig&quot;在命令行模式下进行操作

[root@node1 ~]# chkconfig vncserver on

[root@node1 ~]# chkconfig --list vncserver

6.客户端链接vnc server

客户端方式：[root@node1 ~]# vncview                 vnc-server:screen

客户端方式：[root@node1 ~]# vncview   -viewonly vnc-server:screen   #以只读方式访问vncserver

客户端方式：[root@node1 ~]# vncview   -via   user@vnc-server   vnc-server:screen

浏览器方式： [http://vnc-server-ipaddr:num(num=5800+](http://vnc-server-ipaddr:num(num=5800+) 桌面号)  #需java支持

7.其他操作

[root@node1 ~]# vncserver -kill  :桌面号    #

[root@node1 ~]# vncpasswd   #修改vncserver密码