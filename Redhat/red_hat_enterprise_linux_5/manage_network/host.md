### host {#host}

host - DNS lookup utility

bind-utils 包提供查询名称服务器的工具

host       #简单的逆向和反向查询

nslookup

dig

[root@node ~]# host 8.8.8.8

8.8.8.8.in-addr.arpa domain name pointer google-public-dns-a.google.com.

[root@node ~]# host google-public-dns-a.google.com

google-public-dns-a.google.com has address 8.8.8.8

[root@node ~]# host -t A g.cn

g.cn has address 203.208.37.99

g.cn has address 203.208.37.104

[root@node ~]# host -t NS g.cn

g.cn name server ns1.google.cn.

g.cn name server ns4.google.com.

g.cn name server ns2.google.com.

g.cn name server ns1.google.com.

g.cn name server ns3.google.com.

[root@node ~]# host -t SOA g.cn

g.cn has SOA record ns1.google.com. dns-admin.google.com. 1435338 21600 3600 1209600 300

[root@node ~]# host -t MX g.cn  

g.cn mail is handled by 10 google.com.s9a1.psmtp.com.

g.cn mail is handled by 10 google.com.s9b1.psmtp.com.

g.cn mail is handled by 10 google.com.s9a2.psmtp.com.

g.cn mail is handled by 10 google.com.s9b2.psmtp.com.

[root@node1 ~]#  dig  -ixfr g.cn