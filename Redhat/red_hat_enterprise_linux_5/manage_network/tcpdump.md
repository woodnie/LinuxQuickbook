### tcpdump {#tcpdump}

tcpdump

1.tcpdump 的选项介绍：

-a 　 将网络地址和广播地址转变成名字;

-d 　 将匹配信息包的代码以人们能够理解的汇编格式给出;

-dd 　将匹配信息包的代码以c语言程序段的格式给出;

-ddd   将匹配信息包的代码以十进制的形式给出;

-e 　   在输出行打印出数据链路层的头部信息;

-f 　    将外部的Internet地址以数字的形式打印出来;

-l 　    使标准输出变为缓冲行形式;

-n 　   不把网络地址转换成名字;

-t 　    在输出的每一行不打印时间戳；

-v 　   输出一个稍微详细的信息，例如在ip包中可以包括ttl和服务类型的信息;

-vv 　 输出详细的报文信息;

-c 　 在收到指定的包的数目后，tcpdump就会停止;

-F 　 从指定的文件中读取表达式,忽略其它的表达式;

-i 　   指定监听的网络接口;

-r 　   从指定的文件中读取包(这些包一般通过-w选项产生);

-w 　 直接将包写入文件中，并不分析和打印出来;

-T 　 将监听到的包直接解释为指定的类型的报文，常见的类型有

2\. tcpdump的表达式介绍

表达式是一个正则表达式，tcpdump利用它作为过滤报文的条件，如果一个报文满足表达式的条件，则这个报文将会被捕获。如果没有给出任何条件，则网络上所有的信息包将会被截获。在表达式中一般如下几种类型的关键字。

第一种是关于类型的关键字，主要包括host，net，port, 例如 host 210.27.48.2，指明 210.27.48.2是一台主机，net 202.0.0.0 指明 202.0.0.0是一个网络地址，port 23 指明端口号是23。如果没有指定类型，缺省的类型是host.

第二种是确定传输方向的关键字，主要包括src , dst ,dst or src, dst and src ,这些关键字指明了传输的方向。举例说明，src 210.27.48.2 ,指明ip包中源地址是210.27.48.2 , dst net 202.0.0.0 指明目的网络地址是202.0.0.0 。如果没有指明方向关键字，则缺省是src or dst关键字。

第三种是协议的关键字，主要包括fddi,ip,arp,rarp,tcp,udp等类型。Fddi指明是在FDDI(分布式光纤数据接口网络)上的特定的网络协议，实际上它是&quot;ether&quot;的别名，fddi和ether具有类似的源地址和目的地址，所以可以将fddi协议包当作ether的包进行处理和分析。其他的几个关键字就是指明了监听的包的协议内容。如果没有指定任何协议，则tcpdump将会监听所有协议的信息包。

除了这三种类型的关键字之外，其他重要的关键字如下：gateway, broadcast,less,greater,还有三种逻辑运算，取非运算是 not ! , 与运算是and,&amp;&amp;;或运算 是or ,││；这些关键字可以组合起来构成强大的组合条件来满足人们的需要，下面举几个例子来说明。

A想要截获所有210.27.48.1 的主机收到的和发出的所有的数据包：

#tcpdump host 210.27.48.1

B想要截获主机210.27.48.1 和主机210.27.48.2 或210.27.48.3的通信，使用命令：（在命令行中适用 括号时，一定要

#tcpdump host 210.27.48.1 and \ (210.27.48.2 or 210.27.48.3 \)

C如果想要获取主机210.27.48.1除了和主机210.27.48.2之外所有主机通信的ip包，使用命令：

#tcpdump ip host 210.27.48.1 and ! 210.27.48.2

D如果想要获取主机210.27.48.1接收或发出的telnet包，使用如下命令：

#tcpdump tcp port 23 host 210.27.48.1

3.tcpdump的输出结果介绍

下面我们介绍几种典型的tcpdump命令的输出信息

A,数据链路层头信息

使用命令

#tcpdump --e host ice

ice 是一台装有linux的主机，她的MAC地址是0：90：27：58：AF：1A

H219是一台装有SOLARIC的SUN工作站，它的MAC地址是8：0：20：79：5B：46；上一条命令的输出结果如下所示：

21:50:12.847509 eth0 ＜ 8:0:20:79:5b:46 0:90:27:58:af:1a ip 60: h219.33357 ＞ ice.telne

t 0:0(0) ack 22535 win 8760 (DF)

分析：21：50：12是显示的时间， 847509是ID号，eth0 ＜表示从网络接口eth0 接受该数据包，eth0 ＞表示从网络接口设备发送数据包, 8:0:20:79:5b:46是主机H219的MAC地址,它表明是从源地址H219发来的数据包. 0:90:27:58:af:1a是主机ICE的MAC地址,表示该数据包的目的地址是ICE . ip 是表明该数据包是IP数据包,60 是数据包的长度, h219.33357 ＞ ice.telnet 表明该数据包是从主机H219的33357端口发往主机ICE的TELNET(23)端口. ack 22535 表明对序列号是222535的包进行响应. win 8760表明发送窗口的大小是8760.

