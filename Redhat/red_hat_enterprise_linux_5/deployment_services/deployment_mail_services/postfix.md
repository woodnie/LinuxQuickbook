#### postfix {#postfix}

类型: 系统V （System V）管理的服务

软件包: postfix

守护进程: /usr/libexec/postfix/master and others

脚本: /etc/init.d/postfix

端口: 25 (smtp)

配置文件: /etc/postfix/main.cf and others

相关软件: procmail

[root@node6 ~]# /etc/init.d/sendmail stop

[root@node6 ~]# chkconfig sendmail off

[root@node6 ~]# yum isntall -y postfix.x86_64

[root@node6 ~]# cp -a /etc/postfix/ /tmp/postfix.orig

[root@node6 ~]# alternatives --config mta  #设置系统MTA为postfix

修改配置：

1.postconf命令

#man postconf

#man 5 postconf

#postconf -d  #显示默认配置:

#postconf -n #显示当前的非默认设置

#postconf -e key=value  #修改 main.cf

#postconf -m  #显示支持的映射类型

2.修改配置文件

[root@node6 ~]# vi /etc/postfix/main.cf

mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain  #检查主机名，最好使用FQDN

myorigin = $mydomain  #域名伪装的设置

inet_interfaces = all  #开启监听所有接口

always_bcc = comliance #address  #归档所有消息

[root@node6 ~]# vi /etc/postfix/main.cf

smtpd_client_restrictions = check_client_access hash:/etc/postfix/access

[root@node6 ~]# vi /etc/postfix/access

127.0.0.1         OK

192.168.0.0/24 OK

192.168.1.0/24 OK

0.0.0.0             REJECT

[root@node6 ~]# postmap /etc/postfix/access      #重建散列

[root@node6 ~]# postmap -s /etc/postfix/access  #查看

[root@node6 ~]# /etc/init.d/postfix restart

[root@node6 ~]# mutt -f /var/mail/user

[root@node6 ~]# mail -v user@doamin.tld

设置入站别名

1.本地别名/etc/aliases

对于sendmail:

在sendmail决定消息的接受者的目的地的之前，其先试图在别名中查找。sendmail的主要的别名配置文件是/etc/aliases。为了优化查找，sendmail为其别名记录建立了一个哈希表数据库/etc/aliases.db.该文件通过newalias命令产生（该命令是 sendmail -bi的同名）

#添加用户

student0:student0

student1:student1

student2:student2

student3:student3

student4:student4

student5:student5

student6:student6

student7:student7

student8:student8

student9:student9

#newalias            #更新数据库，尝试发送邮件给您定义的收件人：

echo &quot;test&quot; | mail -s &quot;hello&quot; student

[root@node6 ~]# mutt -f /var/mail/user

[root@node6 ~]# mail -v user@doamin.tld

2.虚拟别名

1.在 main.cf中启用

virtual_alias_maps = hash:/etc/postfix/virtual    #与Sendmail相同的格式定义

3\. 重散列文件：postmap /etc/postfix/virtual

出战地址重编：

[root@node6 ~]# vi /etc/postfix/main.cf

smtp_generic_maps = hash:/etc/postfix/generic

student0 student@node6.domain.org

student1 student@node6.domain.org

.....

##student0-9 发出的邮件，发件人都为student

[root@node6 ~]# postmap /etc/postfix/generic

[root@node6 ~]# /etc/init.d/postfix restart

Postfix 操作

查看 SMTP 交换: mail -v user@domain.tld

查看延期的消息: postqueue -p

发送延期消息: postqueue -f

查看日志: tail -f /var/log/maillog