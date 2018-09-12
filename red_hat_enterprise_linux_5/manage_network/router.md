### router {#router}

route

[root@www ~]# route [-nee]

[root@www ~]# route add [-net|-host] [ 網域或主機 ] netmask [mask] [gw|dev]

[root@www ~]# route del [-net|-host] [ 網域或主機 ] netmask [mask] [gw|dev]

觀察的參數：

  -n   ：不要使用通訊協定或主機名稱，直接使用 IP 或 port number ；

  -ee ：使用更詳細的資訊來顯示

  增加 (add) 與刪除 (del) 路由的相關參數：

  -net     ：表示後面接的路由為一個網域；

  -host   ：表示後面接的為連接到單部主機的路由；

  netmask ：與網域有關，可以設定 netmask 決定網域的大小；

  gw       ： gateway 的簡寫，後續接的是 IP 的數值喔，與 dev 不同；

  dev     ：如果只是要指定由那一塊網路卡連線出去，則使用這個設定，後面接 eth0 等

# 範例一：單純的觀察路由狀態

[root@www ~]# route -n

Kernel IP routing table

Destination     Gateway         Genmask         Flags Metric Ref    Use Iface

192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0

169.254.0.0     0.0.0.0         255.255.0.0     U     0      0        0 eth0

0.0.0.0         192.168.1.254   0.0.0.0         UG    0      0        0 eth0

[root@www ~]# route

Kernel IP routing table

Destination     Gateway         Genmask         Flags Metric Ref    Use Iface

192.168.1.0     *               255.255.255.0   U     0      0        0 eth0

169.254.0.0     *               255.255.0.0     U     0      0        0 eth0

default         gateway.vbird   0.0.0.0         UG    0      0        0 eth0

由上面的例子當中仔細觀察 route 與 route -n 的輸出結果，你可以發現有加 -n 參數的主要是顯示出 IP ，至於使用 route 而已的話，顯示的則是『主機名稱』喔！也就是說，在預設的情況下， route 會去找出該 IP 的主機名稱，如果找不到呢？ 就會顯示的鈍鈍的 ( 有點小慢 ) ，所以說，鳥哥通常都直接使用 route -n 啦！ 由上面看起來，我們也知道 default = 0.0.0.0/0.0.0.0 ， 而上面的資訊有哪些你必須要知道的呢？

Destination, Genmask ：這兩個玩意兒就是分別是 network 與 netmask 啦！所以這兩個咚咚就組合成為一個完整的網域囉！

Gateway ：該網域是通過哪個 gateway 連接出去的？如果顯示 0.0.0.0 表示該路由是直接由本機傳送，亦即可以透過區域網路的 MAC 直接傳訊；如果有顯示 IP 的話，表示該路由需要經過路由器 ( 通訊閘 ) 的幫忙才能夠傳送出去。

Flags ：總共有多個旗標，代表的意義如下：

U (route is up) ：該路由是啟動的；

H (target is a host) ：目標是一部主機 (IP) 而非網域；

G (use gateway) ：需要透過外部的主機 (gateway) 來轉遞封包；

R (reinstate route for dynamic routing) ：使用動態路由時，恢復路由資訊的旗標；

D (dynamically installed by daemon or redirect) ：已經由服務或轉 port 功能設定為動態路由

M (modified from routing daemon or redirect) ：路由已經被修改了；

! (reject route) ：這個路由將不會被接受 ( 用來抵擋不安全的網域！ )

Iface ：這個路由傳遞封包的介面。

此外，觀察一下上面的路由排列順序喔，依序是由小網域 (192.168.1.0/24 是 Class C) ，逐漸到大網域 (169.254.0.0/16 Class B) 最後則是預設路由 (0.0.0.0/0.0.0.0) 。 然後當我們要判斷某個網路封包應該如何傳送的時候，該封包會經由這個路由的過程來判斷喔！ 舉例來說，我上頭僅有三個路由，若我有一個傳往 192.168.1.20 的封包要傳遞，那首先會找 192.168.1.0/24 這個網域的路由，找到了！所以直接由 eth0 傳送出去；

如果是傳送到 Yahoo 的主機呢？ Yahoo 的主機 IP 是 119.160.246.241 ，我們通過判斷 1) 不是 192.168.1.0/24 ， 2) 不是 169.254.0.0/16 結果到達 3)0/0 時， OK ！傳出去了，透過 eth0 將封包傳給 192.168.1.254 那部 gateway 主機啊！所以說，路由是有順序的。

因此當你重複設定多個同樣的路由時， 例如在你的主機上的兩張網路卡設定為相同網域的 IP 時，會出現什麼情況？會出現如下的情況：

Kernel IP routing table

Destination    Gateway         Genmask         Flags Metric Ref    Use Iface

192.168.1.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0

192.168.1.0    0.0.0.0         255.255.255.0   U     0      0        0 eth1

也就是說，由於路由是依照順序來排列與傳送的， 所以不論封包是由那個介面 (eth0, eth1) 所接收，都會由上述的 eth0 傳送出去， 所以，在一部主機上面設定兩個相同網域的 IP 本身沒有什麼意義！有點多此一舉就是了。 除非是類似虛擬主機 (Xen, VMware 等軟體 ) 所架設的多主機時，才會有這個必要～

# 範例二：路由的增加與刪除

[root@www ~]# route del -net 169.254.0.0 netmask 255.255.0.0 dev eth0

# 上面這個動作可以刪除掉 169.254.0.0/16 這個網域！

# 請注意，在刪除的時候，需要將路由表上面出現的資訊都寫入

# 包括 netmask , dev 等等參數喔！注意注意

[root@www ~]# route add -net 192.168.100.0 netmask 255.255.255.0 dev eth0

# 透過 route add 來增加一個路由！請注意，這個路由的設定必須要能夠與你的網路互通。

# 舉例來說，如果我下達底下的指令就會顯示錯誤：

# route add -net 192.168.200.0 netmask 255.255.255.0 gw 192.168.200.254

# 因為我的主機內僅有 192.168.1.11 這個 IP ，所以不能直接與 192.168.200.254

# 這個網段直接使用 MAC 互通！這樣說，可以理解嗎？

[root@www ~]# route add default gw 192.168.1.250

# 增加預設路由的方法！請注意，只要有一個預設路由就夠了喔！

# 同樣的，那個 192.168.1.250 的 IP 也需要能與你的 LAN 溝通才行！

# 在這個地方如果你隨便設定後，記得使用底下的指令重新設定你的網路

# /etc/init.d/network restart

如果是要進行路由的刪除與增加，那就得要參考上面的例子了，其實，使用 man route 裡面的資料就很豐富了！仔細查閱一下囉！ 你只要記得，當出現『 SIOCADDRT: Network is unreachable 』 這個錯誤時，肯定是由於 gw 後面接的 IP 無法直接與你的網域溝通 (Gateway 並不在你的網域內 ) ， 所以，趕緊檢查一下是否輸入錯誤啊！