B,ARP包的TCPDUMP输出信息

使用命令

#tcpdump arp

得到的输出结果是：

22:32:42.802509 eth0 ＞ arp who-has route tell ice (0:90:27:58:af:1a)

22:32:42.802902 eth0 ＜ arp reply route is-at 0:90:27:12:10:66 (0:90:27:58:af:1a)

分析: 22:32:42是时间戳, 802509是ID号, eth0 ＞表明从主机发出该数据包, arp表明是ARP请求包, who-has route tell ice表明是主机ICE请求主机ROUTE的MAC地址。 0:90:27:58:af:1a是主机ICE的MAC地址。

C,TCP包的输出信息

用TCPDUMP捕获的TCP包的一般输出信息是：

src ＞ dst: flags data-seqno ack window urgent options

src ＞ dst:表明从源地址到目的地址, flags是TCP包中的标志信息,S 是SYN标志, F (FIN), P (PUSH) , R (RST) &quot;.&quot; (没有标记); data-seqno是数据包中的数据的顺序号, ack是下次期望的顺序号, window是接收缓存的窗口大小, urgent表明数据包中是否有紧急指针. Options是选项.

D,UDP包的输出信息

用TCPDUMP捕获的UDP包的一般输出信息是：

route.port1 ＞ ice.port2: udp lenth

UDP十分简单，上面的输出行表明从主机ROUTE的port1端口发出的一个UDP数据包到主机ICE的port2端口，类型是UDP， 包的长度是lenth

4.利用网络数据采集分析工具TcpDump分析网络安全

作为IP网络的系统管理员，经常会遇到一些网络连接方面的故障，在排查这些接故障时，除了凭借经验外，使用包分析软件往往会起到事半功倍的效果。

(a).网络的数据过滤

不带任何参数的TcpDump将搜索系统中所有的网络接口，并显示它截获的所有数据，这些数据对我们不一定全都需要，而且数据太多不利于分析。所以，我们应当先想好需要哪些数据，TcpDump提供以下参数供我们选择数据：

-b在数据-链路层上选择协议，包括ip、arp、rarp、ipx都是这一层的。例如：

server#tcpdump -b arp

将只显示网络中的arp即地址转换协议信息。

-i选择过滤的网络接口，如果是作为路由器至少有两个网络接口，通过这个选项，就可以只过滤指定的接口上通过的数据。例如：

server#tcpdump -i eth0

只显示通过eth0接口上的所有报头。src、dst、port、host、net、ether、gateway这几个选项又分别包含src、dst、 port、host、net、ehost等附加选项。他们用来分辨数据包的来源和去向，src host 192.168.0.1指定源主机IP地址是192.168.0.1，dst net 192.168.0.0/24指定目标是网络192.168.0.0。以此类推，host是与其指定主机相关无论它是源还是目的，net是与其指定网络相关的，ether后面跟的不是IP地址而是物理地址，而gateway则用于网关主机。可能有点复杂，看下面例子就知道了：

server#tcpdump src host 192.168.0.1 and dst net 192.168.0.0/24

过滤的是源主机为192.168.0.1与目的网络为192.168.0.0的报头。

server#tcpdump ether src 00:50:04:BA:9B and dst......

过滤源主机物理地址为XXX的报头(为什么ether src后面没有host或者net？物理地址当然不可能有网络喽)。

server#Tcpdump src host 192.168.0.1 and dst port not telnet

过滤源主机192.168.0.1和目的端口不是telnet的报头。

ip icmp arp rarp和tcp、udp、icmp这些选项等都要放到第一个参数的位置，用来过滤数据报的类型。例如：

server#tcpdump ip src......

只过滤数据-链路层上的IP报头。

server#tcpdump udp and src host 192.168.0.1

只过滤源主机192.168.0.1的所有udp报头。

(b)网络的数据显示/输入输出

TcpDump提供了足够的参数来让我们选择如何处理得到的数据，如下所示：

-l可以将数据重定向。

如tcpdump -l＞tcpcap.txt将得到的数据存入tcpcap.txt文件中。

-n不进行IP地址到主机名的转换。

如果不使用这一项，当系统中存在某一主机的主机名时，TcpDump会把IP地址转换为主机名显示，就像这样：eth0＜ntc9.1165＞router.domain.net.telnet，使用-n后变成了：eth0＜192.168.0.9.1165＞192.168.0.1.telnet。

-nn不进行端口名称的转换。

上面这条信息使用-nn后就变成了：eth0＜ntc9.1165＞router.domain.net.23。

-N不打印出默认的域名。

还是这条信息-N后就是：eth0＜ntc9.1165＞router.telnet。

-O不进行匹配代码的优化。

-t不打印UNIX时间戳，也就是不显示时间。

