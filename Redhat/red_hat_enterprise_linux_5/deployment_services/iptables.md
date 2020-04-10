### iptables {#iptables}

在内核中过滤：无守护进程

在 OSI 参考模型（OSI Reference Model）的第2、3、4层插入策略

只查看数据包标题

由内核中的netfilter模块和iptables用户空间软件

路由前：应用nat表

转发：应用filter表

输入：应用filter表

输出：应用net和filter表

路由后：应用net表

IP规则的保存与恢复

iptables-save &gt;/etc/sysconfig/iptables

service iptables save

把规则保存到文件 /etc/sysconfig/iptables

当计算机启动时，rc.d下的脚本将用命令iptables-restore调用这个文件，从而就自动恢复了规则。

iptables 指令语法

iptables [-t table] command [match] [-j target/jump]

[-t table] 指定规则表

-t 参数用来，内建的规则表有三个，分别是：nat、mangle 和 filter，当未指定规则表时，则一律视为是 filter。个规则表的功能如下：

nat：此规则表拥有 PREROUTING 和 POSTROUTING 两个规则链，主要功能为进行一对一、一对多、多对多等网址转换工作（SNAT、DNAT），这个规则表除了作网址转换外，请不要做其它用途。

mangle：此规则表拥有 PREROUTING、FORWARD 和 POSTROUTING 三个规则链。除了进行网址转换工作会改写封包外，在某些特殊应用可能也必须去改写封包（TTL、TOS）或者是设定 MARK（将封包作记号，以进行后续的过滤），这时就必须将这些工作定义在 mangle 规则表中，由于使用率不高，我们不打算在这里讨论 mangle 的用法。

filter： 这个规则表是默认规则表，拥有 INPUT、FORWARD 和 OUTPUT 三个规则链，这个规则表顾名思义是用来进行封包过滤的处理动作（例如：DROP、 LOG、 ACCEPT 或 REJECT），我们会将基本规则都建立在此规则表中。

command 常用命令列表：

命令 -A, --append

范例 iptables -A INPUT ...

说明 新增规则到某个规则链中，该规则将会成为规则链中的最后一条规则。

命令 -D, --delete

范例 iptables -D INPUT --dport 80 -j DROP

iptables -D INPUT 1

说明 从某个规则链中删除一条规则，可以输入完整规则，或直接指定规则编号加以删除。

命令 -R, --replace

范例 iptables -R INPUT 1 -s 192.168.0.1 -j DROP

说明 取代现行规则，规则被取代后并不会改变顺序。

命令 -I, --insert

范例 iptables -I INPUT 1 --dport 80 -j ACCEPT

说明 插入一条规则，原本该位置上的规则将会往后移动一个顺位。

命令 -L, --list  (--line-numbers)

范例1 iptables -L INPUT

说明 列出某规则链中的所有规则。

范例2 iptables -t nat -L

说明 列出nat表所有链中的所有规则。

命令 -F, --flush

范例 iptables -F INPUT

说明 删除filter表中INPUT链的所有规则。

命令 -Z, --zero

范例 iptables -Z INPUT

说明 将封包计数器归零。封包计数器是用来计算同一封包出现次数，是过滤阻断式攻击不可或缺的工具。

命令 -N, --new-chain

范例 iptables -N allowed

说明 定义新的规则链。

命令 -X, --delete-chain

范例 iptables -X allowed

说明 删除某个规则链。

命令 -P, --policy

范例 iptables -P INPUT DROP

说明 定义过滤政策。 也就是未符合过滤条件之封包， 默认的处理方式。

命令 -E, --rename-chain

范例 iptables -E allowed disallowed

说明 修改某 自定义规则链的名称。

[match] 常用封包匹配参数

参数 -p, --protocol

范例 iptables -A INPUT -p tcp

说明 匹配通讯协议类型是否相符，可以使用 ! 运算符进行反向匹配，例如：

-p !tcp

意思是指除 tcp 以外的其它类型，如udp、icmp ...等。

如果要匹配所有类型，则可以使用 all 关键词，例如：

-p all

参数 -s, --src, --source

范例 iptables -A INPUT -s 192.168.1.1

说明 用来匹配封包的来源 IP，可以匹配单机或网络，匹配网络时请用数字来表示 子网掩码，例如：

-s 192.168.0.0/24

匹配 IP 时可以使用 ! 运算符进行反向匹配，例如：

-s !192.168.0.0/24。

参数 -d, --dst, --destination

范例 iptables -A INPUT -d 192.168.1.1

说明 用来匹配封包的目的地 IP，设定方式同上。

参数 -i, --in-interface

范例 iptables -A INPUT -i eth0

说明 用来匹配封包是从哪块网卡进入，可以使用通配字符 + 来做大范围匹配，例如：

-i eth+

表示所有的 ethernet 网卡

也可以使用 ! 运算符进行反向匹配，例如：

-i !eth0

参数 -o, --out-interface

范例 iptables -A FORWARD -o eth0

