### netstat {#netstat}

netstat

功能说明：netstat用于显示与IP、TCP、UDP和ICMP协议相关的统计数据，一般用于检验本机各端口的网络连接情况。

语　　法：netstat [-acCeFghilMnNoprstuvVwx][-A&lt;网络类型&gt;][--ip]

补充说明：利用netstat指令可让你得知整个Linux系统的网络情况。

参　　数：

-a或-all 显示所有连线中的Socket。

-A&lt;网络类型&gt;或-&lt;网络类型&gt; 列出该网络类型连线中的相关地址。

-c或 -continuous 持续列出网络状态。

-C或-cache 显示路由器配置的快取信息。

-e或-extend 显示网络其他相关信息。

-F或-fib 显示FIB。

-g或-groups 显示多重广播功能群组组员名单。

-h或-help 在线帮助。

-i或-interfaces 显示网络界面信息表单。

-l或-listening 显示监控中的服务器的Socket。

-M 或-masquerade 显示伪装的网络连线。

-n或-numeric 直接使用IP地址，而不通过域名服务器。

-N或 -netlink或-symbolic 显示网络硬件外围设备的符号连接名称。

-o或-timers 显示计时器。

-p或 -programs 显示正在使用Socket的程序识别码和程序名称。

-r或-route 显示Routing Table。

-s或 -statistice 显示网络工作信息统计表。

-t或-tcp 显示TCP传输协议的连线状况。

-u或-udp 显示UDP传输协议的连线状况。

-v或-verbose 显示指令执行过程。

-V或-version 显示版本信息。

-w或 -raw 显示RAW传输协议的连线状况。

-x或-unix 此参数的效果和指定&quot;-A unix&quot;参数相同。

-ip或-inet 此参数的效果和指定&quot;-A inet&quot;参数相同

netstat 的一些常用选项

netstat -ntaupe #显示活跃的网络服务，建立的连接

netstat -tpnl     #

netstat -s

本选项能够按照各个协议分别显示其统计数据。如果我们的应用程序（如Web浏览器）运行速度比较慢，或者不能显示Web页之类的数据，那么我们就可以用本选项来查看一下所显示的信息。我们需要仔细查看统计数据的各行，找到出错的关键字，进而确定问题所在。

netstat -e

本选项用于显示关于以太网的统计数据。它列出的项目包括传送的数据报的总字节数、错误数、删除数、数据报的数量和广播的数量。这些统计数据既有发送的数据报数量，也有接收的数据报数量。这个选项可以用来统计一些基本的网络流量）。

netstat -r

本选项可以显示关于路由表的信息，类似于后面所讲使用route print命令时看到的 信息。除了显示有效路由外，还显示当前有效的连接。

netstat -a

本选项显示一个所有的有效连接信息列表，包括已建立的连接（ESTABLISHED），也包括监听连接请（LISTENING）的那些连接。

netstat -n

显示所有已建立的有效连接。