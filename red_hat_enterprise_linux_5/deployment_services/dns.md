### dns {#dns}

DNS

1\. 安装DNS所需的包

[root@node1 ~]# yum install bind bind-chroot caching-nameserver

bind

bind-chroot 包提供一个安全的DNS根环境

caching-nameserver

2.修改DNS主配置文件

[root@server ~]# vi /var/named/chroot/etc/named.caching-nameserver.conf   #该文件包含named.rfc1912.zones区域配置文件

##以下是主要内容(此时根环境为/var/named/chroot)：

options {

       listen-on port 53 { any; };

       listen-on-v6 port 53 { ::1; };

       directory       &quot;/var/named&quot;;

       dump-file       &quot;/var/named/data/cache_dump.db&quot;;

       statistics-file &quot;/var/named/data/named_stats.txt&quot;;

       memstatistics-file &quot;/var/named/data/named_mem_stats.txt&quot;;

       query-source    port 53;

       query-source-v6 port 53;

       allow-query     { any; };

};

logging {

       channel default_debug {

               file &quot;data/named.run&quot;;

               severity dynamic;

       };

};

view localhost_resolver {

       match-clients      { any; };

       match-destinations { any; };

       recursion yes;

       include &quot;/etc/named.rfc1912.zones&quot;;

};

3.修改区域配置文件

[root@server ~]# vi /var/named/chroot/etc/named.rfc1912.zones  #named.rfc1912.zones区域配置文件

##在该文件中添加区域

##添加domain.com域

zone &quot;domain.com&quot; IN {

       type master;

       file &quot;domain.com.zone&quot;;

       allow-update { none; };

};

##添加domain反向域

zone &quot;0.0.10.in-addr.arpa&quot; IN {

       type master;

       file &quot;0.0.10.rev&quot;;

       allow-update { none; };

};

4.domain.com域配置文件

[root@server ~]# vi /var/named/chroot/var/named/domain.com.zone

$TTL    86400

@               IN SOA  node.domain.com.    admin.domain.com. (

                                       42              ;    serial (d. adams)

                                       3H              ;   refresh

                                       15M             ;  retry

                                       1W              ;  expiry

                                       1D )            ;  minimum

domain.com.             IN NS           node.domain.com.

www                         IN A            10.0.0.10

ftp                            IN A            10.0.0.20

5.domain.com反向域配置文件

[root@server ~]# vi /var/named/chroot/var/named/0.0.10.rev

$TTL    86400

@       IN      SOA     domain.com.      root.domain.com.  (

                                     1997022700 ; Serial

                                     28800      ; Refresh

                                     14400      ; Retry

                                     3600000    ; Expire

                                     86400 )    ; Minimum

@              IN      NS     domain.com.

10              IN      PTR     www.domain.com .

20              IN      PTR     ftp.domain.com

注意：这个两个文件named组应该拥有可读权限

           #ln -s /var/named/chroot/...  /var/named/...

6.测试

[root@server named]# dig @localhost www.domain.com           ##@指定DNS服务器IP地址,本机可省略

[root@server named]# dig   www.domain.com

[root@server named]# dig @localhost -x 10.0.0.10                    ##解析反向域

[root@server named]# dig  -x 10.0.0.10

7./usr/sbin/nscd - name service cache daemon  

##########

vim named.conf

     include &quot;/etc/transfer.key&quot;;

　　　acl &quot;example&quot; { 127/8；192.168.0/24; };

  options {

listen-on port 53 { any; };

directory  &quot;/var/named&quot;;

allow-query     { any; };

forwarders { 8.8.8.8; };   #开启外网查询

allow-transfer { key stationX-stationY; };