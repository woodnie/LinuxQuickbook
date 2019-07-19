#### erro info {#erro-info}

1.

&quot;ESC is already running, but is not responding. To open a new window, you must first close the existing ESC process, or restart your system.&quot;

此問題產生原因與智能卡有關，如果不需要可通過 yum remove esc 刪除之，以徹底解決此問題！[root@erptest01 ~]# chkconfig pcscd off

[root@erptest01 ~]# service pcscd stop

Stopping PC/SC smart card daemon (pcscd):                  [  OK  ]

[root@erptest01 ~]# yum remove esc

2.

今天遇到nfs mount的问题,以前从未遇到过,问题出的很奇怪,现象是 mount: 10.1.10.22:/cicro failed, reason given by server: Permission denied 系统权限根本没问题,最后检查到/etc/hosts文件,去掉/etc/hosts里添加的那些IP/主机名对就可以了。 对于增加了IP/主机名对后，mount报错的，当时我的猜测是因为NFS会先把IP地址转成对应的主机名，然后用这个主机名去匹配/etc /exports文件，而该文件都是设置IP段的，当然就没有权限mount。通过查阅资料和测试，证实了我的这个猜测。 另外才测试过程中，如果使用主机名或者全质量主机名(FQDN)来mount NFS 文件系统，会比单纯使用IP要快得多。因此，如果使用NFS服务的局域网内添加一个DNS服务，然后采用全质量主机名的方式来访问，应该效果会好得多。

3.

apache QFDN做主机名

4.

sendmail启动缓慢

/etc/hosts 文件

5.NIS

1、在做好之后遇到一个问题，客户端用yppasswd来更新密码时，一直报下面的错：

（1）yppasswd:yppasswdd not running on NIS master host

在服务器上运行该命令正常。

这是因为客户端不能解析到NIS服务器的IP地址，只要你在客户端的hosts文件里面添加IP对应关系即可。

（2）yppasswd:yppasswdd not running on NIS master host  (&quot;hostname&quot;).

这时候你ping下hostname看解析的为多少，如果为127.0.0.1的话，那就赶紧在/etc/hosts中加入IP  hostname，如果解析正常，自然不会报错了。

2、当启动ypbind 服务的时候，会出现以下的错误：

service ypbind start

  关联到 NIS 域：[确定]

  监听 NIS 域服务器。YPBINDPROC_DOMAIN: 未绑定域

  YPBINDPROC_DOMAIN: 未绑定域

这个问题一般是因为我们在配置yp.conf文件的时候没有配置正确的域和NIS服务器，所以启动在监听NIS服务器的时候会找不到NIS服务器，就会会出错，yp.conf中添加的内容的语法是： &quot;domain 域名称 server 主NIS服务器名称；从NIS服务器名称&quot;，千万不可写错哦。

6.

/etc/host

/etc/resolv.conf

/etc/nssitch

配置有问题影响网络

7.sshd:root@notty解决方法

很多人遇到如下问题，linux系统(假设是hosta)运行正常，但是用ssh user@hosta的时候一直等待，在hosta上有sshd:root@notty这样的进程(登陆用户不同可能显示不是root)，但是ssh user@hosta command却可以正常执行命令command。

问题出在sshd启动伪终端的时候，找不到/dev/ptmx这个设备文件或者/dev/pts这个文件系统没有mount。

解决方法是，首先检查是否有/dev/ptmx这个字符设备文件，如果没有用mknod /dev/ptmx c 5 2创建，然后用mount -t devpts devpts /dev/pts挂载/dev/pts文件系统