-tt打印原始的、未格式化过的时间。

-v详细的输出，也就比普通的多了个TTL和服务类型。

5.网络数据采集分析工具TcpDump分析详细例子

(a)网络邮件服务器(mail)在排障

我们先来看看故障现象，在一局域网中新安装了后台为qmail的邮件服务器server，邮件服务器收发邮件等基本功能正常，但在使用中发现一个普遍的怪现象：pc机器上发邮件时连接邮件服务器后要等待很久的时间才能开始实际的发送工作。我们来看，从检测来看，网络连接没有问题，邮件服务器 server和下面的pc性能都没有问题，问题可能出在哪里呢？为了进行准确的定位，我们在pc机client上发送邮件，同时在邮件服务器server 上使用tcpdump对这个client的数据包进行捕获分析，如下：

server#tcpdump host client

tcpdump: listening on hme0

23:41:30.040578 client.1065 ＞ server.smtp: S 1087965815:1087965815(0) win 64240 (DF)

23:41:30.040613 server.smtp ＞ client.1065: S 99285900:99285900(0) ack 1087965816 win 10136 (DF)

23:41:30.040960 client.1065 ＞ server.smtp: . ack 1 win 64240 (DF)

顺利的完成，到目前为止正常，我们再往下看：

23:41:30.048862 server.33152 ＞ client.113: S 99370916:99370916(0) win 8760 (DF)

23:41:33.411006 server.33152 ＞ client.113: S 99370916:99370916(0) win 8760 (DF)

23:41:40.161052 server.33152 ＞ client.113: S 99370916:99370916(0) win 8760 (DF)

23:41:56.061130 server.33152 ＞ client.113: R 99370917:99370917(0) win 8760 (DF)

23:41:56.070108 server.smtp ＞ client.1065: P 1:109(108) ack 1 win 10136 (DF)

看出问题了，问题在：我们看到server端试图连接client的113identd端口，要求认证，然而没有收到client端的回应，server端重复尝试了3次，费时26秒后，才放弃认证请求，主动发送了reset标志的数据包，开始push后面的数据，而正是在这个过程中所花费的26秒时间，造成了发送邮件时漫长的等待情况。问题找到了，就可以修改了，我们通过修改服务器端的qmail配置，使它不再进行113端口的认证，再次抓包，看到邮件server不再进行113端口的认证尝试，而是在三次检测后直接push数据，问题得到完美的解决。

(b)网络安全中的ARP协议的故障

先看故障现象，局域网中的一台采用solaris操作系统的服务器A-SERVER网络连接不正常，从任意主机上都无法ping通该服务器。排查：首先检查系统，系统本身工作正常，无特殊进程运行，cpu，内存利用率正常，无挂接任何形式的防火墙，网线正常。此时我们借助tcpdump来进行故障定位，首先我们将从B-CLIENT主机上执行ping命令，发送icmp数据包给A-SERVER，如下：

[root@redhat log]# ping A-SERVER

PING A-SERVER from B-CLIENT : 56(84) bytes of data.

此时在A-SERVER启动tcpdump，对来自主机B-CLIENT的数据包进行捕获。

A-SERVER# tcpdump host B-CLIENT

tcpdump: listening on hme0

16:32:32.611251 arp who-has A-SERVER tell B-CLIENT

16:32:33.611425 arp who-has A-SERVER tell B-CLIENT

16:32:34.611623 arp who-has A-SERVER tell B-CLIENT

我们看到，没有收到预料中的ICMP报文，反而捕获到了B-CLIENT发送的arp广播包，由于主机B-CLIENT无法利用arp得到服务器 A-SERVER的地址，因此反复询问A-SERVER的MAC地址，由此看来，高层的出问题的可能性不大，很可能在链路层有些问题，先来查查主机A- SERVER的arp表：

A-SERVER# arp -a

Net to Media Table

Device IP Address Mask Flags Phys Addr

------ -------------------- --------------- ----- ---------------

hme0 netgate 255.255.255.255 00:90:6d:f2:24:00

hme0 A-SERVER 255.255.255.255 S 00:03:ba:08:b2:83

hme0 BASE-ADDRESS.MCAST.NET 240.0.0.0 SM 01:00:5e:00:00:00

请注意A-SERVER的Flags位置，我们看到了只有S标志。我们知道，solaris在arp实现中，arp的flags需要设置P标志，才能响应ARP requests。

手工增加p位

A-SERVER# arp -s A-SERVER 00:03:ba:08:b2:83 pub

此时再调用arp -a看看

A-SERVER# arp -a

Net to Media Table

Device IP Address Mask Flags Phys Addr

------ -------------------- --------------- ----- ---------------

hme0 netgate 255.255.255.255 00:90:6d:f2:24:00

hme0 A-SERVER 255.255.255.255 SP 00:03:ba:08:b2:83

hme0 BASE-ADDRESS.MCAST.NET 240.0.0.0 SM 01:00:5e:00:00:00