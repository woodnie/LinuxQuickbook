#### selinux {#selinux}

selinux

1\. 启用/禁用SElinux

1.1.配置文件

/etc/selinux/config

/etc/sysconfig/selinux  #/etc/selinux/config符号链接

   ##selinux配置文件

  enforcing - SELinux security policy is enforced.

  permissive - SELinux prints warnings instead of enforcing.

  disabled - SELinux is fully disabled.

  其中: enforcing，(1)    SElinux运行在强制执行模式

           permissive，(0) SElinux运行在警告模式

           disabled，        SElinu完全禁用

注：更改selinux重启才会生效

selinux grub选项:

selinux=0 禁用

selinux=1 启用

1.2.配置工具

system-config-securitylevel  #更改模式，禁用需要重启生效

system-config-selinux         #可以修改boolleans

setroubleshootd                  #selinux故障排除

2.查看SElinux运行模式

  [root@localhost ~]# getenforce

3.设置selinux的状态

  [root@localhost ~]# setenforce

4.修改selinux  bool  值

   [root@localhost ~]# setsebool          #不带-P参数表示设置仅在当前状态起作用，重启后恢复为启动时设置值

   [root@localhost ~]# setsebool -P

e.g.:

   [root@localhost ~]#setsebool ftp_home_dir=1 #启用非匿名的ftp

4.2 togglesebool:给se的相关bool值取反

   e.g.:

   togglesebool httpd_enable_homedirs

5.security context

user:role:type:sensitivity:category

user(用户)：说明登录在系统上的用户类型

                 root用户类型：root

                 其他用户类型：user_u

                 进程的类型：system_u

role(角色)：定义某个用户，文件，进程的作用

                用户的角色：systyem_u（对于linux来说用户跟进程很相似）

                进程的角色：system_u

                文件的角色：object_u

type(类型)：被&quot;类型强制&quot;用来指定文件或进程中的数据的性质。策略中的规则表明哪个进程类型可以使用哪个文件类型。

sensitive：有时被政府机关使用的安全级别

category：和群组相似，但能够阻止root存取机密数据

5.1查看selinux的安全环境(security context)

   [root@localhost ~]# command    -Z

   exp:

   [root@localhost ~]# ls   -Z   #查看ls安全语境

   [root@localhost ~]# ps  -Z

5.2.修改selinux的安全环境

[root@localhost ~]# chcon   -t   security context    dir | file                      #修改dir / file 的security context值

[root@localhost ~]# chcon   --reference   源dir | file        目标dir | file       #把源文件的security context 应用到目标文件上

[root@localhost ~]# restorecon    dir | file                                              #自动应用对象的默认环境

e.g.:

5.3.semanage - SELinux Policy Management tool

[root@localhost ~]# semanage {login|user|port|interface|fcontext} -l

[root@localhost ~]# semanage {login|user|port|interface|fcontext} {-l|-a|-d|-m}

e.g:

[root@node5 ~]# semanage fcontext -l |grep squid

[root@node5 ~]# semanage fcontext -l | cut -d: -f3 | sort -u | grep named

SElinux中重要的布尔值：

   为了设置方便，SELinux中内建了许多布尔值参数，我们可以通过setsebool修改这些参数而直接更改一些SElinux的设置，下面列出了常用的服务器boolean设置参数：

1  DHCPD

关闭SElinux对dhcpd服务器的保护。

setsebool dhcpd_disable_trans 1

关闭SElinux对dhcpc服务器的保护。

setsebool dhcpc_disable_trans 1

2  BIND

允许主要domain name的设置被更改。

setsebool named_write_master_zones 1

关闭SELinux 对 DNS的保护。

setsebool named_disable_tran 1

3  HTTPD(Apache)

允许httpd执行cgi scripts。

setsebool httpd_enable_cgi 1

允许使用户存取自己的根目录。

setsebool httpd_enable_homedirs 1

最后要记得修改用户根目录的角色。

chcon -R -t httpd_sys_content_t ~user/public_html

关闭SElinux对httpd的保护。

setsebool httpd_disable_trans 1

4  VSFTPD

允许用户ftp到自己的家目录。

setsebool ftp_home_dir 1

若要vsftpd独立运行，而不挂在XINETD之下，这项数值必须设为1.

setsebool ftpd_is_daemon 1

关闭SELinux对ftpd的保护。

setsebool ftpd_disable_trans 1

5 SAMBAN

允许samba分享用户的根目录。

setsebool samba_enable_home_dirs 1

允许挂接远程的samba服务器用户的根目录。

setsebool use_samba_home_dirs 1

关闭selinux对samba的保护

setsebool smbd_disable_trans 1

6 NFS

允许NFS分享只读文件。

setsebool nfs_export_all_ro 1

允许NFS分享可擦写文件。

setsebool nfs_export_all_rw 1

允许NFS分享用户的根目录。

setsebool usr_nfs_home_dirs 1