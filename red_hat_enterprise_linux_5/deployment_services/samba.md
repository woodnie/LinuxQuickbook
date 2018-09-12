### samba {#samba}

samba

类型: 系统 V （System V）管理的服务

软件包: samba, samba-common, samba-client

守护进程: /usr/sbin/nmbd, /usr/sbin/smbd

脚本: /etc/init.d/smb

端口: [NetBIOS] 137(-ns), 138(-dgm), 139(-ssn), [SMB over TCP] 445(-ds)

配置文件: /etc/samba/*

相关软件包:

可用的配置工具：

system-config-samba

samba-swat：使用 [http://localhost/901](http://localhost/901)  web页面进行配置,samba-swat会删除smb.conf中的所有注释

testpar：测试smb.conf中的语法错误

server端：

1.安装samba,samba-common

  [root@server ~]# service smb start

  [root@server ~]# chkconfig  smb  on

2.确定Samba是在正常工作，可以从服务器获得回应，但是并不代表文件共享可用

  [root@localhost~ ]# smbclient -L server -N      #列举服务器上的共享资源；  -L服务器列表，-N 不使用密码

3.测试进入共享目录（默认情况：此时应该只有用户家目录被共享出）

  [root@localhost ~]# smbclient //sambaserver/smbuser -U smbuser

4.创建samba共享

4.1创建samba共享目录

  [root@server ~]# mkdir /smbshare

  [root@localhost ~]# chown smbuser:smbuser /smnshare/

  [root@localhost ~]# chmod 775 /smbshare

  其他：setfacl设置smb共享目录其他用户访问权限

4.2设置smb.conf共享

  [root@localhost ~]# vi /etc/samba/smb.conf   #在 smb.conf中添加一下内容

      [smbshare]                     #共享访问点

      comment = samba share   #共享的描述

      path = /smbshare             #共享路径

      browseable = No    #共享是否可以被浏览

      public = yes          #是否可以匿名访问

      writable = yes        #是否可写

      write list = +staff,@student #可写用户列表，@引用组，+引用用户

      printable = no     #共享是否是打印  

      hosts allow =     #允许访问的主机

5.

5.1验证方式：

    security = user                  #使用smb的用户名密码文件 smb passwd file 验证（默认）

    security = share               #不需密码即可访问

    security = domain/server #带有一组验证数据的工作组

    security = ads #kerberos验证

5.2设置密码文件类型

     5.2.1

     passdb backend = tdbsam                                              #smb密码文件为passdb.tdb

     [root@localhost ~]# tdbdump /etc/samba/passdb.tdb        #生成密码数据库

     5.2.2

     #passdb backend = tdbsam                                  #注释掉db验证

     smb passwd file = /etc/samba/smbpasswd               #添加smb密码文件位置

     [root@localhost ~]# touch /etc/samba/smbpasswd   #手动建立密码文件

6.创建samba用户：

  [root@localhost~ ]# useradd -a test    #smb用户必须为系统用户

  [root@localhost~ ]# smbpasswd -a test

  －a　表示添加指定的用户帐号

  －d　表示禁止指定的用户帐号

  －e　表示启用指定的用户帐号

  －x　表示删除指定的用户帐号

  [root@localhost~ ]# pdbedit

  [root@localhost~ ]# pdbedit -L  #浏览smb用户数据库

7.测试：

  [root@localhost ~]# testparm /etc/samba/smb.conf   #测试smb.conf有无语法错误

  [root@localhost ~]# testparm /etc/samba/smb.conf netbios_name ip_addr     #显示访问权限

  [root@localhost ~]# testparm  -v  #显示smb默认配置

8.selinux相关

 [root@localhost ~]#smbclient //172.16.25.25/student -U student    #student 用户登录自己家目录

  若有如下提示信息：

  tree connect failed: NT_STATUS_BAD_NETWORK_NAME

  [root@localhost ~]# #tail /var/log/message                                 #系统log里会有selinux的报错，通常需要修改一下selinuxbool值：

  setsebool -P samba_enable_home_dirs=1    #即：开启samba共享家目录

  [root@localhost ~]# smbclient //192.168.1.254/smbshare    

  若有如下提示信息：

  tree connect failed: NT_STATUS_BAD_NETWORK_NAME

  [root@localhost ~]# #tail /var/log/message      #系统log里没有selinux的报错，但是会报告权限问题，通常需要修改selinux fcontext

  [root@localhost ~]# man -k sam

  [root@localhost ~]# man -k sam |grep selinux

  [root@localhost ~]# man samba_selinux

  [root@localhost ~]# manwhatis &amp;  #以上man命令报错活无结果是须执行此命令

  [root@localhost ~]# chcon -t samba_share_t /smbshare/

客户端配置：

1\. 安装samba-client

  [root@localhost ~]# rpm -q samba-client

  samba-client-3.0.28-0.el5.8

2.浏览客户端共享

 smb浏览windows共享目录

 [root@node5 ~]# smbclient -L //192.168.1.84

 [root@node5 ~]# smbclient -L //192.168.1.84   -U Administrator

 smb浏览linux共享

 [root@node5 ~]# smbclient -L //192.168.1.254

 [root@node5 ~]# smbclient -L //192.168.1.254   -N

3.访问共享目录

  [root@localhost ~]# smbclient //192.168.1.254/smbshare                         #guest(nobody)用户访问

  [root@localhost ~]# smbclient //192.168.1.254/smbshare -U username     #指定用户访问

  smbclient常用选项：

  -W  #工作组或域

  -U   #用户名

  -N   #不使用密码

4.挂载共享目录

  [root@client ~]# mount.cifs    //192.168.1.254/smbshare /mnt -o username=username,password=password

  [root@client ~]# mount -t cifs //192.168.1.254/smbshare /mnt -o username=username,password=password

  [root@client ~]# smbmount    //192.168.1.254/smbshare /mnt -o username=username,password=password

5./etc/fstab挂载

5.1\.  #smbpasswd nobody #设置密码为空

       /192.168.1.50/smbshare         /mnt/smbshare   cifs    username=nobody 0 0

5.2\.  /192.168.1.50/smbshare         /mnt/smbshare   cifs    username=wood,password=smb 0 0

5.3\.  /192.168.1.50/smbshare         /mnt/smbshare   cifs    credentials=/etc/samba/cre 0 0

       #cat /etc/samba/cre

       username=wood

       passwoed=smb

5.4\.   /192.168.1.50/smbshare         /mnt/smbshare   cifs    username=wood,noauto 0 0

windows系统访问linux下的共享

在windows查看linux下的共享文件就很方便了，在文件浏览器里直接输入 \\IP 就可以直接查看文件内容了，

比如 \\192.168.1.254 输入用户名和密码，这里用户名和密码就是在开始设置的samba用户名和密码

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

ACCEPT     tcp  --  192.168.1.0/24       0.0.0.0/0           tcp dpt:445

ACCEPT     udp  --  192.168.1.0/24       0.0.0.0/0           udp dpt:445

ACCEPT     tcp  --  192.168.1.0/24       0.0.0.0/0           tcp dpt:139

ACCEPT     udp  --  192.168.1.0/24       0.0.0.0/0           udp dpt:139

ACCEPT     tcp  --  192.168.1.0/24       0.0.0.0/0           tcp dpt:138

ACCEPT     udp  --  192.168.1.0/24       0.0.0.0/0           udp dpt:138

ACCEPT     tcp  --  192.168.1.0/24       0.0.0.0/0           tcp dpt:137

ACCEPT     udp  --  192.168.1.0/24       0.0.0.0/0           udp dpt:137