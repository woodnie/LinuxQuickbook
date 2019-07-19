### arping {#arping}

说明：arping是用于发送arp请求到一个相邻主机的工具，arping使用arp数据包，通过ping命令检查设备上的硬件地址。

语法：arping [-fqbDUAV] [-c count] [-w timeout] [-I device] [-s source] destination

常用参数：

-b：用于发送以太网广播帧（FFFFFFFFFFFF）。arping一开始使用广播地址，在收到响应后就使用unicast地址。

-q：quiet output 不显示任何信息；

-f：表示在收到第一个响应包后就退出；

-w timeout：设定一个超时时间，单位是秒。如果到了指定时间，arping 还没有完全收到响应则退出；

-c count：表示发送指定数量的 ARP 请求数据包后就停止。如果制定了deadline选项，则arping会等待相同数量的arp响应包，直到超时为止。

-s source：设定 arping 发送的 arp 数据包中的 SPA 字段的值。如果为空，则按下面处理

如果是 DAD 模式（冲突地址探测），则设置为 0.0.0.0，如果是 Unsolicited ARP 模式（Gratutious ARP）则设置为目标地址，否则从路由表得出。

-I interface：设置ping使用的网络接口；

destination：设置目标地址。