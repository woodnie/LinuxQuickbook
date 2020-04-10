#### /etc/syslog.conf {#etc-syslog-conf}

linux 系统日志

1.日志配置文件syslog

klogd解读内核信息，并将其传递给syslogd，再由syslogd提供系统日志

/etc/syslog.conf          #syslogd 的配置文件

/etc/sysconfig/syslog  #设定从系统V初始化脚本中启动syslogd和klogd的选项

/etc/syslog.conf根据如下的格式定义规则：

facility.level   action

设备.优先级  动作

facility.level 字段也被称为seletor（选择条件），选择条件和动作之间用空格或tab分割开。

1、facility

facility定义日志消息的范围，其可使用的key有： auth －由 pam_pwdb 报告的认证活动。

authpriv －包括特权信息如用户名在内的认证活动

cron －与 cron 和 at 有关的计划任务信息。

daemon －与 inetd 守护进程有关的后台进程信息。

kern －内核信息，首先通过 klogd 传递。

lpr －与打印服务有关的信息。

mail －与电子邮件有关的信息

mark － syslog内部功能用于生成时间戳

news －来自新闻服务器的信息

syslog －由 syslog 生成的信息

user －由用户程序生成的信息

uucp －由 uucp 生成的信息

local0-local7 －与自定义程序使用

* 通配符代表除了 mark 以外的所有功能除mark为内部使用外，还有security为一个旧的key定义，等同于auth，已经不再建议使用。

2、level级别

level定义消息的紧急程度。按严重程度由高到低顺序排列为： emerg －该系统不可用，等同panic

alert －需要立即被修改的条件

crit －阻止某些工具或子系统功能实现的错误条件

err －阻止工具或某些子系统部分功能实现的错误条件，等同error

warning －预警信息，等同warn

notice －具有重要性的普通条件

info －提供信息的消息

debug －不包含函数条件或问题的其他信息

none －没有重要级，通常用于排错

* 所有级别，除了none其中，panic、error、warn均为旧的标识符，不再建议使用。

在定义level级别的时候，需要注意两点： 1）优先级是由应用程序在编程的时候已经决定的，除非修改源码再编译，否则不能改变消息的优先级

低的优先级包含高优先级，例如，为某个应用程序定义info的日志导向，则涵盖notice、warning、err、crit、alert、emerg等消息。（除非使用=号定义）

3、selector选择条件

通过小数点符号&quot;.&quot;把facility和level连接在一起则成为selector（选择条件）。

可以使用分号&quot;;&quot;同时定义多个选择条件。也支持三个修饰符： * － 所有日志信息

= － 等于，即仅包含本优先级的日志信息

! － 不等于，本优先级日志信息除外

4、action动作

由前面选择条件定义的日志信息，可执行下面的动作： file－指定日志文件的绝对路径

terminal 或 print －发送到串行或并行设备标志符，例如/dev/ttyS2

@host －远程的日志服务器

username －发送信息本机的指定用户信息窗口中，但该用户必须已经登陆到系统中

named pipe －发送到预先使用 mkfifo 命令来创建的 FIFO 文件的绝对路径※注意，不能通过&quot;|/var/xxx.sh&quot;方式导向日志到其他脚本中处理。

5、举例

*.info;mail.none;news.none;authpriv.none;cron.none /var/log/messages

#把除邮件、新闻组、授权信息、计划任务等外的所有通知性消息都写入messages文件中。

mail,news.=info /var/adm/info

#把邮件、新闻组中仅通知性消息写入info文件，其他信息不写入。

mail.*;mail.!=info /var/adm/mail

#把邮件的除通知性消息外都写入mail文件中。

mail.=info /dev/tty12

#仅把邮件的通知性消息发送到tty12终端设备

*.alert root,joey

#如果root和joey用户已经登陆到系统，则把所有紧急信息通知他们

*.* @finlandia

#把所有信息都导向到finlandia主机（通过/etc/hosts或dns解析其IP地址）※注意：每条消息均会经过所有规则的，并不是唯一匹配的。

也就是说，假设mail.=info信息通过上面范例中定义的规则时，/var/adm/info、/var/adm/mail、/dev /tty12，甚至finalandia主机都会收到相同的信息。这样看上去比较烦琐，但可以带来的好处就是保证了信息的完整性，可供不同地方进行分析。

2.日志格式

系统日志文件的每个条目都有四个部分：

信息的时间和日期

信息来源的主机名

信息来源的应用程序或其子程序名称

信息内容

3.相关日志文件

/var/log/messages

/var/log/maillog  #邮件系统信息

/var/log/dmesg   #内核引导信息

/var/log/secure   #记录登入系统存取数据的文件，例如 pop3, ssh, telnet, ftp 等都会被记录，我们可以利用此文件找出不安全的登陆IP

/var/log/wtmp     #记录登入者的讯息数据，由于本文件已经被编码过(为二进制文件)，所以必须使用 last指令来取出文件的内容，#用/usr/bin/lastt读取/var/log/wtmp

/var/log/lastlog   #记录每个使用者最近签入系统的时间， 因此当使用者签入时， 就会显示其上次签入的时间， #用 /usr/bin/lastlog指令读取

/etc/logrotate.conf