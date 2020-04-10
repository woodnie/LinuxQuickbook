#### vsftpd {#vsftpd}

vsftp

服务侧写：

类型：系统V（System V）管理的服务

软件包：vsftpd

守护进程：/usr/sbin/vsftpd

脚本：/etc/init.d/vsftpd

端口：21（ftp）、20（ftp-data）

配置文件:/etc/vsftpd/vsftpd.conf、/etc/vsftpd/vsftpd.ftpusers、/etc/pam.d/vsftpd

日志：/var/log/xferlog

相关软件包：tcp_wrappers、ip_conntrack_ftp、ip_nat_ftp

1.安装包：

[root@localhost ~]#  rpm -ivh vsftpd-2.0.5-10.el5.i386.rpm

2.配置文件

[root@localhost Server]# rpm -qil vsftpd|grep etc

/usr/sbin/vsftpd        #守护进程

/etc/init.d/vsftpd      #启动脚本          

/etc/vsftpd/ftpuser     #禁止登陆ftp的用户

/etc/vsftpd/user_list   #根据相关设置，控制该表单中用户访问属性(默认禁止登录)

/etc/vsftpd/vsftpd.conf #vsftp主配置文件

/etc/pam.d/vsftpd

3.用户登录

3.1匿名用户默认可以登录

     [root@localhost ~]# lftp

3.2实名用户登录（默认禁止）

     [root@localhost ~]# ftp       #指定用户登录

     500 OOPS: chroot             #出现该错误

     Login failed.  

     [root@localhost ~]# tail /var/log/message  #看看报错内容      

     [root@localhost ~]#setsebool ftp_home_dir on         #一般情况下需要更改selinux bool值:ftp_home_dir on        

3.3匿名用户上传文件（默认禁止)  

     anon_upload_enable = YES      ##修改/etc/vsftp/vsftp.conf  中允许匿名用户上传权限

     setsebool  allow_ftpd_anon_write=1

     [root@localhost ~]# cd /var/ftp

     [root@localhost ~]# mkdir incoming

     [root@localhost ~]# chown root.ftp incoming

     [root@localhost ~]# chmod 775 incoming

4.实名用户锁定在自己家目录

chroot_local_user=yes

#chroot_list_enable=YES    #启用该项可在chroot_list文件中指定锁定用户

#chroot_list_file=/etc/vsftpd/chroot_list

5.更改上传/下载目录

下载文件到指定目录 / 从指定目录上传文件：

ftp&gt;lcd    指定目录

6.修改ftp的根目录

local_root=/var/www/html

anon_root=/var/www/html

注：local_root 针对系统用户；

       anon_root 针对匿名用户。

ftp登录格式

