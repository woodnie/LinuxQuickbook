### ntp {#ntp}

ntp

网络时间服务的概述(ntpd服务)

NTP（Network Time Protocol，网络时间协议）是用来使计算机系统时间与NTP服务器或时钟源（如石英钟、GPS等）同步化的一种协议，它可以提供高精度的网络时间校正。通常与标准时间的偏差在局域网上可小于1毫秒，在广域网上可小于几十毫秒。

NTP只是一个协议，执行NTP需要专门的服务器和客户端软件。NTP服务器（也称网络时间服务器）可提供网络校时服务，当客户端发出请求，它就将自己的时间或标准时间源的时间发送给客户端；而NTP客户端是用来将客户端主机的时钟精确地同步到NTP服务器时钟。

NTP要提供准确时间，首先要有准确而可靠的时间源，这一时间应该是全球标准时间（UTC）。NTP可以通过原子钟、天文台、卫星和Internet等多种方式来获得UTC。在获得了准确的时钟源后，要将时间按NTP服务器的层次等级传播。

一 网络时钟服务的工作原理

当NTP服务器获得了标准时间源的时间后，NTP客户端又是如何与NTP服务器进行时间同步的呢？通常情况下，时间同步是按以下步骤进行的。

① NTP客户端向NTP服务器发出一个时间请求包（UDP包），其中包含了该包离开客户端时的时间戳。

② 当服务器接收到该包时，填入包到达时的时间戳、包离开时的时间戳等信息，然后立即把包返回给客户端。

③ 客户端在接收到响应包时再填入包返回时的时间戳，然后利用这些时间参数计算出两个关键参数，即包往返的延迟、客户端与服务器之间的时钟偏移。

④ 客户端使用时钟偏移来调整本地时钟，以使其时间与服务器时间一致。

二、网络时间服务的配置

1 安装

[root@server1 ~]# rpm -qa |grep ntp

chkfontpath-1.10.1-1.1

ntp-4.2.2p1-7.el5（这是服务器的包）

[root@server1 ~]# rpm -ql ntp-4.2.2p1-7.el5

/etc/ntp.conf（这是主要的配置文件）

2 .1配置/etc/ntp.conf

[root@server1 ~]# vi /etc/ntp.conf

# /etc/ntp.conf

# Author: Steve Bonneville &lt;sbonnevi@redhat.com&gt;

# $Id: ntp.conf-gls,v 1.1.2.1 2007/07/24 15:43:17 jhoffman  Exp $

# NTP configuration for server1.example.com

# Permit anyone to synchronize, but dont peer with them or reconfigure

restrict default nomodify

# Allow all access from localhost

restrict 127.0.0.1

# Use inaccurate motherboard clock to synchronize

server 127.127.1.0

server 192.168.0.254（实验环境中，192.168.0.254是我们的上层服务器）

peer 192.168.0.X   （此处是指定了一个同级的时钟服务器）

fudge 127.127.1.0 stratum 10

# Centennial:  synchronize with 192.168.22.0/24

# restrict 192.168.22.0 mask 255.255.255.0 nomodify notrap

# server albus.training.redhat.com

driftfile /var/lib/ntp/drift

broadcastdelay 0.008

2 .2配置/etc/ntp.conf

假定时钟服务器IP地址为：192.168.0.1

服务器端配置：

1:置/etc/ntp.conf文件内容为：

server 127.127.1.0 minpoll 4

fudge 127.127.1.0 stratum 1

restrict 127.0.0.1

restrict 192.168.0.0 mask 255.255.255.0 nomodify notrap

driftfile /var/lib/ntp/drift

2: /etc/ntp/ntpservers应置空

3: /etc/ntp/step-tickers应配置为 127.127.1.0

上诉修改完成后，以root用户身份重启ntpd服务:service ntpd restart即可

客户端配置：

1:置/etc/ntp.conf文件内容为：

server 192.168.0.1

fudge 127.127.1.0 stratum 2

restrict 127.0.0.1

driftfile /var/lib/ntp/drift

restrict 192.168.0.1 mask 255.255.255.255

2\. /etc/ntp/ntpservers 文件内容置空

3\. /etc/ntp/step-tickers文件内容置为时钟服务器IP地址 192.168.0.1

上诉修改完成后，以root用户身份重启ntpd服务:service ntpd restart即可

用户可用以下两个常用命令查看ntpd服务状态：

1 ntpq -p

2 ntpstat

3 /etc/ntp.conf相关解释

有关权限设置的语句格式为：

restrict　IP地址或域名　［mask 子网掩码］　［选项］

常用选项如下。

ignore：表示禁止所有的NTP请求包进入。

nomodify：表示禁止其他计算机更改本机NTP服务的设置，但可以通过本NTP服务器进行网络校时。

notrust：表示禁止所有未通过认证的NTP包进入。

noquery：表示禁止其他计算机查询本机NTP服务的状态。

设置使用上层NTP服务器来校正本机时钟的语句格式为：

server  IP地址或域名 ［prefer］

4启动和停止网络时间服务

[root@server1 ~]# service ntpd start

[root@server1 ~]# service ntpd stop

5 测试网络时间服务

[root@server1 ~]# ntpq

ntpq&gt; peer

    remote           refid      st t when poll reach   delay   offset  jitter

==============================================================================

*LOCAL(0)        .LOCL.          10 l   48   64  377    0.000    0.000   0.001

server1.example LOCAL(0)        11 u  370 1024  377    2.602  2372135 1515905

192.168.0.174   .INIT.          16 u    - 1024    0    0.000     ntpq&gt;

上面给出了当前所有可用来进行网络校时的上层NTP服务器列表。其中，部分显示字段的含义如下。

 remote：表示上层NTP服务器的IP地址或域名，前面带有符号&quot;*&quot;，则表示该上层NTP服务器是本机的NTP服务器当前同步用的时间源；

               &quot;+&quot;表示该上层NTP服务器是本机的NTP服务器当前同步用的候选时间源。

 refid：表示该上层NTP服务器同步用的时间源，如GPS（全球卫星定位服务）、ACTS（计算机自动化报时服务）等。

 st：表示该上层NTP服务器的层号，若为16，则表示本机的NTP服务器无法与该上层NTP服务器进行同步。

 delay：表示与该上层NTP服务器之间联系所花费的时间，单位为毫秒。

 offset：表示本机的NTP服务器与该上层NTP服务器之间的时间偏移，单位为毫秒。

6 检查本地NTP服务器是否通过上层NTP服务器进行校时

[root@server1 ~]# ntptrace 192.168.0.254

server1.example.com: stratum 11, offset 0.000000, synch distance 0.011350

它是说我们的主机为第二层时钟服务器，与本机的时间误差（offset 0.000000），同步更新需要的时间（synch distance ）

至此，服务端就配置完成了，下面讲一下，客户端向服务端同步的方法

7 客户端时间同步

做为时间服务器的客户端，有两种方法可以向上级时钟服务器同步时间，最简单的方式就是让客户机上也启用ntpd服务，直接指定你要同步的上级的时钟服务器做为时钟源就可以了。

运行system-config-date这个图形化命令最方便！！！

另一种方法，只需要做以下的操作就可以了

[root@stationX ~]# ntpdate 192.168.0.X（这是我之前配置的时钟服务器）

9 Jul 12:32:48 ntpdate[8150]: step time server 192.168.0.X offset -151.316557 sec

我们还要让本机系统和BIOS同步一下，运行下面命令

[root@stationX ~]# hwclock  -w

大家想一下，如果你想周期性的同步时钟，是不是要做一个计划任务呀！！！！

公网NTPserver：

clock.redhat.com