说明 用来匹配封包要从哪 块网卡送出，设定方式同上。

参数 --sport, --source-port

范例 iptables -A INPUT -p tcp --sport 22

说明 用来匹配封包的源端口，可以匹配单一端口，或是一个范围，例如：

--sport 22:80

表示从 22 到 80 端口之间都算是符合条件，如果要匹配不连续的多个端口，则必须使用 --multiport 参数，详见后文。匹配端口号时，可以使用 ! 运算符进行反向匹配。

参数 --dport, --destination-port

范例 iptables -A INPUT -p tcp --dport 22

说明 用来匹配封包的目的地端口号，设定方式同上

参数 --tcp-flags

范例 iptables -p tcp --tcp-flags SYN,FIN,ACK SYN

说明匹配 TCP 封包的状态标志，参数分为两个部分，第一个部分列举出想 匹配的标志，第二部分则列举前述标志中哪些有被设置，未被列举的标志必须是空的。TCP 状态标志包括：SYN（同步）、ACK（应答）、FIN（结束）、RST（重设）、URG（紧急） 、PSH（强迫推送） 等均可使用于参数中，除此之外还可以使用关键词 ALL 和 NONE 进行匹配。匹配标志时，可以使用 ! 运算符行反向匹配。

参数 --syn

范例 iptables -p tcp --syn

说明 用来表示TCP通信协议中，SYN位被打开，而ACK与FIN位关闭的分组，即TCP的初始连接，与 iptables -p tcp --tcp-flags SYN,FIN,ACK SYN 的作用完全相同，如果使用 !运算符，可用来 匹配非要求连接封包。

参数 -m multiport --source-port

范例 iptables -A INPUT -p tcp -m multiport --source-port 22,53,80,110

说明用来匹配不连续的多个源端口，一次最多可以匹配 15 个端口，可以使用 ! 运算符进行反向匹配。

参数 -m multiport --destination-port

范例 iptables -A INPUT -p tcp -m multiport --destination-port 22,53,80,110

说明用来 匹配不连续的多个目的地端口号，设定方式同上

参数 -m multiport --port

范例 iptables -A INPUT -p tcp -m multiport --port 22,53,80,110

说明 这个参数比较特殊，用来匹配源端口和目的端口号相同的封包，设定方式同上。注意：在本范例中，如果来源端口号为 80目的地端口号为 110，这种封包并不算符合条件。

参数 --icmp-type

范例 iptables -A INPUT -p icmp --icmp-type 8

说明用来匹配 ICMP 的类型编号，可以使用代码或数字编号来进行 匹配。请打 iptables -p icmp --help 来查看有哪些代码可用。

参数 -m limit --limit

范例 iptables -A INPUT -m limit --limit 3/hour

说明 用来匹配某段时间内封包的平均流量，上面的例子是用来 匹配：每小时平均流量是否超过一次 3 个封包。 除了每小时平均次外，也可以每秒钟、每分钟或每天平均一次，默认值为每小时平均一次，参数如后： /second、 /minute、/day。 除了进行封 包数量的匹配外，设定这个参数也会在条件达成时，暂停封包的匹配动作，以避免因骇客使用洪水攻击法，导致服务被阻断。

参数 --limit-burst

范例 iptables -A INPUT -m limit --limit-burst 5

说明 用来匹配瞬间大量封包的数量，上面的例子是用来匹配一次同时涌入的封包是否超过 5 个（这是默认值），超过此上限的封包将被直接丢弃。使用效果同上。

参数 -m mac --mac-source

范例 iptables -A INPUT -m mac --mac-source 00:00:00:00:00:01

说明 用来匹配封包来源网络接口的硬件地址，这个参数不能用在 OUTPUT 和 POSTROUTING 规则链上，这是因为封包要送到网 卡后，才能由网卡驱动程序透过 ARP 通讯协议查出目的地的 MAC 地址，所以 iptables 在进行封包匹配时，并不知道封包会送到哪个网络接口去。

参数 --mark

范例 iptables -t mangle -A INPUT -m mark --mark 1

说明 用来匹配封包是否被表示某个号码，当封包被 匹配成功时，我们可以透过 MARK 处理动作，将该封包标示一个号码，号码最大不可以超过 4294967296。

参数 -m owner --uid-owner

范例

iptables -A OUTPUT -m owner --uid-owner 500

说明 用来匹配来自本机的封包，是否为某特定使用者所产生的，这样可以避免服务器使用 root 或其它身分将敏感数据传送出，可以降低系统被骇的损失。可惜这个功能无法 匹配出来自其它主机的封包。

参数 -m owner --gid-owner

范例 iptables -A OUTPUT -m owner --gid-owner 0

说明 用来匹配来自本机的封包，是否为某特定使用者群组所产生的，使用时机同上。

参数 -m owner --pid-owner

范例

iptables -A OUTPUT -m owner --pid-owner 78

说明 用来匹配来自本机的封包，是否为某特定进程所产生的，使用时机同上。

