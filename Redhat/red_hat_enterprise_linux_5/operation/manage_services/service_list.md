#### service list {#service-list}

CentOS 6 默认服务列表 ~~

2012-03-07 11:01:03|~~ 分类： [\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\l &quot;m=0&amp;t=1&amp;c=fks_084066092081080068087087080095086081089064082095084074&quot; \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\o &quot;CentOS&quot;](http://ipeng.blog.163.com/blog/) CentOS | 字号 ~ 订阅

| 服务名称 | **功能** | **默认** | **建议** | **备注** |
| --- | --- | --- | --- | --- |
| NetworkManager | 用于自动连接网络，常用在 Laptop 上 | 开启 | 关闭 | 对服务器没用 |
| abrt-ccpp |  | 开启 | 开启 |  |
| abrt-oops |  | 开启 | 开启 |  |
| abrtd |  | 开启 | 开启 |  |
| acpid | 电源的开关等检测管理，常用在 Laptop 上 | 开启 | 关闭 | 对服务器没用 |
| atd | 在指定时间执行命令 | 开启 | 关闭 | 如果用 crond ，则可关闭它 |
| auditd | 审核守护进程 | 开启 | 自定 | 如果用 selinux ，需要开启它 |
| autofs | 文件系统自动加载和卸载 | 开启 | 自定 | 只在需要时开启它，可以停止 |
| avahi-daemon | 本地网络服务查找 | 开启 | 关闭 | 对服务器没用 |
| bluetooth | 蓝牙无线通讯 | 开启 | 关闭 | 对服务器没用 |
| certmonger |  | 关闭 | 关闭 |  |
| cpuspeed | 调节 cpu 速度用来省电，常用在 Laptop 上 | 开启 | 关闭 | 对服务器没用 |
| crond | 计划任务管理 | 开启 | 开启 | 很有用，开启 |
| cups | 通用 unix 打印服务 | 开启 | 关闭 | 对服务器没用 |
| dnsmasq | dns cache | 关闭 | 关闭 | 没用 |
| firstboot | 系统安装后初始设定 | 关闭 | 关闭 |  |
| haldaemon | 硬件信息收集服务 | 开启 | 开启 |  |
| ip6tables | ipv6 防火墙 | 开启 | 关闭 |  |
| iptables | ipv4 防火墙 | 开启 | 开启 |  |
| irqbalance | cpu 负载均衡 | 开启 | 自定 | 多核 cup 需要 |
| kdump | 硬件变动检测 | 关闭 | 关闭 | 服务器无用 |
| lvm2-monitor | lvm 监视 | 开启 | 关闭 | 非集群无用 |
| matahari-broker |  | 关闭 | 关闭 |  |
| matahari-host |  | 关闭 | 关闭 |  |
| matahari-network |  | 关闭 | 关闭 |  |
| matahari-service |  | 关闭 | 关闭 |  |
| matahari-sysconfig |  | 关闭 | 关闭 |  |
| mdmonitor | 软 raid 监视 | 开启 | 关闭 |  |
| messagebus | 负责在各个系统进程之间传递消息 | 开启 | 开启 | 如停用， haldaemon 启动会失败 |
| netconsole |  | 关闭 | 关闭 |  |
| netfs | 系统启动时自动挂载网络文件系统 | 开启 | 关闭 |  |
| network | 系统启动时激活所有网络接口 | 开启 | 开启 |  |
| nfs | 网络文件系统 | 关闭 | 关闭 |  |
| nfslock | nfs 相关 | 开启 | 关闭 |  |
| ntpd | 自动对时工具 | 关闭 | 开启 |  |
| ntpdate | 自动对时工具 | 关闭 | 关闭 |  |
| oddjobd | 与 D-BUS 相关 | 关闭 | 关闭 |  |
| portreserve | RPC 服务相关 | 开启 | 自定 |  |
| postfix | 替代 sendmail 的邮件服务器 | 开启 | 自定 | 如服务器不提供邮件服务，可关闭 |
| psacct | 负荷检测 | 关闭 | 关闭 |  |
| qpidd | 消息通信 | 开启 | 开启 |  |
| quota_nld |  | 关闭 | 关闭 |  |
| rdisc | 自动检测路由器 | 关闭 | 关闭 |  |
| restorecond | selinux 相关 | 关闭 | 关闭 |  |
| rpcbind |  | 开启 | 开启 |  |
| rpcgssd | NFS 相关 | 开启 | 关闭 |  |
| rpcidmapd | RPC name to UID/GID mapper | 开启 | 关闭 | NFS 相关 |
| rpcsvcgssd | NFS 相关 | 关闭 | 关闭 |  |
| rsyslog | 提供系统的登录档案记录 | 开启 | 开启 |  |
| saslauthd | sasl 认证服务相关 | 关闭 | 关闭 |  |
| smartd | 硬盘自动检测守护进程 | 关闭 | 关闭 |  |
| spice-vdagentd |  | 开启 | 开启 |  |
| sshd | ssh 服务端，可提供安全的 shell 登录 | 开启 | 开启 | 关闭后系统将无法远程登录 |
| sssd |  | 关闭 | 关闭 |  |
| sysstat |  | 开启 | 开启 |  |
| udev-post | 设备管理系统 | 开启 | 开启 |  |
| wdaemon |  | 关闭 | 关闭 |  |
| wpa_supplicant | 无线认证相关 | 关闭 | 关闭 |  |
| ypbind | network information service 客户端 | 关闭 | 关闭 |  |

**注：上述服务列表是在** **CentOS 6** **最小化桌面安装前提下显示出来的，其中红色字体标注的是默认最小化系统安装后的服务列表。**

附： 显示所有服务： # chkconfig --list 查看服务的说明： 在 centos 下这些服务都是脚本，我们可以利用 rpm 的命令来查看相应的说明。例如想查看 sshd 服务说明，请执行： # rpm

################

[\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\o &quot;Linux&quot;](http://xiexiejiao.cn) **Linux** **服务（** **Linux services** **）** 对于每个应用 Linux 的用户来说都很重要。关闭不需要的服务，可以让 Linux 运行的更高效，但并不是所有的 Linux 服务都可以关闭。今天安装了一次 [CentOs](http://en.wikipedia.org/wiki/Centos) Linux ，发现 Linux 启动的时候启动了好多服务，大部分都不知道是干什么的。因此着重了解了一下那些 Linux 服务（ Linux services ）可以关闭，那些 Linux 服务（ Linux services ）不能随意关闭。

在关闭 Linux 服务之前，需要了解一些概念：

1\.         什么是 Linux 服务 / 后台进程（ Linux services/daemons ）

2\.         什么是 Linux 服务运行级别（ Linux runlevels ）

3\.         以及各种用于管理 Linux 服务（ Linux services ）的工具。

Linux 服务（ Linux services ）管理工具介绍：

1\.         可以在命令行下使用 chkonfig 或 ntsysv 命令来管理 Linux 服务（ Linux services ）

2\.         使用具有图形用户界面的 system-config-services 命令。

3\.        GNOME 用户可以使用 系统 -&gt; 管理 -&gt; 服务器设置 -&gt;Services 来管理 Linux 服务（ Linux services ）

通常情况下下列 Linux 服务（ Linux services ）最好不要关闭，他们是一些系统级的服务，因为我是 Linux 入门级选手，所以我选择不去动他们。

1\.        acpid,~~

~

但笔者最常用的一个十分有用的 Linux 服务 是 **sshd** , 通过 ssh 连接到 Linux 上，这个是必不可少的。所以严重建议保留这个。还有，就是 **SendMail** 服务，笔者使用的是 CentOs Linux 5.2, 居然安装时候默认就安装了改服务，因为没有用，所以关闭之。这个服务启动的够慢的。 最后请确定修改的是运行级别 3 和 5 。

Linux 服务（ Linux services ）： **NetworkManager, NetworkManagerDispatcher** NetworkManager 是一个自动切换网络连接的后台进程。很多笔记本用户都需要启用该功能，它让你能够在无线网络和有线网络之间切换。大多数台式机用户应该关闭该服务。一些 DHCP 用户可能需要开启它。 Linux 服务（ Linux services ）： **acpid** ACPI （全称 Advanced Configuration and Power Interface ）服务是电源管理接口。建议所有的笔记本用户开启它。一些服务器可能不需要 acpi 。支持的通用操作有： &quot; 电源开关 &quot; ， &quot; 电池监视 &quot; ， &quot; 笔记本 Lid 开关 &quot; ， &quot; 笔记本显示屏亮度 &quot; ， &quot; 休眠 &quot; ， &quot; 挂机 &quot; ，等等。

Linux 服务（ Linux services ）： **anacron, atd, cron** 这几个调度程序有很小的差别。 建议开启 cron ，如果你的电脑将长时间运行，那就更应该开启它。对于服务器，应该更深入了解以确定应该开启哪个调度程序。大多数情况下，笔记本 / 台式机应该关闭 atd 和 anacron 。注意：一些任务的执行需要 anacron ，比如：清理 /tmp 或 /var 。

Linux 服务（ Linux services ）： **apmd** 一些笔记本和旧的硬件使用 apmd 。如果你的电脑支持 acpi ，就应该关闭 apmd 。如果支持 acpi ，那么 apmd 的工作将会由 acpi 来完成。

Linux 服务（ Linux services ）： **autofs** 该服务自动挂载可移动存储器（比如 USB 硬盘）。如果你使用移动介质（比如移动硬盘， U 盘），建议启用这个服务。

Linux 服务（ Linux services ）： **avahi-daemon, avahi-dnsconfd** Avahi 是 zeroconf 协议的实现。它可以在没有 DNS 服务的局域网里发现基于 zeroconf 协议的设备和服务。它跟 mDNS 一样。除非你有兼容的设备或使用 zeroconf 协议的服务，否则应该关闭它。

Linux 服务（ Linux services ）： **bluetooth, hcid, hidd, sdpd, dund, pand** 蓝牙（ Bluetooth ）是给无线便携设备使用的（非 wifi, 802.11 ）。很多笔记本提供蓝牙支持。有蓝牙鼠标，蓝牙耳机和支持蓝牙的手机。很多人都没有蓝牙设备或蓝牙相关的服务，所以应该关闭它。其他蓝牙相关的服务有： hcid 管理所有可见的蓝牙设备， hidd 对输入设备（键盘，鼠标）提供支持， dund 支持通过蓝牙拨号连接网络， pand 允许你通过蓝牙连接以太网。

Linux 服务（ Linux services ）： **capi** 仅仅对使用 ISDN 设备的用户有用。大多数用户应该关闭它。

Linux 服务（ Linux services ）： **cpuspeed** 该服务可以在运行时动态调节 CPU 的频率来节约能源（省电）。许多笔记本的 CPU 支持该特性，现在，越来越多的台式机也支持这个特性了。如果你的 CPU 是： Petium-M ， Centrino ， AMD PowerNow ， Transmetta ， Intel SpeedStep ， Athlon-64 ， Athlon-X2 ， Intel Core 2 中的一款，就应该开启它。如果你想让你的 CPU 以固定频率运行的话就关闭它。

Linux 服务（ Linux services ）： **cron** 参见 **anacron** 。

Linux 服务（ Linux services ）： **cupsd, cups-config-daemon** 打印机相关。如果你有能在 Fedora 中驱动的 CUPS 兼容的打印机，你应该开启它。

Linux 服务（ Linux services ）： **dc_client, dc_server** 磁盘缓存（ Distcache ）用于分布式的会话缓存。主要用在 SSL/TLS 服务器。它可以被 Apache 使用。大多数的台式机应该关闭它。

Linux 服务（ Linux services ）： **dhcdbd** 这是一个让 DBUS 系统控制 DHCP 的接口。可以保留默认的关闭状态。

Linux 服务（ Linux services ）： **diskdump, netdump** 磁盘转储（ Diskdump ）用来帮助调试内核崩溃。内核崩溃后它将保存一个 &quot;dump&quot; 文件以供分析之用。网络转储（ Netdump ）的功能跟 Diskdump 差不多，只不过它可以通过网络来存储。除非你在诊断内核相关的问题，它们应该被关闭。

Linux 服务（ Linux services ）： **dund** 参见 **bluetooth** 。

Linux 服务（ Linux services ）： **firstboot** 该服务是 Fedora 安装过程特有的。它执行在安装之后的第一次启动时仅仅需要执行一次的特定任务。它可以被关闭。

Linux 服务（ Linux services ）： **gpm** 终端鼠标指针支持（无图形界面）。如果你不使用文本终端（ CTRL-ALT-F1, F2.. ），那就关闭它。不过，我在运行级别 3 开启它，在运行级别 5 关闭它。

Linux 服务（ Linux services ）： **hidd** 参见 **bluetooth** 。

Linux 服务（ Linux services ）： **hplip, hpiod, hpssd** HPLIP 服务在 Linux 系统上实现 HP 打印机支持，包括 Inkjet ， DeskJet ， OfficeJet ， Photosmart ， Business InkJet 和一部分 LaserJet 打印机。这是 HP 赞助的惠普 Linux 打印项目（ HP Linux Printing Project ）的产物。如果你有相兼容的打印机，那就启用它。

Linux 服务（ Linux services ）： **iptables** 它是 Linux 标准的防火墙（软件防火墙）。如果你直接连接到互联网（如， cable ， DSL ， T1 ），建议开启它。如果你使用硬件防火墙（比如： D-Link ， Netgear ， Linksys 等等），可以关闭它。强烈建议开启它。

Linux 服务（ Linux services ）： **ip6tables** 如果你不知道你是否在使用 IPv6 ，大部分情况下说明你没有使用。该服务是用于 IPv6 的软件防火墙。大多数用户都应该关闭它。阅读这里了解如何关闭 Fedora 的 IPv6 支持。

Linux 服务（ Linux services ）： **irda, irattach** IrDA 提供红外线设备（笔记本， PDAs ，手机，计算器等等）间的通讯支持。大多数用户应该关闭它。

Linux 服务（ Linux services ）： **irqbalance** 在多处理器系统中，启用该服务可以提高系统性能。大多数人不使用多处理器系统，所以关闭它。但是我不知道它作用于多核 CPUs 或 超线程 CPUs 系统的效果。在单 CPU 系统中关闭它应该不会出现问题。

Linux 服务（ Linux services ）： **isdn** 这是一种互联网的接入方式。除非你使用 ISDN 猫来上网，否则你应该关闭它。

Linux 服务（ Linux services ）： **kudzu** 该服务进行硬件探测，并进行配置。如果更换硬件或需要探测硬件更动，开启它。但是绝大部分的台式机和服务器都可以关闭它，仅仅在需要时启动。

Linux 服务（ Linux services ）： **lm_sensors** 该服务可以探测主板感应器件的值或者特定硬件的状态（一般用于笔记本电脑）。你可以通过它来查看电脑的实时状态，了解电脑的健康状况。它在 GKrellM 用户中比较流行。查看 lm_sensors 的主页获得更多信息。如果没有特殊理由，建议关闭它。

Linux 服务（ Linux services ）： **mctrans** 如果你使用 SELinux 就开启它。默认情况下 CentOs 和 Fedora Core 开启 SELinux 。

Linux 服务（ Linux services ）： **mdmonitor** 该服务用来监测 [\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\o &quot;Software&quot;](http://xiexiejiao.cn/software) Software