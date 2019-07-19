### ping {#ping}

ping

语  法：ping [-dfnqrRv][-c&lt;完成次数&gt;][-i&lt;间隔秒数&gt;][-I&lt;网络界面&gt;][-l&lt;前置载入&gt;][-p&lt;范本样式&gt;][-s&lt;数据包大小&gt;][-t&lt;存活数值&gt;][主机名称或IP地址]

补充说明：执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。

参  数：

-d 使用Socket的SO_DEBUG功能。

-c&lt;完成次数&gt; 设置完成要求回应的次数。

-f 极限检测。

-i&lt;间隔秒数&gt; 指定收发信息的间隔时间。

-I&lt;网络界面&gt; 使用指定的网络界面送出数据包。

-l&lt;前置载入&gt; 设置在送出要求信息之前，先行发出的数据包。

-n 在輸出資料時不進行 IP 與主機名稱的反查，直接使用 IP 輸出(速度較快)；

-p&lt;范本样式&gt; 设置填满数据包的范本样式。

-q 不显示指令执行过程，开头和结尾的相关信息除外。

-r 忽略普通的Routing Table，直接将数据包送到远端主机上。

-R 记录路由过程。

-s&lt;数据包大小&gt; 设置数据包的大小。

-t&lt;存活数值&gt; 设置存活数值TTL的大小。

-v 详细显示指令的执行过程。

-w deadline 指定一個到期時間。以秒計算，不管在時間到之前送出或收到多少封包，時間一到就停止。

-M [do|dont] ：主要在偵測網路的 MTU 數值大小，兩個常見的項目是：

  do  ：代表傳送一個 DF (Dont Fragment) 旗標，讓封包不能重新拆包與打包；

  dont：代表不要傳送 DF 旗標，表示封包可以在其他主機上拆包與

e.g.:

[root@node ~]# ping -c3 8.8.8.8    

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.

64 bytes from 8.8.8.8: icmp_seq=1 ttl=46 time=136 ms

64 bytes from 8.8.8.8: icmp_seq=2 ttl= 46 time=138 ms&lt; /FONT&gt;

--- 8.8.8.8 ping statistics ---

3 packets transmitted, 2 received, 33% packet loss, time 2002ms

rtt min/avg/max/mdev = 136.635/137.742/138.850/1.168 ms                  #rtt往返时间