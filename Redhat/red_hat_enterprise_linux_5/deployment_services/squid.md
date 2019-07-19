### squid {#squid}

squid

类型: 系统 V （System V）管理的服务

软件包: squid

守护进程: /usr/sbin/squid

脚本: /etc/init.d/squid

端口: 3128(squid), (可配置)

配置文件: /etc/squid/*

  Squid的基本配置

1\. 在您的系统上安装squid

2.开始服务（service squid start），然后配置您的浏览器使用您的localhost作为您的Proxy并且把端口设定为3128。

3.尝试访问一些主页（是访问你自己搭建的web服务）。

4\. 现在使用您的邻居来把您的主机当作Proxy。这样子应该不能工作。

squid返回的页面在/var/log/squid/access.log文件的底部有所解释。

5.使用您喜欢的文本编辑器打开/etc/squid/squid.conf文件。正如您所看到的，大部分是文档和注释。您也该注意到squid是非常易于调校的。对于本实验，我们仅作简单的配置，熟悉了以后您将会适应更加复杂的配置。

6.在文件中查找第二次出现Recommend minimum configuration的地方。您将会看到缺省的存取控制列表（acl）。在CONNECT method CONNECT 行的下面添加一个对于本地网络的存取访问列表项目：

acl example src 192.168.0.0/24

对于这个配置您可以把它作为参考以应用到其他的任何地方。src是该acl的源IP地址。7.在文件中查找INSERT YOUR RULE(S) HERE，在localhost acl的上面增加如下的内容：

http_access allow example

重新启动squid。 您的邻居将能够访问您的Web缓存了。

8.一些URL最好能够避免。返回到acl的部分，在您新添加行的下面（使用example.com如果您在教师里面没有Internet访问权限的话）

acl otherguys dstdomain .yahoo.comacl otherguys dstdomain .hotmail.com

这里有一些要提及的东西。首先，注意到acl的附加属性。第二注意到dstdomain的acl类型，指明了关心的目的域。第三、注意到在域名前的点表示符号，确保加上点。

9.增加一条拒绝访问规则应用到这些存在问题的域。返回到您刚在添加allow的地方，在其下面增加如下行：

http_access deny otherguys

再次重新启动squid，再次检查这些相关的域，非常不幸，访问没有被拒绝。

10.再次打开配置文件，将您添加的拒绝规则放在example的允许规则之前。也就是说，在otherguys拒绝规则之前的example允许规则使得访问被允许，但是拒绝没有被生效。在移动规则以后，重新启动squid。这回它将禁止访问在任何上面禁止访问的域内的站点了。

Squid主配置文件基本参数

主配置文件：/etc/squid/squid.conf

设置Squid监听的IP地址和端口号

http_port 3128

设置内存缓冲区的大小

cache_mem 64 MB

设置硬盘缓冲区的大小

cache_dir ufs /var/spool/squid 4096 16 256

本选项的意思：

硬盘缓冲区的目录为/var/spool/squid

缓冲区的存储类型类型为ufs

缓存空间为4096MB，硬盘缓冲区的目录下有16个

一级目录，每个一级目录下有256个二级子目录

设置使用缓存的有效用户

cache_effective_user squid

设置使用缓存的有效用户组

cache_effective_group squid

定义DNS服务器的地址

dns_nameservers 61.144.56.101

设置访问日志

cache_access_log /var/log/squid/access.log

设置缓存日志文件

cache_log /var/log/squid/cache.log

设置网页缓存日志文件的名称

cache_store_log /var/log/squid/store.log

设置运行Squid主机的名称

visible_hostname 192.168.16.1

设置管理员的E-mail地址

cache_mgr lindenstar@163.com

设置访问控制列表

acl all src 0.0.0.0/0.0.0.0（所有的IP地址）

允许或拒绝某个访问控制列表的HTTP请求

http_access allow all

范例：

Squid访问控制应用实例

一 禁止IP地址为192.168.16.200的客户机上网

Acl badclientip src 192.168.16.200

http_access deny badclientip

二禁止子网192.168.1.0的所有客户机上网

Acl badclientnet src 192.168.1.0/24

http_access deny badclientnet

三 禁止用户访问IP地址为210.21.116.68的网站

Acl badsrcip dst 210.21.118.68

http_access deny badsrcip

四 禁止用户访问域名为 www.163.com 的网站

Acl baddomain dstdomain www.163.com

http_access deny baddomain

五 禁止用户访问域名含有163.com的网站

Acl badurl url_regex -i 163.com

http_access deny badurl

六 禁止用户访问含有sex关键字的url

Acl badurl2 url_regex -i sex

http_access deny badurl2

七 禁止192.168.2.0子网内的所有客户机在周一到周五的九点到18点上网

Acl clientnet1 src 192.168.2.0/24

Acl worktime time MTWHF 9:00-18:00

http_access deny clientnet1 worktime

八 禁止客户机下载*.mp3 *.zip类型的文件

Acl badfile urlpath_regex -i \.mp3 \.zip$

http_access deny badfile

九 禁止QQ通过squid 代理上网

Acl qq method CONNECT &quot;tencent.com&quot;

http_access deny qq

squid原代码安装文档

下载squid-2.6.STABLE16.tar.gz

用命令 tar -zxvf  squid-2.6.STABLE16.tar.gz -C /usr/local/src

  cd  squid-2.6.STABLE16

  ./configure --prefix=/usr/local/squid  --enable-referer-log  --enable-useragent-log

Make ;make install

三，配置squid.conf

   1.vi /usr/local/squid/etc/squid.conf

 在相应的位置添加设置如下内容：

 http_port 3128

 cache_dir ufs /db/squid/var/cache 102400 16 256

 cache_mem 1536 MB（本机内存的2/3）

 cache_log /db/squid/var/logs/cache.log

 access_log /db/squid/var/logs/access.log squid(启动的时候改为none)

 cache_store_log none

 visible_hostname 主机名

 http_access allow all

2, mkdir /db/squid/var/logs/ -p

 Mkdir /db/squid/var/cache/ -p

 Chown 99.99 /db/squid/var/cache -R

     Chmod 755 /db/squid/var/logs -R

     Chown 99.99 /db/squid/var/logs -R

调试启动

/usr/local/squid/sbin/squid -zX (生成缓存)

/usr/local/squid/sbin/squid -DNCd1 （调试）

/usr/local/squid/sbin/squid -k parse(检查配置)

/usr/local/squid/sbin/squid -D （启动）