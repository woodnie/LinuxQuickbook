### ifconfig {#ifconfig}

ifconfig

[root@localhost ~]# ifconfig                                           ## 查看相关接口信息

[root@localhost ~]# ifconfig interface   start/stop       ##启用/禁用相关接口

ifconfig 主要是可以手動的啟動、觀察與修改網路介面的相關參數，可以修改的參數很多啊，包括 IP 參數以及 MTU 等等都可以修改，他的語法如下：

[root@www ~]# ifconfig {interface} {up|down}  &lt;== 觀察與啟動介面

[root@www ~]# ifconfig interface {options}    &lt;== 設定與修改介面

選項與參數：

interface：網路卡介面代號，包括 eth0, eth1, ppp0 等等

options  ：可以接的參數，包括如下：

           up, down ：啟動 (up) 或關閉 (down) 該網路介面(不涉及任何參數)

           mtu      ：可以設定不同的 MTU 數值，例如 mtu 1500 (單位為 byte)

           netmask  ：就是子遮罩網路；

           broadcast：就是廣播位址啊！

# 範例一：觀察所有的網路介面(直接輸入 ifconfig)

[root@www ~]# ifconfig

eth0      Link encap:Ethernet  HWaddr 08:00:27:F3:5D:23

         inet addr:192.168.1.11  Bcast:192.168.1.255  Mask:255.255.255.0

         inet6 addr: fe80::a00:27ff:fef3:5d23/64 Scope:Link

         UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

         RX packets:485 errors:0 dropped:0 overruns:0 frame:0

         TX packets:143 errors:0 dropped:0 overruns:0 carrier:0

         collisions:0 txqueuelen:1000

         RX bytes:46082 (45.0 KiB)  TX bytes:19559 (19.1 KiB)

一般來說，直接輸入 ifconfig 就會列出目前已經被啟動的卡，不論這個卡是否有給予 IP，都會被顯示出來。而如果是輸入 ifconfig eth0 ，則會秀出這張介面的相關資料， 而不管該介面是否有啟動。所以如果你想要知道某張網路卡的 Hardware Address，直接輸入『 ifconfig &quot;網路介面代號&quot; 』即可喔！ ^_^！至於上表出現的各項資料是這樣的(資料排列由上而下、由左而右)：

eth0：就是網路卡的代號，也有 lo 這個 loopback ；

HWaddr：就是網路卡的硬體位址，俗稱的 MAC 是也；

inet addr：IPv4 的 IP 位址，後續的 Bcast, Mask 分別代表的是 Broadcast 與 netmask 喔！

inet6 addr：是 IPv6 的版本的 IP ，我們沒有使用，所以略過；

MTU：就是第二章談到的 MTU 啊！

RX：那一行代表的是網路由啟動到目前為止的封包接收情況， packets 代表封包數、errors 代表封包發生錯誤的數量、 dropped 代表封包由於有問題而遭丟棄的數量等等

TX：與 RX 相反，為網路由啟動到目前為止的傳送情況；

collisions：代表封包碰撞的情況，如果發生太多次， 表示你的網路狀況不太好；

txqueuelen：代表用來傳輸資料的緩衝區的儲存長度；

RX bytes, TX bytes：總接收、傳送的位元組總量

透過觀察上述的資料，大致上可以瞭解到你的網路情況，尤其是那個 RX, TX 內的 error 數量， 以及是否發生嚴重的 collision 情況，都是需要注意的喔！ ^_^

# 範例二：暫時修改網路介面，給予 eth0 一個 192.168.100.100/24 的參數

[root@www ~]# ifconfig eth0 192.168.100.100

# 如果不加任何其他參數，則系統會依照該 IP 所在的 class 範圍，

# 自動的計算出 netmask 以及 network, broadcast 等 IP 參數；

# 如果還想要更改不同的網路參數，那可以這樣做：

[root@www ~]# ifconfig eth0 192.168.100.100 \

&gt; netmask 255.255.255.128 mtu 8000

# 設定不同參數的網路介面，同時設定 MTU 的數值！

[root@www ~]# ifconfig eth0 mtu 9000

# 僅修改該介面的 MTU 數值，其他的保持不動！

[root@www ~]# ifconfig eth0:0 192.168.50.50

# 仔細看那個介面， eth0:0 喔！那就是在該網路介面上，再模擬一個網路介面，

# 亦即是在一張網路卡上面設定多個 IP 的意思啦！

[root@www ~]# ifconfig

eth0    Link encap:Ethernet  HWaddr 08:00:27:F3:5D:23

       inet addr:192.168.100.100  Bcast:192.168.100.127  Mask:255.255.255.128

       inet6 addr: fe80::a00:27ff:fef3:5d23/64 Scope:Link

       UP BROADCAST RUNNING MULTICAST  MTU:9000  Metric:1

       RX packets:1305 errors:0 dropped:0 overruns:0 frame:0

       TX packets:230 errors:0 dropped:0 overruns:0 carrier:0

       collisions:0 txqueuelen:1000

       RX bytes:123306 (120.4 KiB)  TX bytes:30671 (29.9 KiB)

eth0:0  Link encap:Ethernet  HWaddr 08:00:27:F3:5D:23

       inet addr:192.168.50.50  Bcast:192.168.50.255  Mask:255.255.255.0

       UP BROADCAST RUNNING MULTICAST  MTU:9000  Metric:1

# 仔細看，是否與硬體有關的資訊都相同啊！沒錯！因為是同一張網卡嘛！

# 那如果想要將剛剛建立的那張 eth0:0 關閉就好，不影響原有的 eth0 呢？

[root@www ~]# ifconfig eth0:0 down

# 關掉 eth0:0 這個介面。那如果想用預設值啟動 eth1：『ifconfig eth1 up』即可達成

# 範例三：將手動的處理全部取消，使用原有的設定值重建網路參數：

[root@www ~]# /etc/init.d/network restart

# 剛剛設定的資料全部失效，會以 ifcfg-ethX 的設定為主！

使用 ifconfig 可以暫時手動來設定或修改某個介面卡的相關功能，並且也可以透過 eth0:0 這種虛擬的網路介面來設定好一張網路卡上面的多個 IP 喔！手動的方式真是簡單啊！並且設定錯誤也不打緊，因為我們可以利用 /etc/init.d/network restart 來重新啟動整個網路介面，那麼之前手動的設定資料會全部都失效喔！另外， 要啟動某個網路介面，但又不讓他具有 IP 參數時，直接給他 ifconfig eth0 up 即可！ 這個動作經常在無線網卡當中會進行，因為我們必須要啟動無線網卡讓他去偵測 AP 存在與否啊！