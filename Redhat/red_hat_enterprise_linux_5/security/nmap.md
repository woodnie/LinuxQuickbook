### nmap {#nmap}

**Nmap** **参考指南** **(Man Page)**

[http://nmap.org/man/zh/](http://nmap.org/man/zh/)

**nmap**

NAME~~~~~~nmap - Network exploration tool and security / port scanne

nmap~.rpm #nmap-frontend #nmap 图像化前端 nmap xnmap #nmap-frontend..rpm 提供的工具

[root@node ~]# nmap -sT 192.168.1.201~~~#TCP 端口扫描 [root@node ~]# nmap -sU 192.168.1.201 ~~~ #UDP 端口扫描 [root@node ~]# nmap ~ -sP 192.168.1.* ~~~~~ # -sP 是用于扫描整个局域网段 [root@node ~]# nmap -sS 192.168.1.201 ~~ ~ #TCP 同步（ SYN ）端口扫描选项可以进行更加隐蔽的扫描，并防止被目标主机检测到。 [root@node ~]# nmap ~~ -v 192.168.1.201 ~~~~ # 显示详细信息 [root@node ~]# nmap ~~ -P0 192.168.1.201 ~~~ # 扫描前不 ping[root@node ~]# nmap -A ~ 192.168.1.201 ~~~ # 扫描 OS 版本

选项可以进行更加隐蔽的扫描，并防止被目标主机检测到。 但此方式需要用户拥有 root 权限 -sF -sX -sN 则可以进行一些超常的 扫描。假如目标主机安装了过滤和日志软件来检测同步空闲字符 SYN ， 那么 -sS 的隐蔽作用就失效了，此时可以采用 -sF( 隐蔽 FIN), -sX(Xmas Tree) 以及 -sN(Null) 方式的扫描。这里需要注意的是由于微软的坚 持和独特，对于运行 Windows 95/98 或者 NT 的机器 FIN,Xmas 或者 Null 的扫描结果将都是端口关闭，由此也是推断目标主机可能运行 Windows 操作系统的一种方法。

[root@node ~]# nmap~-sU/ -sT /-sS[root@node ~]# nmap 192.168.1.1-254 -oN /tmp/hosts~~# 扫描信息保存到文件 [root@node ~]# nmap~-sP 192.168.1.*~~# 扫描 192.168.1 网段有那些机子

nmap -P0 -A -v

rpm -vhU [http://nmap.org/dist/nmap-6.01-1.i386.rpm](http://nmap.org/dist/nmap-6.01-1.i386.rpm)

rpm -vhU [http://nmap.org/dist/zenmap-6.01-1.noarch.rpm](http://nmap.org/dist/zenmap-6.01-1.noarch.rpm)

rpm -vhU [http://nmap.org/dist/ncat-6.01-1.i386.rpm](http://nmap.org/dist/ncat-6.01-1.i386.rpm)

rpm -vhU [http://nmap.org/dist/nping-0.6.01-1.i386.rpm](http://nmap.org/dist/nping-0.6.01-1.i386.rpm)  

 ######################

**Nmap** 是一款网络扫描和主机检测的非常有用的工具。 是不局限于仅仅收集信息和枚举，同时可以用来作为一个漏洞探测器或安全扫描器。它可以适用于 winodws,linux,mac 等操作系统。 Nmap 是一款非常强大的实用工具 , 可用于：

检测活在网络上的主机（主机发现） 检测主机上开放的端口（端口发现或枚举） 检测到相应的端口（服务发现）的软件和版本 检测操作系统，硬件地址，以及软件版本 检测脆弱性的漏洞（ Nmap 的脚本） Nmap 是一个非常普遍的工具，它有命令行界面和图形用户界面。本人包括以下方面的内容 :

介绍 Nmap

扫描中的重要参数

操作系统检测

Nmap 使用教程

Nmap 使用不同的技术来执行扫描，包括： TCP 的 connect （）扫描， TCP 反向的 ident 扫描， FTP 反弹扫描等。所有这些扫描的类型有自己的优点和缺点，我们接下来将讨论这些问题。

Nmap 的使用取决于目标主机 , 因为有一个简单的（基本）扫描和预先扫描之间的差异。我们需要使用一些先进的技术来绕过防火墙和入侵检测 / 防御系统，以获得正确的结果。下面是一些基本的命令和它们的用法的例子：

扫描单一的一个主机，命令如下：

**#nmap nxadmin.com**

**#nmap 192.168.1.2**

扫描整个子网 , 命令如下 :

**#nmap 192.168.1.1/24**

扫描多个目标 , 命令如下：

**#nmap 192.168.1.2 192.168.1.5**

扫描一个范围内的目标 , 如下：

**#nmap 192.168.1.1-100 (** 扫描 IP 地址为 192.168.1.1-192.168.1.100 内的所有主机 )

如果你有一个 ip 地址列表，将这个保存为一个 txt 文件，和 namp 在同一目录下 , 扫描这个 txt 内的所有主机，命令如下：

**#nmap -iL target.txt**

如果你想看到你扫描的所有主机的列表，用以下命令 :

**#nmap -sL 192.168.1.1/24**

扫描除过某一个 ip 外的所有子网主机 , 命令：

**#nmap 192.168.1.1/24 -exclude 192.168.1.1**

扫描除过某一个文件中的 ip 外的子网主机命令

**#nmap 192.168.1.1/24 -exclude file xxx.txt** ~(xxx.txt 中的文件将会从扫描的主机中排除 )

扫描特定主机上的 80,21,23 端口 , 命令如下

**#nmap -p80,21,23 192.168.1.1**

[从上面我们已经了解了](http://www.nxadmin.com/wp-content/uploads/2012/07/Nmap.jpg) Nmap 的基础知识，下面我们深入的探讨一下 Nmap 的扫描技术 .

**Tcp SYN Scan (sS)**

这是一个基本的扫描方式 , 它被称为半开放扫描，因为这种技术使得 Nmap 不需要通过完整的握手，就能获得远程主机的信息。 Nmap 发送 SYN 包到远程主机，但是它不会产生任何会话 . 因此不会在目标主机上产生任何日志记录 , 因为没有形成会话。这个就是 SYN 扫描的优势 .

如果 Nmap 命令中没有指出扫描类型 , 默认的就是 Tcp SYN. 但是它需要 root/administrator 权限 .

**#nmap -sS 192.168.1.1**

**Tcp connect() scan(sT)**

如果不选择 SYN 扫描 ,TCP connect() 扫描就是默认的扫描模式 . 不同于 Tcp SYN 扫描 ,Tcp connect() 扫描需要完成三次握手 , 并且要求调用系统的 connect().Tcp connect() 扫描技术只适用于找出 TCP 和 UDP 端口 .

**#nmap -sT 192.168.1.1**

**Udp scan(sU)**

顾名思义 , 这种扫描技术用来寻找目标主机打开的 UDP 端口 . 它不需要发送任何的 SYN 包，因为这种技术是针对 UDP 端口的。 UDP 扫描发送 UDP 数据包到目标主机，并等待响应 , 如果返回 ICMP 不可达的错误消息，说明端口是关闭的，如果得到正确的适当的回应，说明端口是开放的 .

**#nmap -sU 192.168.1.1**

**FIN scan (sF)**

有时候 Tcp SYN 扫描不是最佳的扫描模式 , 因为有防火墙的存在 . 目标主机有时候可能有 IDS 和 IPS 系统的存在 , 防火墙会阻止掉 SYN 数据包。发送一个设置了 FIN 标志的数据包并不需要完成 TCP 的握手 .

root@bt **:~# nmap -sF 192.168.1.8**

Starting Nmap 5.51~at 2012-07-08 19:21 PKT

Nmap scan report for 192.168.1.8

Host is up (0.000026s latency).

Not shown: 999 closed ports

PORT STATE SERVICE

111/tcp open|filtered rpcbind

FIN 扫描也不会在目标主机上创建日志 (FIN 扫描的优势之一 ). 个类型的扫描都是具有差异性的 ,FIN 扫描发送的包只包含 FIN 标识 ,NULL 扫描不发送数据包上的任何字节 ,XMAS 扫描发送 FIN 、 PSH 和 URG 标识的数据包 .

**PING Scan (sP)**

PING 扫描不同于其它的扫描方式，因为它只用于找出主机是否是存在在网络中的 . 它不是用来发现是否开放端口的 .PING 扫描需要 ROOT 权限，如果用户没有 ROOT 权限 ,PING 扫描将会使用 connect() 调用 .

**#nmap -sP 192.168.1.1**

**版本检测** **(sV)**

版本检测是用来扫描目标主机和端口上运行的软件的版本 . 它不同于其它的扫描技术，它不是用来扫描目标主机上开放的端口，不过它需要从开放的端口获取信息来判断软件的版本 . 使用版本检测扫描之前需要先用 TCP SYN 扫描开放了哪些端口 .

**#nmap -sV 192.168.1.1**

**Idle scan (sL)**

Idle scan 是一种先进的扫描技术，它不是用你真实的主机 Ip 发送数据包，而是使用另外一个目标网络的主机发送数据包 .

**#nmap -sL 192.168.1.6 192.168.1.1**

Idle scan 是一种理想的匿名扫描技术 , 通过目标网络中的 192.168.1.6 向主机 192.168.1.1 发送数据，来获取 192.168.1.1 开放的端口

有需要其它的扫描技术，如 bounce （ FTP 反弹） , fragmentation scan （碎片扫描） , IP protocol scan （ IP 协议扫描） , 以上讨论的是几种最主要的扫描方式 .

**Nmap** **的** **OS** **检测（** **O** **）**

Nmap 最重要的特点之一是能够远程检测操作系统和软件， Nmap 的 OS 检测技术在渗透测试中用来了解远程主机的操作系统和软件是非常有用的，通过获取的信息你可以知道已知的漏洞。 Nmap 有一个名为的 nmap-OS-DB 数据库，该数据库包含超过 2600 操作系统的信息。 Nmap 把 TCP 和 UDP 数据包发送到目标机器上，然后检查结果和数据库对照。

Initiating SYN Stealth Scan at 10:21Scanning localhost (127.0.0.1) [1000 ports]Discovered open port 111/tcp on 127.0.0.1Completed SYN Stealth Scan at 10:21, 0.08s elapsed (1000 total ports)Initiating OS detection (try #1) against localhost (127.0.0.1)Retrying OS detection (try #2) against localhost (127.0.0.1)

上面的例子清楚地表明， Nmap 的首次发现开放的端口，然后发送数据包发现远程操作系统。操作系统检测参数是 O （大写 O ）

[~](http://www.nxadmin.com/wp-content/uploads/2012/07/Nmap-os.jpg)

Nmap 的操作系统指纹识别技术：

设备类型（路由器，工作组等） 运行（运行的操作系统） 操作系统的详细信息（操作系统的名称和版本） 网络距离（目标和攻击者之间的距离跳）

如果远程主机有防火墙， IDS 和 IPS 系统，你可以使用 -PN 命令来确保不 ping 远程主机，因为有时候防火墙会组织掉 ping 请求 .-PN 命令告诉 Nmap 不用 ping 远程主机。

**# nmap -O -PN 192.168.1.1/24**

以上命令告诉发信主机远程主机是存活在网络上的，所以没有必要发送 ping 请求 , 使用 -PN 参数可以绕过 PING 命令 , 但是不影响主机的系统的发现 .

Nmap 的操作系统检测的基础是有开放和关闭的端口，如果 OS scan 无法检测到至少一个开放或者关闭的端口，会返回以下错误：

Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port

OS Scan 的结果是不可靠的，因为没有发现至少一个开放或者关闭的端口 .

[~](http://www.nxadmin.com/wp-content/uploads/2012/07/nmap-osscan.jpg)

这种情况是非常不理想的，应该是远程主机做了针对操作系统检测的防范。如果 Nmap 不能检测到远程操作系统类型，那么就没有必要使用 -osscan_limit 检测。

[想好通过](http://www.nxadmin.com/wp-content/uploads/2012/07/Nmap-os2.jpg) Nmap 准确的检测到远程操作系统是比较困难的，需要使用到 Nmap 的猜测功能选项 ,~-osscan-guess 猜测认为最接近目标的匹配操作系统类型。

**_# nmap -O --osscan-guess 192.168.1.1_**

总结

Nmap 是一个非常强大的工具，它具有覆盖渗透测试的第一方面的能力，其中包括信息的收集和统计。本文从初级到高级的讲解了 Nmap 入侵扫描工具的使用 . 希望对大家有所帮助 .