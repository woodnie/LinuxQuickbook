#### nis {#nis}

nis

网络信息服务

1.确保系统安装以下3个包：

[root@server ~]# yum list yp*

ypserv   ：提供 NIS Server 端的設定套件

yp-tools ：提供 NIS 相關的查尋指令功能

ypbind   ：提供 NIS Client 端的設定套件

portmap  ：就是 RPC 一定需要的資料啊！              

make           用于生成NIS数据库

2.服务器端配置

2.1.设置NIS域名

[root@server ~]# vi /etc/sysconfig/network

NISDOMAIN=mynisdomain

[root@server ~]# domainname mynisdomain    

[root@server ~]# /usr/lib/yp/ypinit -m                          #生成NIS数据库(Ctrl+D)

[root@server ~]# /usr/lib64/yp/ypinit -m

2.2.启动相关服务

[root@server ~]# chkconfig  portmap on

[root@server ~]# chkconfig  ypserv on

[root@server ~]# chkconfig  yppasswdd on

[root@server ~]# service portmap start

[root@server ~]# service ypserv start

[root@server ~]# service yppasswdd start    #该服务用于客户端更改密码，会同时更新nis数据库和/etc/passwd

3.客户端配置

yp-tools                              

ypbind                            

portmap                        

authconfig

3.1使用setup配置nis认证

使用setup命令的authconfig工具配置你的客户端使用NIS进行身份验证，选定&quot;Use NIS&quot;，

在&quot;Domain：&quot;后指定你的NIS域，

在&quot;Server：&quot;后指定你的NIS服务器。

3.2确认客户端可以看到服务器上的portmap服务

[root@clinent ~]# rpcinfo -p 192.168.0.254     探测服务器端启用RPC的服务

[root@server ~]# service ypbind start

3.3使用yppasswd在客户端更改用户密码

[user@clent ~]# yppasswd

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

注意事项：

1.为保证rpc正常通信，/etc/hosts中应该加入server/client的相关条目

server  /etc/hosts:

127.0.0.1  server.domain.net server

192.168.1.201  server.domain.net server

192.168.1.202  client.domain.net client

clients  /etc/hosts:

127.0.0.1 client.domain.net client

192.168.1.202 client.domain.net client

192.168.1.201 server.domain.net server

2.主机所在domain和nisdomain是可以不一样的

server   /etc/sysconfig/network

HOSTNAME=server.domain.net

NISDOMAIN=mynisdomain

client   /etc/sysconfig/network

HOSTNAME=clients.domain.net

NISDOMAIN=mynisdomain

3.注意查看server/clients  /etc/yp.conf中nisdomain的相关设置

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

客户端测试工具：

[root@linux ~]# yptest                             #利用 yptest 檢驗資料庫之測試

[root@linux ~]# ypwhich -x                         #利用 ypwhich 檢驗資料庫數量

[root@linux ~]# ypcat [-h nisserver] [資料庫名稱]  #利用 ypcat 讀取資料庫內容

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

#nis 的其他配置/etc/yp.conf

#nis 的其他配置/etc/ypserv.conf

dns: no

# NIS 伺服器的使用時機絕大部分都是在區域網路內而已，所以不需要 DNS 系統，

# 只要 /etc/hosts 設定正確即可！！

files: 30

# 預設會有 30 個資料庫被讀入記憶體當中，因為我們的帳號檔案其實不多，

# 30 個已經足夠使用了！

slp: no

slp_timeout: 3600

# 這兩個與 SLP 服務有關，因為我們僅使用單純的 NIS ，所以不需要啟動。

xfr_check_port: yes

# 這個與 master/slave 有關，將同步更新的資料庫比對所使用的埠口，

# 放置於小於 1024 的埠口內。

# 底下則是設定限制用戶端或 slave server 查詢的權限，利用冒號隔成四部分，分別為：

# [主機名稱/IP] : [NIS網域名稱] : [可用資料庫名稱] : [安全限制]

# [主機名稱/IP]   ：可以使用 network/netmask 如 192.168.1.0/255.255.255.0

# [NIS網域名稱]   ：例如本案例中的 vbirdnis

# [可用資料庫名稱]：就是由 NIS 製作出來的資料庫名稱；

# [安全限制]      ：包括沒有限制 (none)、僅能使用 &lt;1024 (port) 及拒絕 (deny)

# 一般來說，你可以依照我們的網域來設定成為底下的模樣：

127.0.0.0/255.255.255.0   : * : * : none

192.168.1.0/255.255.255.0 : * : * : none

*                         : * : * : deny

# 星號 (*) 代表任何資料都接受的意思。上面三行的意思是，開放 lo 內部介面、

# 開放內部 LAN 網域，且杜絕所有其他來源的 NIS 要求的意思。

# 萬一上面這三行權限相關的設定無法讓你的 NIS 順利的生效時，

# 你可以先將上面三行註解，然後加入底下這一行：

*                         : * : * : none

# 這樣應該就能夠讓 NIS 順利的連線了。

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++