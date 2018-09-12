### nslookup {#nslookup}

bind-utils 包提供查询名称服务器的工具

host       #简单的逆向和反向查询

nslookup

dig

++++++++++++++++++++++++++++++++

　nslookup -qt=类型 目标域名

　　注意qt必须小写。

　　类型可以是一下字符，不区分大小写：

　　A 地址记录(Ipv4)

　　AAAA 地址记录（Ipv6）

　　AFSDB Andrew文件系统数据库服务器记录（不懂）

　　ATMA ATM地址记录（不是自动提款机）

　　CNAME 别名记录

　　HINFO 硬件配置记录，包括CPU、操作系统信息

　　ISDN 域名对应的ISDN号码

　　MB 存放指定邮箱的服务器

　　MG 邮件组记录

　　MINFO 邮件组和邮箱的信息记录

　　MR 改名的邮箱记录

　　MX 邮件服务器记录

　　NS 名字服务器记录

　　PTR 反向记录（从IP地址解释域名）

　　RP 负责人记录

　　RT 路由穿透记录（不懂）

　　SRV TCP服务器信息记录（将有大用处）

　　TXT 域名对应的文本信息

　　X25 域名对应的X.25地址记录