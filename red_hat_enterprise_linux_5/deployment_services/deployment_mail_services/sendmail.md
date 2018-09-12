#### sendmail {#sendmail}

mail

名词：

MUA（mail user agent）邮件用户代理

MSP （Mail Submission program）邮件提交程序

MTA (Mail Transport Agent) 邮件传输代理 sendmail postfix

MDA (Mail Delivery Agent) 邮件递交代理

MRA （Mail Retrieve Agent）邮件检索代理

sendmail电子邮件

# yum install sendmail sendmail-cf m4

chkconfig sendmail on

ntsysv

步骤1：配置MTA来收取邮件

1.# cp /etc/mail/sendmail.mc  /etc/mail/sendmail.mc.bak  #将您的sendmail 配置文件做一个备份

2.# vi /etc/mail/sendmail.mc

DAEMON_OPTIONS(`Port=smtp,Addr=127.0.0.1, Name=MTA)  #把127.0.0.1改为0.0.0.0 sendmail缺省的配置只接受从回环接口上的连接

3.#m4 /etc/mail/sendmail.mc &gt; /etc/mail/sendmail.cf     #编译mc ---&gt; cf

4.# /etc/init.d/sendmail restart

步骤4：添加新的入站别名

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

echo &quot;this is a amil test&quot; | mail -s &quot;hello&quot; student

[root@node6 ~]# mutt -f /var/mail/user

[root@node6 ~]# mail -v user@doamin.tld

您是否得到了期望的结果？是否所有的位于wizards的收件人都受到了邮件？如果没有，su - 到不是root的用户再试一次。

步骤5：实现域与域之间的互发邮件

1.定义了/etc/mail/access（默认只允许本机转发）

[root@test root]#vi /etc/mail/access

#启用的启用相关域和主机

domain  RELAY

host      RELAY

#储存后离开

[root@test root]#makemap hash /etc/mail/access.db &lt;/etc/mail/access

[root@test root]# /etc/init.d/sendmail restart

2.启用SMTP 邮件认证机制 ( SMTP Authorization ) ，主要透过 cyrus sasl 这个套件来达成邮件的认证动作 ，只要通过认证，并保证/etc/mail/access中没有策略阻止，就可以达到邮件收发的目的.Cyrus SASL是Cyrus Simple Authentication and Security Layer的简写，它最大的功能是为应用程序提供了认证函数库。应用程序可以通过函数库所提供的功能定义认证方式，并让SASL通过与邮件服务器主机的沟通从而提供认证的功能。

A 查看cyrus-sasl包是否安装

B  编译sendmail.mc文件，把52行和53行的注释去掉，然后用m4编译生成sendmail.cf文件

重新启动服务，telnet 25端口，如果出现LOGIN PLAIN字样，说明编译成功

C 默认情况下，Cyrus-SASL V2版使用saslauthd这个守护进程进行密码认证,要保证saslauthd这个服务是启动状态

这样，你们就可以互发邮件测试了！

sendmail 操作：

#sendmail -q

#mailq

#mailq -Ac

sendmail troubleshooting

sendmail可执行文件位于/usr/sbin/sendmail。为了确定sendmail是否正确标识您的主机名称，通过命令行开关开启其调试模式并且设定为0：

# sendmail -d0 如果sendmail返回您的主机名称为localhost,您可能错误配置主机名。检查您的/etc/hosts,/etc/sysconfig/netwoek中的HOSTNAME的定义。

您的消息是不是在/var/log/maillog中正确的记录呢

[root@node6 ~]# iptables -t filter -I INPUT 1 -p udp --dport 25 -m state --state NEW -j ACCEPT

[root@node6 ~]# iptables -t filter -I INPUT 1 -p tcp --dport 25 -m state --state NEW -j ACCEPT      

++++++++++++++++++

trouble shooting

# service sendmail restart

Shutting down sendmail: [FAILED]

Shutting down sm-client: [FAILED]

Starting sendmail: 451 4.0.0 /etc/mail/sendmail.cf: line 91: fileclass: cannot open /etc/mail/local-host-names: World writable directory

451 4.0.0 /etc/mail/sendmail.cf: line 570: fileclass: cannot open /etc/mail/trusted-users: World writable directory

[FAILED]

Starting sm-client: /etc/mail/submit.cf: line 527: fileclass: cannot open /etc/mail/trusted-users: World writable directory

[FAILED]

chmod go-w / /etc /etc/mail /usr /var /var/spool /var/spool/mqueue

chown root / /etc /etc/mail /usr /var /var/spool /var/spool/mqueue

然后可以用如下命令验证

sendmail -v -bi

##########################

发件人伪装：

1,开启sendmail.mc行

FEATURE(genericstable)dnl

FEATURE(`always_add_domain)dnl