参数 -m owner --sid-owner

范例

iptables -A OUTPUT -m owner --sid-owner 100

说明 用来匹配来自本机的封包，是否为某特定 连接（Session ID）的响应封包，使用时机同上。

参数 -m state --state

范例

iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

说明 使用一条规则来允许建立连接

范例

iptables -A INPUT -m state --state NEW -p tcp --dport 25 -j ACCEPT

说明 使用多条规则来跟踪，每个许可的服务都有一条规则

范例

iptables -A INPUT -m state --state NEW -j DROP

说明 最后，使用一条规则来阻止所有其它进入的连接

连接状态共有四种：INVALID、ESTABLISHED、NEW 和 RELATED。

INVALID 表示该封包的连接编号（Session ID）无法辨识或编号不正确。

ESTABLISHED 表示该封包属于某个已经建立的连接。

NEW 表示该封包想要起始一个连接（重设连接或将连接重导向）。

RELATED 表示该封包是属于某个已经建立的连接，所建立的新连接。例如：FTP-DATA 连接必定是源自某个 FTP 连接。

[-j target/jump] 常用的处理动作：

-j 参数用来指定要进行的处理动作，常用的处理动作包括：ACCEPT、REJECT、DROP、REDIRECT、MASQUERADE、LOG、DNAT、SNAT、MIRROR、QUEUE、RETURN、MARK，分别说明如下：

ACCEPT： 将封包放行，进行完此处理动作后，将不再匹配其它规则，直接跳往下一个规则链（natostrouting）。

REJECT： 拦阻该封包，并传送封包通知对方，可以传送的封包有几个选择：ICMP port-unreachable、ICMP echo-reply 或是tcp-reset（这个封包会要求对方关闭 连接），进行完此处理动作后，将不再匹配其它规则，直接中断过滤程序。 范例如下：

iptables -A FORWARD -p TCP --dport 22 -j REJECT --reject-with tcp-reset

DROP： 丢弃封包不予处理，进行完此处理动作后，将不再匹配其它规则，直接中断过滤程序。

REDIRECT： 将封包重新导向到另一个端口（PNAT），进行完此处理动作后，将会继续匹配其它规则。 这个功能可以用来实现透明代理或用来保护 web 服务器。例如：

iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080

MASQUERADE： 改写封包来源 IP 为防火墙 NIC IP，可以指定 port 对应的范围，进行完此处理动作后，直接跳往下一个规则 链（manglepostrouting）。这个功能与SNAT 略有不同，当进行 IP 伪装时，不需指定要伪装成哪个 IP，IP会从网卡直接读取，当使用拨 号接连时，IP通常是由ISP公司的DHCP 服务器指派的，这个时候 MASQUERADE 特别有用。范例如下：

iptables -t nat -A POSTROUTING -p TCP -j MASQUERADE --to-ports 1024-31000

LOG： 将封包相关讯息纪录在 /var/log 中，详细位置请查阅 /etc/syslog.conf 配置文件，进行完此处理动作后，将会继续匹配其规则。例如：

iptables -A INPUT -p tcp -j LOG --log-prefix &quot;INPUT packets&quot;

SNAT： 改写封包来源 IP 为某特定 IP 或 IP 范围，可以指定 port 对应的范围，进行完此处理动作后，将直接跳往下一个规则（mangleostrouting）。范例如下：

iptables -t nat -A POSTROUTING -p tcp -o eth0 -j SNAT --to-source?194.236.50.155-194.236.50.160:1024-32000　

DNAT： 改写封包目的地 IP 为某特定 IP 或 IP 范围，可以指定 port 对应的范围，进行完此处理动作后，将会直接跳往下一个规则链（filter:input 或 filter:forward）。范例如下：

iptables -t nat -A PREROUTING -p tcp -d 15.45.23.67 --dport 80 -j DNAT --to-destination 192.168.1.1-192.168.1.10:80-100

MIRROR： 镜射封包，也就是将来源 IP 与目的地 IP 对调后，将封包送回，进行完此处理动作后，将会中断过滤程序。

QUEUE： 中断过滤程序，将封包放入队列，交给其它程序处理。通过自行开发的处理程序，可以进行其它应用，例如：计算连接费用等。

RETURN： 结束在目前规则链中的过滤程序，返回主规则链继续过滤，如果把自定义规则链看成是一个子程序，那么这个动作，就相当于提前结束子程序并返回到主程序中。

MARK： 将封包标上某个代号，以便提供作为后续过滤的条件判断依据，进行完此处理动作后，将会继续匹配其它规则。范例如下：

iptables -t mangle -A PREROUTING -p tcp --dport 22 -j MARK --set-mark 2

IPv6流量的数据包过滤由iptables-ipv6软件包提供.规则保存在/etc/sysconfig/ip6tables中.

目前尚不支持:

1.REJECT 目标

2.nat 表

3.用 state 模块进行连接跟踪

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Netfilter 表和Netfilter链

Netfilter 的数据包流程

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++