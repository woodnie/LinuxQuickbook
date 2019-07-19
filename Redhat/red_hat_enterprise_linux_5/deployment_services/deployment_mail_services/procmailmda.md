#### procmail(MDA) {#procmail-mda}

MDA (Mail Delivery Agent) 邮件递交代理

procmail

sendmail 默认启用

define(`PROCMAIL_MAILER_PATH, `/usr/bin/procmail)dnl

FEATURE(local_procmail, `, `procmail -t -Y -a $h -d $u)dnl

MAILER(procmail)dnl

postfix

1.postfix有自己MDA

2.postfix使用procmail :

[root@node6 ~]# vi /etc/postfix/main.cf

mailbox_command = /usr/sbin/procmail

或者：

[root@node6 ~]# postconf -e &quot;mailbox_command = /usr/sbin/procmail&quot;

Procmail 配置简介:

如果存在，配置文件会被依次处理

1./etc/procmailrc

2.~/.procmailrc

配置文件中的元素

指令: VERBOSE=yes

变量: LOGFILE=/var/spool/mail/procmail.log

组合

以 &quot;:0&quot; 开头的行和标志

使用正则表达式的零个或多个匹配

一个或多个动作行

默认忽略大小写

[root@node6 ~]# cat /etc/procmailrc                    

LOGFILE=/var/spool/mail/procmail.log

VERBOSE=yes

:0 c                                      # :0 开头

                                           # c  如果匹配成功procmail继续运行

* ^Subject.*rhce

/var/spool/mail/procmail.test   # procmail将消息保存这个文件中