GENERICS_DOMAIN_FILE(`/etc/mail/local-host-names)dnl

2,创建genericstable文件

vim /etc/mail/genericstable

apache@MagentoWebServer shop@adidas.cn

paul@example.com paul@otherexample.com

david@example.com david.lastname@example.com

发件人paul@example.com伪装成这个发件人 paul@otherexample.com

发件人david@example.com伪装成这个发件人 david.lastname@example.com

3,local-host-names中要有所指定的域名

adidas.cn

MagentoWebServer

############################

邮件代发：

sendmail_path = /usr/sbin/sendmail -f shop@adidas.cn -t -i

mail queue

邮件队列是存储 sendmail 命令传送的邮件消息数据和控制文件的目录。缺省情况下，邮件队列是 /var/spool/mqueue。

邮件消息可能由于很多原因而排入队列。

例如：

   sendmail 命令可以配置成按一定的时间间隔处理队列，而不是立即处理。如果这样，邮件消息必须临时存储。

   如果一个远程主机不响应一个邮件连接的请求，邮件系统会将这些消息排入队列，稍后再作尝试。

打印邮件队列队列内容可以使用 mailq 命令打印（或通过指定 sendmail 命令的 -bp 标志）。

这些命令产生队列标识、消息大小、消息进入队列的日期以及发送方与收件人的列表。

邮件队列文件队列中的每条消息都与一定数量的文件相关联。这些文件按以下约定命名：

TypefID其中 ID 是一个唯一的消息队列标识，而 Type 是以下表示文件类型的字母中的一个：

d        包含消息正文但无标题信息的数据文件。

q        队列控制文件。该文件包含处理作业所需要的信息。

t        临时文件。该文件是 q 文件重建时的一个映象。它快速重命名为 q 文件。

x        在会话过程中存在并显示该次会话中发生的任何事件的记录文件。

例如，如果一条消息的队列标识为 AA00269，当 sendmail 命令尝试传送消息时，在邮件队列目录中创建和删除以下文件：

dfAA00269        数据文件

qfAA00269        控制文件

tfAA00269        临时文件

xfAA00269        记录文件

q 控制文件

q 控制文件包括一系列行，每一行都以一个代码字母开始：

B        指定 body type。该行其余部分是定义 body type 的文本字符串。如果缺失该项字段，则缺省情况下 body type 是 7 位的，而且不会尝试特殊的处理。合法值是 7BIT 和 8BITMIME。

C        包括控制用户。对于以文件或程序作收件人的地址，sendmail 作为该文件或程序的所有者来执行传送。控制用户被设置为文件或程序的所有者。由 .forward 或 :include: 文件读取的收件人地址也将使控制用户被设置为文件所有者。当 sendmail 传送邮件到这些收件人时，sendmail 作为控制用户传送，然后转换回 root 用户。

F        包括信包标志。标志是以下的任意组合：w（设置 EF_WARNING 标志）、r（设置 EF_RESPONSE 标志）、8（设置 EF_HAS8BIT 标志）和 b（设置 EF_DELETE_BCC 标志）。其它字母则被忽略而无提示。

H        包括一个标题定义。此类行的数量任意。H 行出现的顺序确定了它们在最终消息里的出现顺序。这些行使用的语法与 /etc/mail/sendmail.cf 配置文件中的标题定义相同。（对于早于 AIX 5.1 的版本，该文件是 /etc/sendmail.cf。）

I        为 df 文件指定内节点和设备信息；这可以在磁盘崩溃后用来恢复邮件队列。

K        指定上一次传输尝试的时间（以秒为单位）。

M        当一条消息由于在传送尝试中出现了错误而放入队列时，错误的性质就存储在 M 行。

N        指定传送尝试的总数。

O        指定 ESMTP 的消息传输系统（MTS）的原始值。它只用于传送状态通知。

P        包括当前消息的优先级。优先级用来对队列排序。数字越大表示优先级越低。当消息位于队列中时优先级增加。初始优先级取决于消息的类和消息的大小。

Q        包含初始收件人，由 ESMTP 事务中的 ORCPT= 字段指定。仅用于传送状态通知。只应用于紧接着的 R 行。

R        包含收件人地址。每个收件人占一行。

S        包含发送方地址。此类行只有一行。

T        包含消息创建时间，用来计算何时消息超时。

V        指定队列文件格式版本号（该队列文件格式用来允许新的 sendmail 二进制文件读取旧版本创建的队列文件）。缺省时指版本 0。如果存在，必须是文件的第一行。

Z        指定原始信包标识（从 ESMTP 事务中）。只用于传送状态通知。

$        包含宏定义。某些宏（$r 和 $s）的值会传递到队列运行阶段。

传送到 amy@zeus 的消息的 q 文件类似于：

P217031T566755281MDeferred: Connection timed out during user open with zeusRamy@zeusH?P?return-path: &lt;geo&gt;Hreceived: by george (0.13 (NL support)/0.01)id AA00269; Thu, 17 Dec 87 10:01:21 CSTH?D?date: Thu, 17 Dec 87 10:01:21 CSTH?F?From: geoHmessage-id: &lt;8712171601.AA00269@george&gt;HTo: amy@zeusHsubject: test其中：

P217031        消息的优先级

T566755281        提交时间（秒）

MDeferred: Connection timed out during user open with zeus        状态消息

Sgeo        发送方标识

Ramy@zeus        收件人标识

Hlines        消息的报头信息

在 sendmail 中指定时间值

要设置消息超时和队列处理间隔，必须用特定的时间值格式。时间值的格式是：

-qNumberUnit其中 Number 是一个整数值，Unit 是单位字母。Unit 可以是以下值中的一个：

s        秒

m        分

h        小时

d        天

w        周

如果没有指定 Unit，sendmail 守护程序使用分（m）作为缺省值。下面三个示例说明时间值的规范：

/usr/sbin/sendmail -q15d该命令使得 sendmail 守护程序每 15 天处理一次队列。

/usr/sbin/sendmail -q15h该命令使得 sendmail 守护程序每 15 小时处理一次队列。

/usr/sbin/sendmail -q15该命令使得 sendmail 守护程序每 15 分钟处理一次队列。

强制邮件队列

在某些情况下，您可能发现队列由于某种原因阻塞。您可以使用 -q 标志（没有值）强制一个队列运行。您也可以用 -v 标志（详细）来观察发生了什么：

/usr/sbin/sendmail -q -v使用一个队列修饰符，您也可以将作业限制在具有特定队列标识符、发送方或收件人的范围中。例如，-qRsally 将队列运行限制为收件人地址之一中有字符串 sally 的作业。同样，-qS 字符串会将运行限制为特定的发送方，而 -qI 字符串将它限制为特定的队列标识。

设置队列处理时间间隔守护程序启动时 -q 标志的值确定 sendmail 守护程序处理邮件队列的时间间隔。

sendmail 守护程序通常由 /etc/rc.tcpip 文件在系统启动时启动。/etc/rc.tcpip 文件包含一个称为队列处理间隔（QPI）的变量，该变量在该文件启动 sendmail 守护程序时用来指定 -q 标志的值。缺省情况下，qpi 的值是 30 分钟。要指定不同的队列处理间隔：

   用您喜欢的编辑器编辑 /etc/rc.tcpip 文件。

   查找给 qpi 变量指定值的行，例如：

   qpi=30m

   将指定给变量 qpi 的值更改为希望的时间值。

这些变化会在下一次系统重新启动时生效。如果您想让这些变化立刻生效，请停止并重新启动 sendmail 守护程序，指定新的 -q 标志值。更多相关信息，请参阅停止 sendmail 守护程序和启动 sendmail 守护程序。

移动邮件队列

当一个主机长期关闭时，路由到（或通过）该主机的很多消息可能存储在邮件队列中。结果 sendmail 命令要花费很长时间对队列排序，这严重降低了系统性能。如果您移动队列到一个临时空间并创建一个新的队列，旧队列可以稍后在该主机恢复服务后运行。要移动队列到一个临时空间并创建一个新的队列，请：

   按停止 sendmail 守护程序中的指示信息停止 sendmail 守护程序。

   输入以下内容移动整个队列目录：

   cd /var/spool    mv mqueue omqueue

   按启动 sendmail 守护程序中的指示信息重新启动 sendmail 守护程序。

   输入以下内容处理旧邮件队列：

   /usr/sbin/sendmail -oQ/var/spool/omqueue -q

   -oQ 标志指定一个备用队列目录。 -q 标志指定运行队列中的每一项作业。要获取操作过程的报告，请使用 -v 标志。

   注:

   此操作可能要花些时间。

   当队列为空时，输入以下内容除去日志文件和临时目录：

   rm /var/spool/omqueue/*    rmdir /var/spool/omqueue

启动 sendmail 守护程序要启动 sendmail 守护程序，请输入以下命令中的一个：

startsrc -s sendmail -a &quot;-bd -q15&quot;

/usr/lib/sendmail -bd -q15如果 sendmail 守护程序在输入这些命令中的一个时已经激活，请参阅屏幕上的以下消息：

sendmail 子系统已经激活。不支持多实例。如果 sendmail 守护程序没有被激活，您将会看到一条消息表示 0sendmail 守护程序已经启动。

停止 sendmail 守护程序要停止 sendmail 守护程序，请运行 stopsrc -s sendmail 命令。

如果 sendmail 守护程序没有随 startsrc 命令启动，请：

   查找 sendmail 进程标识。

   输入 kill sendmail_pid 命令。（其中 sendmail_pid 是 sendmail 过程的处理标识）。