web: [ftp://username:passwd@host](ftp://username:passwd@host)

dos: ftp ip port

dos: ftp

dos: open ip port

7.vsftpd.conf中的所有配置信息

# 與主機較相關的設定值

   * connect_from_port_20=YES (NO)

     記得在前一小節提到的主動式連線使用的 FTP 伺服器的埠號嗎？這就是 ftp-data 的埠號；

   * listen_port=21

     vsftpd 使用的命令通道之埠號，如果您想要使用非正規的埠號，在這個設定項目修改吧！ 不過你必須要知道，這個設定值僅適合以 stand alone 的方式來啟動喔！(對於 super daemon 無效)

   * dirmessage_enable=YES (NO)

     當使用者進入某個目錄時，會顯示該目錄需要注意的內容，顯示的檔案預設是 .message ，你可以使用底下的設定項目來修訂！

   * message_file=.message

     當 dirmessage_enable=YES 時，可以設定這個項目來讓 vsftpd 尋找該檔案來顯示訊息！

   * listen=YES (NO)

     若設定為 YES 表示 vsftpd 是以 standalone 的方式來啟動的！

   * pasv_enable=YES (NO)

     啟動被動式連線模式(passive mode)，一定要設定為 YES 的啦！

   * use_localtime=YES (NO)

     是否使用本地時間？vsftpd 預設使用 GMT 時間(格林威治)，所以會比台灣晚 8 小時，建議設定為 YES 吧！

   * write_enable=YES (NO)

     如果你允許使用者上傳資料時，就要啟動這個設定值；

   * connect_timeout=60

     單位是秒，在資料連接的主動式連線模式下，我們發出的連接訊號在 60 秒內得不到用戶端的回應，則不等待並強制斷線咯。

   * accept_timeout=60

     當使用者以被動式 PASV 來進行資料傳輸時，如果主機啟用 passive port 並等待 client 超過 60 秒而無回應， 那麼就給他強制斷線！這個設定值與 connect_timeout 類似，不過一個是管理主動連線，一個管理被動連線。

   * data_connection_timeout=300

     如果伺服器與用戶端的資料連線已經成功建立 (不論主動還是被動連線)，但是可能由於線路問題導致 300 秒內還是無法順利的完成資料的傳送，那用戶端的連線就會被我們的 vsftpd 強制剔除！

   * idle_session_timeout=300

     如果使用者在 300 秒內都沒有命令動作，強制離線！

   * max_clients=0

     如果 vsftpd 是以 stand alone 方式啟動的，那麼這個設定項目可以設定同一時間，最多有多少 client 可以同時連上 vsftpd 哩！？

   * max_per_ip=0

     與上面 max_clients 類似，這裡是同一個 IP 同一時間可允許多少連線？

   * pasv_min_port=0, pasv_max_port=0

     上面兩個是與 passive mode 使用的 port number 有關，如果您想要使用 65400 到 65410 這 11 個 port 來進行被動式連線模式的連接，可以這樣設定 pasv_max_port=65410 以及 pasv_min_port=65400。 如果是 0 的話，表示隨機取用而不限制。

   * ftpd_banner=一些文字說明

     當使用者連線進入到 vsftpd 時，在 FTP 用戶端軟體上頭會顯示的說明文字。不過，這個設定值資料比較少啦！ 建議你可以使用底下的設定值來取代這個項目；

   * banner_file=/path/file

     這個項目可以指定某個純文字檔作為使用者登入 vsftpd 伺服器時所顯示的歡迎字眼。

# 與實體用戶較相關的設定值

   * guest_enable=YES (NO)

     若這個值設定為 YES 時，那麼任何非 anonymous 登入的帳號，均會被假設成為 guest (訪客) 喔！ 至於訪客在 vsftpd 當中，預設會取得 ftp 這個使用者的相關權限。但可以透過 guest_username 來修改。

   * guest_username=ftp

     在 guest_enable=YES 時才會生效，指定訪客的身份而已。

   * local_enable=YES (NO)

     這個設定值必須要為 YES 時，在 /etc/passwd 內的帳號才能以實體用戶的方式登入我們的 vsftpd 主機喔！

   * local_max_rate=0

     實體用戶的傳輸速度限制，單位為 bytes/second， 0 為不限制。

   * chroot_local_user=YES (NO)

     將使用者限制在自己的家目錄之內(chroot)！這個設定在 vsftpd 當中預設是 NO，因為有底下兩個設定項目的輔助喔！ 所以不需要啟動他！

   * chroot_list_enable=YES (NO)

     是否啟用將某些實體用戶限制在他們的家目錄內？預設是 NO ，不過，如果您想要讓某些使用者無法離開他們的家目錄時， 可以考慮將這個設定為 YES ，並且規劃下個設定值

   * chroot_list_file=/etc/vsftpd.chroot_list

     如果 chroot_list_enable=YES 那麼就可以設定這個項目了！ 他裡面可以規定那一個實體用戶會被限制在自己的家目錄內而無法離開！(chroot) 一行一個帳號即可！

   * userlist_enable=YES (NO)

     是否藉助 vsftpd 的抵擋機制來處理某些不受歡迎的帳號，與底下的設定有關；

   * userlist_deny=YES (NO)

     當 userlist_enable=YES 時才會生效的設定，若此設定值為 YES 時，則當使用者帳號被列入到某個檔案時， 在該檔案內的使用者將無法登入 vsftpd 伺服器！該檔案檔名與下列設定項目有關。

   * userlist_file=/etc/vsftpd.user_list

     若上面 userlist_deny=YES 時，則這個檔案就有用處了！在這個檔案內的帳號都無法使用 vsftpd 喔！

# 匿名者登入的設定值

   * anonymous_enable=YES (NO)

     設定為允許 anonymous 登入我們的 vsftpd 主機！預設是 YES ，底下的所有相關設定都需要將這個設定為 anonymous_enable=YES 之後才會生效！

   * anon_world_readable_only=YES (NO)

     僅允許 anonymous 具有下載可讀檔案的權限，預設是 YES。

   * anon_other_write_enable=YES (NO)

     是否允許 anonymous 具有寫入的權限？預設是 NO！如果要設定為 YES， 那麼開放給 anonymous 寫入的目錄亦需要調整權限，讓 vsftpd 的 PID 擁有者可以寫入才行！

   * anon_mkdir_write_enable=YES (NO)

     是否讓 anonymous 具有建立目錄的權限？預設值是 NO！如果要設定為 YES， 那麼 anony_other_write_enable 必須設定為 YES ！

   * anon_upload_enable=YES (NO)

     是否讓 anonymous 具有上傳資料的功能，預設是 NO，如果要設定為 YES ， 則 anon_other_write_enable=YES 必須設定。

   * deny_email_enable=YES (NO)

     將某些特殊的 email address 抵擋住，不讓那些 anonymous 登入！ 如果以 anonymous 登入主機時，不是會要求輸入密碼嗎？密碼不是要您 輸入您的 email address 嗎？如果你很討厭某些 email address ， 就可以使用這個設定來將他取消登入的權限！需與下個設定項目配合：

   * banned_email_file=/etc/vsftpd.banned_emails

     如果 deny_email_enable=YES 時，可以利用這個設定項目來規定哪個 email address 不可登入我們的 vsftpd 喔！在上面設定的檔案內，一行輸入一個 email address 即可！

   * no_anon_password=YES (NO)

     當設定為 YES 時，表示 anonymous 將會略過密碼檢驗步驟，而直接進入 vsftpd 伺服器內喔！所以一般預設都是 NO 的！

   * anon_max_rate=0

     這個設定值後面接的數值單位為 bytes/秒 ，限制 anonymous 的傳輸速度，如果是 0 則不限制(由最大頻寬所限制)，如果您想讓 anonymous 僅有 30 KB/s 的速度，可以設定『anon_max_rate=30000』

   * anon_umask=077

     限制 anonymous 的權限！如果是 077 則 anonymous 傳送過來的檔案 權限會是 -rw------- 喔！

# 關於系統安全方面的一些設定值

   * ascii_download_enable=YES (NO)

     如果設定為 YES ，那麼 client 就可以使用 ASCII 格式下載檔案。

   * ascii_upload_enable=YES (NO)

     與上一個設定類似的，只是這個設定針對上傳而言！預設是 NO

   * one_process_model=YES (NO)

     這個設定項目比較危險一點～當設定為 YES 時，表示每個建立的連線 都會擁有一支 process 在負責，可以增加 vsftpd 的效能。不過， 除非您的系統比較安全，而且硬體配備比較高，否則容易耗盡系統資源喔！一般建議設定為 NO 的啦！

   * tcp_wrappers=YES (NO)

     當然我們都習慣支援 TCP Wrappers 的啦！所以設定為 YES 吧！

   * xferlog_enable=YES (NO)

     當設定為 YES 時，使用者上傳與下載檔案都會被紀錄起來。記錄的檔案與下一個設定項目有關：

   * xferlog_file=/var/log/vsftpd.log

     如果上一個 xferlog_enable=YES 的話，這裡就可以設定了！這個是登錄檔的檔名啦！

   * xferlog_std_format=YES (NO)

     是否設定為 wu ftp 相同的登錄檔格式？！預設為 NO ，因為登錄檔會比較容易讀！ 不過，如果您有使用 wu ftp 登錄檔的分析軟體，這裡才需要設定為 YES

   * nopriv_user=nobody

     我們的 vsftpd 預設以 nobody 作為此一服務執行者的權限。因為 nobody 的權限 相當的低，因此即使被入侵，入侵者僅能取得 nobody 的權限喔！

   * pam_service_name=vsftpd

     這個是 pam 模組的名稱，我們放置在 /etc/pam.d/vsftpd 即是這個咚咚！

++++++++++++++++++++++++

1\. 安装vsftpd（以rpm包方式）

2.修改/etc/vsftpd/vsftpd.conf  下的linsten=YES 改为listen=NO

3\. 查看vsftpd安装包

5.cp /usr/share/doc/vsftpd-2.0.1/vsftpd.xinetd   /etc/xinetd.d/vsftpd

6.vi  /etc/xinetd.d/vsftpd

# default: off

# description: The vsftpd FTP server serves FTP connections. It uses \

#   normal, unencrypted usernames and passwords for authentication.

service ftp

{

   socket_type       = stream

   wait                  = no

   user                  = root

   server            = /usr/sbin/vsftpd

   server_args        = /etc/vsftpd/vsftpd.conf

   nice                  = 10

   disable           = no

   flags             = IPv4

   access_times       = 13:00-17:00    ;限制访问时间

only_from          = 10.0.0.0       ；限制访问地址

no_access          = 10.10.10.10    ;拒绝访问

}Netstat -ln | grep  21  端口查询

更多选项可以man  /etc/xinetd.conf

7.重新启动xinetd服务  service xinetd restart

8\. 查看ftp ：netstat  -an |grep  : 21

++++++++++++++++++++++++

ftp troubleshooting

#iptables -A INPUT  -p tcp/udp --sport 21 -j ACCEPT

#iptables -A INPUT  -p tcp/udp --sport 20 -j ACCEPT

#iptables -P INPUT DROP

ftp 链接出现延时：

方法1.关闭被动模式

ftp&gt; passive

passive off  #关闭被动模式

方法2.iptables开放已经建立的连接，和其子连接

#iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

ftp相关命令：

ftp&gt; binary  #以二进制传输

ftp&gt; hash 1024  #以1024bytes传输数据