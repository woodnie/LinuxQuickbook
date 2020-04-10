#### apache {#apache}

conf apache

类型: 系统 V （System V）管理服务

软件包: httpd, httpd-devel, httpd-manual

守护进程: /usr/sbin/httpd

脚本: /etc/init.d/httpd

端口: 80(http)、443(https)

配置: /etc/httpd/*, /var/www/*

相关软件包: system-config-httpd, mod_ssl

[http://httpd.apache.org/docs/2.2/zh-cn/](http://httpd.apache.org/docs/2.2/zh-cn/)

Apache 名称域配置

1.指定一个用于用户页的目录:

  UserDir public_html

2.MIME 类型配置:

  AddType application/x-httpd-php .phtml

  AddType text/html .htm

3.指出用于目录的索引文件:

  DirectoryIndex index.html default.htm

Apache 访问配置

&lt;Directory /var/www/html&gt;

 order allow,deny

 allow from  允许的主机列表

 deny from  拒绝的主机列表

&lt;/Directory&gt;

主机说明文件应该包括：点式数字地址、网络 / 子网掩码、主机和域

order allow,deny 允许明确指定的allow客户，拒绝其他所有客户；同时匹配allow和deny,则被拒绝

order deny,allow 拒绝明确指定的deny客户，允许其他所有客户；同时匹配allow和deny,则被允许

VirtualHost

NameVirtualHost 192.168.1.60:80

&lt;VirtualHost 192.168.1.60:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.60.com/html

   ServerName www.60.com

   ErrorLog logs/www.60.com-error_log

   ScriptAlias /cgi-bin/ /var/www/virtualhost/www.60.com/html/cgi-bin/

   CustomLog logs/www.60.com-access_log common

&lt;/VirtualHost&gt;

&lt;VirtualHost 192.168.1.60:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.60.org/html

   ServerName www.60.org

   ErrorLog logs/www.60.org-error_log

   CustomLog logs/www.60.org-access_log common

&lt;/VirtualHost&gt;

NameVirtualHost 192.168.1.61:80

&lt;VirtualHost 192.168.1.61:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.61.com/html

   ServerName www.61.com

   ErrorLog logs/www.61.com-error_log

   CustomLog logs/www.61.com-access_log common

&lt;/VirtualHost&gt;

&lt;VirtualHost 192.168.1.61:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.61.org/html

   ServerName www.61.org

   ErrorLog logs/www.61.org-error_log

   CustomLog logs/www.61.org-access_log common

&lt;/VirtualHost&gt;

NameVirtualHost 192.168.1.62:80

&lt;VirtualHost 192.168.1.62:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.62.com/html

   ServerName www.62.com

   ErrorLog logs/www.62.com-error_log

   CustomLog logs/www.62.com-access_log common

&lt;/VirtualHost&gt;

&lt;VirtualHost 192.168.1.62:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.62.org/html

   ServerName www.62.org

   ErrorLog logs/www.62.org-error_log

   CustomLog logs/www.62.org-access_log common

&lt;/VirtualHost&gt;

CGI(Common Gate Interface)

&lt;VirtualHost 192.168.1.60:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.60.com/html

   ServerName www.60.com

   ErrorLog logs/www.60.com-error_log

   ScriptAlias /cgi-bin/ /var/www/virtualhost/www.60.com/html/cgi-bin/

   CustomLog logs/www.60.com-access_log common

&lt;/VirtualHost&gt;

[root@node6 ~]# vi /var/www/virtualhost/www.60.com/html/cgi-bin/test.sh

#!/bin/bash

echo Content-Type: text/html;

echo

echo &quot;&lt;pre/&gt;&quot;

echo My username is:whoami

echo

echo My id is:id

echo

echo My shell setting are:set

echo

echo My environment variable are:env

echo

echo Here is /etc/passwd

cat /etc/passwd

echo Here are the files in /tmp/:

ls /tmp/*

echo &quot;&lt;/pre&gt;&quot;

chmod o+x /var/www/virtualhost/www.60.com/html/cgi-bin/test.sh

.htaccess

[root@node6 ~]# vi /var/www/virtualhost/www.60.com/html/.htaccess

Authname &quot;restricted stuff&quot;&quot;

AuthType Basic

AuthUserFile /etc/httpd/conf/.htpasswd.www.60.com

require valid-user

[root@node6 ~]# touch /etc/httpd/conf/.htpasswd.www.60.com

[root@node6 ~]# cd /etc/httpd/conf

[root@node6 conf]# htpasswd -mc .htpasswd.www.60.com wood

New password:

Re-type new password:

Adding password for user wood

[root@node6 ~]# chown :apache .htpasswd.www.60.com wood

[root@node6 ~]# chmod 640 .htpasswd.www.60.com wood

[root@node6 ~]# /etc/init.d/httpd reload

添加httpd.conf

&lt;VirtualHost 192.168.1.60:80&gt;

   ServerAdmin wood_nie@163.com

   DocumentRoot /var/www/virtualhost/www.60.com/html

   ServerName www.60.com

       &lt;Directory /var/www/virtualhost/www.60.com/html&gt;

        AllowOverride AuthConfig

       &lt;/Directory&gt;

   ErrorLog logs/www.60.com-error_log

   CustomLog logs/www.60.com-access_log common

&lt;/VirtualHost&gt;

++++++++++++++++++++++++

#  .htaccess高级用法示例

Authname &quot;restricted stuff&quot;&quot;

AuthType Basic

AuthUserFile   /var/www/html/.htpasswd

AuthGroupFile /var/www/html/.htgroup

&lt;Limit GET&gt;

require group staff

&lt;/Limit&gt;

&lt;Limit PUT POST&gt;

require user bob

&lt;/Limit&gt;

+++++++++++++++++++++++++

SSL:

++++++++++++++++++++++++++++++++++++++++++++++++++++++++

加强Apache Web服务器的安全

安装mod_security，这个Apache模块起到了应用防火墙的作用。它可以过滤Web请求的所有部分，并终结恶意代码。它在Web服务器进行任何实际处理之前发挥作用，因而与Web应用程序无关。Mod_security适用于过滤从SQL注入到 XSS攻击的任何恶意流量。它也是保护易受攻击的Web应用程序的最快速、最容易的方法。该软件有众多随时可以使用的规则，但你也很容易自行编写规则。比如设想一下：你有一个过时的Joomla版本，担心会遭到SQL注入攻击。这个简单规则可以过滤含有jos_ （Joomla表的默认前缀）: SecFilter &quot;jos_&quot;的任何POST和GET内容。

安装mod_evasive，这是另一个重要的Apache模块，可以保护Web应用程序远离拒绝服务（DOS）请求。它的效果受制于这个现实：它是在应用程序层面工作的，这意味着Apache无论如何都接受连接，因而耗用带宽和系统资源。不过，如果是源自少量远程主机的比较弱的DOS攻击，该模块能有所帮助。一旦mod_evasive装入，你需要这样来配置它：

DOSPageCount 2  DOSSiteCount 30  DOSBlockingPeriod 120

这指示服务器阻止（默认情况下返回HTTP error 403 Forbidden）两次访问同一页面，或者在一秒内（默认的时间间隔）共有30次请求的任何主机。入侵者会在外面被阻挡120秒。

过滤访客的IP地址。这可能被认为是很极端的措施，但结果很好。首先，考虑安装mod_httpbl，这是用 Apache实现的Project Honeypot。一旦该模块安装并被启用，它可以阻止有恶意活动&quot;前科&quot;的IP地址。另一个办法是使用mod_geoip，它可以用来允许只有来自某些国家的访客才可以访问接受留言、注册和登录信息等输入内容的页面。它甚至可以阻止和允许来自某些国家的服务器端访客。

++++++

Apache支持jsp:

apache+tomcat+mod_jserv

apache+tomcat+mod_jk

apache+resin

+++++++++++++++

TraceEnable Off&quot;

++++++++++

&lt;Limit POST PUT DELETE&gt;

Require valid-user

&lt;/Limit&gt;

&lt;LimitExcept POST GET&gt;

Require valid-user

&lt;/LimitExcept&gt;

++++++++++++++++++++++++++++++

Options 指令

Indexes

如果一个映射到目录的URL被请求，而此目录中又没有DirectoryIndex(例如：index.html)，那么服务器会返回由mod_autoindex生成的一个格式化后的目录列表。

httpd.conf

httpd.conf 中文版(配置文件注解)  httpd.conf 中文版

# 基于 NCSA 服务器的配置文件 由 Rob McCool 编写!

#

# Apache服务器主配置文件. 包括服务器指令的目录设置.

# 详见 &lt;&gt;

#

# 请在理解用途的基础上阅读各指令

#ujie

# 再读取此文档后，服务器将继续搜索运行

# E:/Program Files/Apache Group/Apache/conf/srm.conf

# E:/Program Files/Apache Group/Apache/conf/access.conf

# 除非用ResourceConfig或AccessConfig覆盖这儿的标识

#

# 配置标识由三个基本部分组成:

# 1\. 作为一个整体来控制Apache服务器进程的标识 (the global environment).

# 2\. 用于定义主（默认）服务器参数的标识

# 响应虚拟主机不能处理的请求

# 同时也提供所有虚拟主机的设置值

# 3\. 虚拟主机的设置在一个Apache服务器进程中配置不同的IP地址和主机名

#

# 配置和日志文件名：指定服务器控制文件命名时，

# 以 &quot;/&quot; (或 &quot;drive:/&quot; for Win32)开始，服务器将使用这些绝对路径

# 如果文件名不是以&quot;/&quot;开始的，预先考虑服务器根目录--

# 因此 &quot;logs/foo.log&quot;，如果服务器根目录是&quot;/usr/local/apache&quot;，

# 服务器将解释为 &quot;/usr/local/apache/logs/foo.log&quot;.

#

# 注: 指定的文件名需要用&quot;/&quot;代替&quot;\&quot;

# (例, &quot;c:/apache&quot; 代替 &quot;c:\apache&quot;).

# 如果省略了驱动器名，默认使用Apache.exe所在的驱动器盘符

# 建议指定盘符，以免混乱

#

### 部分 1: 全局环境

#

# 本部分的表示将影响所有Apache的操作

# 例如，所能处理的并发请求数或配置文件地址

#

#

# ServerType 可取值 inetd 或 standalone. Inetd 只适用于Unix平台

#

ServerType standalone

#

# ServerRoot: 目录树的根结点服务器配置出错信息日志文件都保存在根目录下

#

# 不要再目录末尾加&quot;/&quot;

#

ServerRoot &quot;C:/Program Files/Apache Group/Apache&quot;

#

# PidFile: 服务器用于记录启动时进程ID的文件

#

PidFile logs/httpd.pid

#

# ScoreBoardFile: 用于保存内部服务器进程信息的文件

# 并非必须 但是如果指定了（此文件当运行Apache时生成）

# 那么必须确保没有两个Apache进程共享同一个scoreboard文件

#

ScoreBoardFile logs/apache_runtime_status

#

# 在标准配置下，服务器将顺序读取 httpd.conf(此文件可通过命令行中-f参数指定），

# srm.conf 和 access.conf

# 目前后两个文件是空的为了简单起见，建议将所有的标识放在一个文件中

# 以下两条注释的标识，是默认设置

# 要让服务器忽略这些文件可以用 &quot;/dev/null&quot; (for Unix)

# 或&quot;nul&quot; (for Win32) 作为参数

#

#ResourceConfig conf/srm.conf

#AccessConfig conf/access.conf

#

# Timeout: 接受和发送timeout的时间

#

Timeout 300

#

# KeepAlive: 是否允许保持连接（每个连接有多个请求）

# &quot;Off&quot; -无效

#

KeepAlive On

#

# MaxKeepAliveRequests: 每个连接的最大请求数

# 设置为0表示无限制

# 建议设置较高的值，以获得最好的性能

#

MaxKeepAliveRequests 100

#

# KeepAliveTimeout: 同一连接同一客户端两个请求之间的等待时间

#

KeepAliveTimeout 15

#

# 在Win32下,Apache每次产生一个子进程来处理请求

# 如果这个进程死了，会自动产生另一个子进程

# 所有的进入请求在子进程中多线程处理

# 以下两个标识控制进程的运行

#

#

# MaxRequestsPerChild: 每个子进程死亡之前最大请求数

# 如果超过这个请求数，子程序会自动退出，避免延期使用导致内存溢出或其他问题

# 大部分系统，并不需要此设置，

# 但是部分，象Solaris，确实值得注意

# 对Win32, 可设置为0 (无限制)

# 除非有另外的考虑

#

# 注: 此值不包括在每个连接初始化请求后，&quot;keptalive&quot;请求

# 例如, 如果一个子进程处理一个初始化请求和10个后续&quot;keptalive&quot;请求，

# 在这个限制下，只会记为一个请求

#

MaxRequestsPerChild 0

#

# ThreadsPerChild: 服务器所允许的并发线程数

# 此值的设置取决于服务器的响应能力（约多的请求在同一时间激活，则每个请求的处理时间越慢）

# 和服务器所允许消耗的系统资源

#

ThreadsPerChild 50

#

# Listen: 允许将Apache绑顶到指定的IP地址和端口，作为默认值的辅助选项

# 参见

#

#Listen 3000

#Listen 12.34.56.78:80

#

# BindAddress: 通过此选项可支持虚拟主机

# 此标识用于告诉服务器监听哪个IP地址

# 包括：&quot;*&quot;, IP地址, 或域名.

# 参见 和 Listen directives.

#

BindAddress 166.111.178.144

#

# Apache模块编译成标准的Windows结构

#

# 以下模块绑定到标准的Apache二进制windows分布

# 要修改标准操作，取消以下行的注释并且修改指定模块列表

#

# 警告：这是高级选项可能导致服务器崩溃

# 没有专家的指导，不要轻易修改

#

#ClearModuleList

#AddModule mod_so.c mod_mime.c mod_access.c mod_auth.c mod_negotiation.c

#AddModule mod_include.c mod_autoindex.c mod_dir.c mod_cgi.c mod_userdir.c

#AddModule mod_alias.c mod_env.c mod_log_config.c mod_asis.c mod_imap.c

#AddModule mod_actions.c mod_setenvif.c mod_isapi.c

#

# 动态共享对象（Dynamic Shared Object，DSO）

#

# 要使用基于DSO的功能模块，需要替换此处相应的

# `LoadModule 行这样在使用之前这些包含的标识都将生效

# 有关DSO及至的详细资料请看Apache1.3版中的README.DSOSO

# 运行&quot;apche -l&quot;将列表显示Apache内奸的模块（类似标准的连接已经生效）

#

# 注：模块载入的顺序很重要没有专家的建议，不要修改以下的顺序

#

#LoadModule anon_auth_module modules/ApacheModuleAuthAnon.dll

#LoadModule dbm_auth_module modules/ApacheModuleAuthDBM.dll

#LoadModule digest_auth_module modules/ApacheModuleAuthDigest.dll

#LoadModule cern_meta_module modules/ApacheModuleCERNMeta.dll

#LoadModule digest_module modules/ApacheModuleDigest.dll

#LoadModule expires_module modules/ApacheModuleExpires.dll

#LoadModule headers_module modules/ApacheModuleHeaders.dll

#LoadModule proxy_module modules/ApacheModuleProxy.dll

#LoadModule rewrite_module modules/ApacheModuleRewrite.dll

#LoadModule speling_module modules/ApacheModuleSpeling.dll

#LoadModule info_module modules/ApacheModuleInfo.dll

#LoadModule status_module modules/ApacheModuleStatus.dll

#LoadModule usertrack_module modules/ApacheModuleUserTrack.dll

#

# ExtendedStatus 在服务器状态句柄被呼叫时控制是产生完整的状态信息(ExtendedStatus On)

# 还是仅返回基本信息(ExtendedStatus Off)

# 默认是：Off

#

#ExtendedStatus On

### 部分 2: 主服务器配置

#

# 此部分的标识用于主服务器所有的设置值，

# 响应任何定义不处理的请求

# 这些值同时给你稍后在此文件中定义的提供默认值

#

# 所有的标识可能会在中出现

# 对应的默认值会被虚拟主机重新定义覆盖

#

#

# Port: Standalone服务器监听的端口

# 在Apache能够监听指定端口前，需要在防火墙中进行设置

# 其它运行httpd的服务器也可能影响此端口 Disable

# 如果遇到问题，请关闭所有的防火墙安全保护和其他的服务

# Windos NT的&quot;NETSTAT -a&quot;指令会有助于问题的分析

#

Port 80

#

# ServerAdmin: 你的地址如果服务器有任何问题将发信到这个地址

# 这个地址会在服务器产生的某些页面中出现，例如，错误报告

#

ServerAdmin

#

# ServerName 允许设置主机名如果与程序获得的不同，主机名将返回客户端

# （例如，用&quot;www&quot;代替主机真实的名字）

#

# 注: 主机名不能随便指定必须是你的机器有效的DNS名称否则无法正常工作

# 如果不能理解，倾向你的网络管理员询问

# 如果你的主机没有注册DNS名，可在此输入IP地址

# 此时必须用IP地址来访问(如, )

# 这样扔可以完成重新定向的工作

#

# 127.0.0.1 是TCP/IP的本地环路地址, 通常命名为localhost.

# 机器默认此地置为本身 如果只是使用Apache来进行本地测试和开发，

# 可使用127.0.0.1 作为服务器名.

#

#ServerName new.host.name

#

# DocumentRoot: 放置服务文档的目录

# 默认状态下，所有的请求都以这个目录为基础

# 但是直接符号连接和别名可用于指向其他位置

#

DocumentRoot &quot;D:/www_root&quot;

#

# Apache访问的每个目录可设置相关的服务和特性是允许或（和）不允许

# (同样影响其子目录）

#

# 首先，设置&quot;default&quot;地址只有最基本的权限

#

Options FollowSymLinks

AllowOverride None

#

# 注意从现在开始必须制定开启特殊的权限

# 这样就不会产生意想不到的结果

# 请仔细确认

#

#

# 这个地址应与DocumentRoot保持一致

#

#

# 此值可是： &quot;None&quot;, &quot;All&quot;, 或下列的组合： &quot;Indexes&quot;,

# &quot;Includes&quot;, &quot;FollowSymLinks&quot;, &quot;ExecCGI&quot;, 或 &quot;MultiViews&quot;.

#

# 注意&quot;MultiViews&quot;必须明确指定--- &quot;Options All&quot;不包括此特性

#

Options Indexes FollowSymLinks MultiViews

#

# 此项控制目录中哪些.htaccess文件可覆盖

# 允许值： &quot;All&quot;或者以下项的组合：&quot;Options&quot;, &quot;FileInfo&quot;,

# &quot;AuthConfig&quot;, &quot;Limit&quot;

#

AllowOverride None

#

# 控制哪些用户可从此服务器获得资料

#

Order allow,deny

Allow from all

#

# UserDir: 当请求~user时，追加到用户主目录的路径地址

#

# 在Win32下，并不要求指定为用户登陆的主目录

# 因此可使用以下的格式

# 详细参照文档UserDir

#

UserDir &quot;f:/homepages/&quot;

#

# 控制访问UserDir目录. The following is an example

# 以下是一个站点的例子，权限限制为只读

#

#

# AllowOverride FileInfo AuthConfig Limit

# Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec

#  

# Order allow,deny

# Allow from all

#

#  

# Order deny,allow

# Deny from all

#

#

#

# DirectoryIndex: 预设的HTML目录索引文件名

# 用空格来分隔多个文件名

#

DirectoryIndex index.html

#

# AccessFileName: 每个目录中用于控制访问信息的文件名

#

AccessFileName .htaccess

#

# 以下行防止客户端可访问 .htaccess 文件

# 因为 .htaccess文件通常包含授权信息，

# 处于安全的考虑不允许访问

# 如果想让访客看到.htaccess文件的内容，

# 可将这些行注释

# 如果修改了上面的AccessFileName,

# 请在此进行相应的修改

#

# 同时，一般会用类似.htpasswd的文件保存密码

# 这些文件同样可以得到保护

#

Order allow,deny

Deny from all

#

# CacheNegotiatedDocs: 默认下，Apache对每个文档发送&quot;Pragma: no-cache&quot;

# 这将要求代理服务器不缓存此文档

# 取消下列行的可取消这个属性，这样代理服务器将缓存这些文档

#

#CacheNegotiatedDocs

#

# UseCanonicalName: (1.3新增) 当此设置为on时，

# 无论何时Apache需要构建一个自引用的URL(指向响应来源服务器），

# 它将用ServerName和Port来构建一个规范的格式

# 当此设置为off时，Apache将使用客户端提供的&quot;主机名：端口&quot;

# 这将同时影响CGI脚本中的SERVER_NAME和SERVER_PORT

#

UseCanonicalName On

#

# TypesConfig 记录媒体类型（mime.types）文件或类似的东东放置的位置

#

TypesConfig conf/mime.types

#

# DefaultType 是服务器处理未确认类型的文件，如为止的扩展名，的默认类型

# 如果你的服务器上主要包含的是文本或HTML文档，&quot;text/plain&quot;是较好的设置

# 如果服务器上主要包含二进制文件，如应用程序或图片，

# 最好设置成&quot;application/octet-stream&quot;防止浏览器将二进制文件以文本的方式显示

#

DefaultType text/plain

#

# mod_mime_magic模块允许服务器使用文件自身的不同标识来确定文件类型

# MIMEMagicFile指示模块文件标识的定义所在的位置

# mod_mime_magic不是默认服务器的一部分

# (必须自行用LoadModule来追加 [见全局环境部分的 DSO 章节],

# 或者在编译服务器时包含mod_mime_magic部分）

# 包含在 中.

# 就是说，如果该模块是服务器的一部分，MIMEMagicFile标识将执行

#

MIMEMagicFile conf/magic

#

# HostnameLookups: 注册客户端的机器名或IP地址

# 例如：  (on) 或 204.62.129.132 (off).

# 默认为off，因为对于网络来说，最好让人们有意识的设置为on，

# 因为开启此功能意味着每个客户请求将导致至少向name服务器发送一个lookup请求

#

HostnameLookups Off

#

# ErrorLog: 错误记录文件的地址

# 如果不在内指定ErrorLog

# 改虚拟主机的错误心细将记录到此处

# 如果在中明确指定了错误记录文件，

# 则错误将记录在那儿而不是这儿

#

ErrorLog logs/error.log

#

# LogLevel: 控制记录在error.log中信息的个数.

# 可能的值：debug, info, notice, warn, error, crit,

# alert, emerg.

#

LogLevel warn

#

# 以下标识定义CustomLog标识使用的格式（见下）

#

LogFormat &quot;%h %l %u %t \&quot;%r\&quot; %&gt;s %b \&quot;%{Referer}i\&quot; \&quot;%{User-Agent}i\&quot;&quot; combined

LogFormat &quot;%h %l %u %t \&quot;%r\&quot; %&gt;s %b&quot; common

LogFormat &quot;%{Referer}i -&gt; %U&quot; referer

LogFormat &quot;%{User-agent}i&quot; agent

#

# 访问记录的位置和格式 (功用的记录文件格式).

# 如果不在中定义记录文件，

# 那些访问记录就将保存在这儿 Contrariwise, if you *do*

# 反之，如果指定了记录文件，那么访问记录将记录在那儿而不是这个文件中

#

CustomLog logs/access.log common

#

# 如果希望使用代理和参考的记录文件, 取消以下标识的注释符

#

#CustomLog logs/referer.log referer

#CustomLog logs/agent.log agent

#

# 如果想在一个文件中记录访问代理参考信息（复合的记录格式）

# 可使用以下标识

#

#CustomLog logs/access.log combined

#

# 在服务器产生的页面（如错误文档信息，FTP目录列表等等，不包括CGI产生的文档）中

# 增加一条服务器版本和虚拟主机名的信息

# 设置为&quot;EMail&quot;将包含mailto: ServerAdmin的连接.

# 可选值: On | Off | EMail

#

ServerSignature On

#

# 默认下，Apache用工作行解析所有CGI脚本

# 此注释行（脚本的第一行）包括#和!后面跟着执行特殊脚本的程序路径，

# 对perl脚本来说是C:\Program Files\Perl目录中的perl.exe

# 工作行如下：

#!c:/program files/perl/perl

# 注意真实的工作行不能有缩进，必须是文件的第一行

# 当然，CGI进程必须通过适当的ScriptAlias或ExecCGI选项标识来启动

#

# 然而，Windows下的Apache即允许以上的Unix方式，也可以通过注册表的形式

# 用注册表执行文件的方法同在Windows资源管理器中双击运行的注册方法相同

# 此脚本操作可在Windows资源管理器的查看菜单中设置

# 文件夹选项，然后查看文件类型点击编辑按钮

# 修改操作属性Apache 1.3会尝试执行Open操作，

# 如果失败则会尝试工作行

# 这个属性在Apache release 2.0中会有改变.

#

# 每个机制都有自身特定的安全弱点，这样可能导致别人运行你不希望调用的程序

# 最佳的解决方案还在讨论中

#

# 要是这个Windows的特殊属性生效 (同时会是Unix属性无效）

# 取消下列标识的注释符

#

#ScriptInterpreterSource registry

#

# 上面的标识可在块或.htaccess文件中单独替换

# 可选择registry (Windows behavior)或 script

# (Unix behavior) option, 将覆盖服务器的默认值

#

#

# Aliases: 可无限制的追加别名格式如下：

# Alias 假名 真名

#

#

# 注意如果假名中包含/，服务器会在当前URL中发出请求

# 因此&quot;/icons&quot;不能用于别名

# 必须用 &quot;/icons/&quot;..

#

Alias /icons/ &quot;C:/Program Files/Apache Group/Apache/icons/&quot;

Options Indexes MultiViews

AllowOverride None

Order allow,deny

Allow from all

#

# ScriptAlias: 控制哪个目录包含服务器脚本

# ScriptAlias本质行和Aliases一样, except that

# 区别在于真名目录中的文档被看作是一个应用程序

# 请求时由服务器运行而不是发往客户端

# &quot;/&quot;符号的规则同

# Alias相同.

#

ScriptAlias /cgi-bin/ &quot;C:/Program Files/Apache Group/Apache/cgi-bin/&quot;

#

# &quot;C:/Program Files/Apache Group/Apache/cgi-bin&quot; 可修改为任何放置CGI脚本的目录

#

AllowOverride None

Options None

Order allow,deny

Allow from all

# 别名结束

#php脚本说明

ScriptAlias /php/ &quot;d:/php/&quot;

AddType application/x-httpd-php .php

AddType application/x-httpd-php .php3

AddType application/x-httpd-php .phtml

Action application/x-httpd-php &quot;/php/php.exe&quot;

#php脚本说明结束

#

# Redirect 允许告诉客户端服务器上曾经有的文档，但是现在不存在了

# 并且可以告诉客户端到哪儿去寻找

# 格式: Redirect old-URL new-URL

#

#

# 控制服务器目录列表显示的标识

#

#

# FancyIndexing标识是使用特定的目录检索还是标准的（standard）

#

IndexOptions FancyIndexing

#

# AddIcon*表明不同文件或扩展名显示的图标

# 这些图标只在特定检索状态下显示

#

AddIconByEncoding (CMP,/icons/compressed.gif) x-compress x-gzip

AddIconByType (TXT,/icons/text.gif) text/*

AddIconByType (IMG,/icons/image2.gif) image/*

AddIconByType (SND,/icons/sound2.gif) audio/*

AddIconByType (VID,/icons/movie.gif) video/*

AddIcon /icons/binary.gif .bin .exe

AddIcon /icons/binhex.gif .hqx

AddIcon /icons/tar.gif .tar

AddIcon /icons/world2.gif .wrl .wrl.gz .vrml .vrm .iv

AddIcon /icons/compressed.gif .Z .z .tgz .gz .zip

AddIcon /icons/a.gif .ps .ai .eps

AddIcon /icons/layout.gif .html .shtml .htm .pdf

AddIcon /icons/text.gif .txt

AddIcon /icons/c.gif .c

AddIcon /icons/p.gif .pl .py

AddIcon /icons/f.gif .for

AddIcon /icons/dvi.gif .dvi

AddIcon /icons/uuencoded.gif .uu

AddIcon /icons/script.gif .conf .sh .shar .csh .ksh .tcl

AddIcon /icons/tex.gif .tex

AddIcon /icons/bomb.gif core

AddIcon /icons/back.gif ..

AddIcon /icons/hand.right.gif README

AddIcon /icons/folder.gif ^^DIRECTORY^^

AddIcon /icons/blank.gif ^^BLANKICON^^

#

# DefaultIcon 用于为制定图标的文件所显示的图标

#

DefaultIcon /icons/unknown.gif

#

# AddDescription在服务器生成的检索的某个文件后追加小段说明

# 此项只在设置为FancyIndexed时有效

# 格式：AddDescription &quot;描述&quot; 文件名

#

#AddDescription &quot;GZIP compressed document&quot; .gz

#AddDescription &quot;tar archive&quot; .tar

#AddDescription &quot;GZIP compressed tar archive&quot; .tgz

#

# ReadmeName是服务器默认的README文件

# 并且会追加到目录列表的最后

#

# HeaderName 是目录中需要预先显示内容的文件名

#

# 如果MultiViews在选项中，作为结果，服务器将先找name.html，

# 如果存在就包含它如果name.html不存在，

# 服务器会继续寻找name.txt如果存在就作为纯文本包含进来

#

ReadmeName README

HeaderName HEADER

#

# IndexIgnore是一系列的文件名目录索引将忽略这些文件并且不包含在列表中

# 允许使用通配符

#

IndexIgnore .??* *~ *# HEADER* README* RCS CVS *,v *,t

# indexing标识结束

#

# AddEncoding 可用于特殊浏览器(Mosaic/X 2.1+)快速传输压缩信息

# 注：并不是所有的服务器都支持

# 除了名字相似，以下Add*标识对上面的FancyIndexing定制标识无影响

#

AddEncoding x-compress Z

AddEncoding x-gzip gz tgz

#

# AddLanguage用于指定文档的语言

# 可以使用content标签指定每个文件的语言

#

# 注 1: 后缀不必与所用语言的关键字相同

# --- 波兰语（Polish，标准代码为pl）的文档可以用

# &quot;AddLanguage pl .po&quot; 来避免与perl脚本文件混淆

#

# 注 2: 以下例子表明两个字母的语言缩写和两个字母的国家缩写并不一定相同

# E.g. Danmark/dk 对比 Danish/da.

#

# 注 3: 其中ltz使用了三个字符，与RFC的规定不同

# 但是这个问题正在修订中，并且重新清理RFC1766

#

# 丹麦Danish (da) - 荷兰Dutch (nl) - 英国English (en) - 爱萨尼亚Estonian (ee)

# 法国French (fr) - 德国German (de) - 现代希腊文Greek-Modern (el)

# 意大利Italian (it) - 朝鲜Korean (kr) - 挪威Norwegian (no)

# 葡萄牙Portuguese (pt) - 卢森堡Luxembourgeois* (ltz)

# 西班牙Spanish (es) - 瑞典Swedish (sv) - 加泰罗尼亚Catalan (ca) - 捷克Czech(cz)

# 波兰Polish (pl) - 巴西Brazilian Portuguese (pt-br) - 日本Japanese (ja)

# 俄国Russian (ru)

#

AddLanguage da .dk

AddLanguage nl .nl

AddLanguage en .en

AddLanguage et .ee

AddLanguage fr .fr

AddLanguage de .de

AddLanguage el .el

AddLanguage he .he

AddCharset ISO-8859-8 .iso8859-8

AddLanguage it .it

AddLanguage ja .ja

AddCharset ISO-2022-JP .jis

AddLanguage kr .kr

AddCharset ISO-2022-KR .iso-kr

AddLanguage no .no

AddLanguage pl .po

AddCharset ISO-8859-2 .iso-pl

AddLanguage pt .pt

AddLanguage pt-br .pt-br

AddLanguage ltz .lu

AddLanguage ca .ca

AddLanguage es .es

AddLanguage sv .se

AddLanguage cz .cz

AddLanguage ru .ru

AddLanguage tw .tw

AddCharset Big5 .Big5 .big5

AddCharset WINDOWS-1251 .cp-1251

AddCharset CP866 .cp866

AddCharset ISO-8859-5 .iso-ru

AddCharset KOI8-R .koi8-r

AddCharset UCS-2 .ucs2

AddCharset UCS-4 .ucs4

AddCharset UTF-8 .utf8

# LanguagePriority 可设置语言的优先级

#

# 优先级降序排列

# 在此处按照字母顺序，可自行修改

#

LanguagePriority en da nl et fr de el it ja kr no pl pt pt-br ru ltz ca es sv tw

#

# AddType 可临时改变mime.types或者指定特殊文件的格式

#

# 例如：PHP 3.x 模块 (非Apache标准配件，参见)可用下面格式定义：

#

#AddType application/x-httpd-php3 .php3

#AddType application/x-httpd-php3-source .phps

#

# PHP 4.x, 使用:

#

#AddType application/x-httpd-php .php

#AddType application/x-httpd-php-source .phps

AddType application/x-tar .tgz

#

# AddHandler 可将特定文件扩展名映射到处理方法上

# 与文件类型无关此特性可内建到服务器中或者追加在操作指令中（见下）

#

# 如果希望用服务器端应用或ScriptAliased外的CGI，取消以下行的注释符

#

# 用CGI脚本:

#

#AddHandler cgi-script .cgi

#

# 用服务器解析的HTML文档

#

#AddType text/html .shtml

#AddHandler server-parsed .shtml

#

# 取消以下注释符可激活Apache的send-asis HTTP file特性

#

#AddHandler send-as-is asis

#

# 如果使用服务器端解析的图像定位文件，使用以下标识：

#

#AddHandler imap-file map

#

# 要激活type maps使用：

#

#AddHandler type-map var

# 文档类型说明结束

#

# Action 定义在文件匹配时执行相应的脚本

# 可简化常用CGI文件的调用

# 格式: Action media/type /cgi-script/location

# 格式: Action handler-name /cgi-script/location

#

#

# MetaDir: 指定保存meta信息文件的目录

# 这些文件包含附加的HTTP头，在发送文档是一并发送

#

#MetaDir .web

#

# MetaSuffix: 指定包含meta信息的文件的后缀

#

#MetaSuffix .meta

#

# 可定制的错误响应(Apache类型)

# 共三种风格：

#

# 1) 纯文本

#ErrorDocument 500 &quot;The server made a boo boo.

# 注： 第一个&quot;号用于表示是文本，实际不输出

#

# 2) 本地重定向

#ErrorDocument 404 /missing.html

# to redirect to local URL /missing.html

#ErrorDocument 404 /cgi-bin/missing_handler.pl

# 注：可重定向到任何一个服务器端的脚本或文档

#

# 3) 外部重定向

#ErrorDocument 402

# 注: 大部分与初始请求关联的环境变量对这样的脚本无效

#

#

# 基于浏览器的定制操作

#

#

# 以下标识修改普通的HTTP响应操作

# 第一个标识针对Netscape2.x和其他无此功能的浏览器取消保持激活状态的功能

# 这些浏览器在执行这些功能时会出错

# 第二个标识针对IE4.0b2设置其中有一条不完整的HTTP/1.1指令

# 在301或302（重定向）响应时不能正确的保持激活状态

#

BrowserMatch &quot;Mozilla/2&quot; nokeepalive

BrowserMatch &quot;MSIE 4\.0b2;&quot; nokeepalive downgrade-1.0 force-response-1.0

#

# 下面的标识通过不产生基本的1.1响应取消对违反HTTP/1.0标准的浏览器的响应

#

BrowserMatch &quot;RealPlayer 4\.0&quot; force-response-1.0

BrowserMatch &quot;Java/1\.0&quot; force-response-1.0

BrowserMatch &quot;JDK/1\.0&quot; force-response-1.0

# 浏览器定制标识结束

#

# 允许使用URL&quot;&quot;的形式查看服务器状态报告

# 修改 &quot;.your_domain.com&quot;来匹配相应的域名以激活此功能

#

#

# SetHandler server-status

# Order deny,allow

# Deny from all

# Allow from .your_domain.com

#

#

# 允许使用URL&quot;://servername/server-info&quot;（要求加载mod_info.c），

# 来远程察看服务器配置报告

# 修改 &quot;.your_domain.com&quot;来匹配相应的域名以激活此功能

#

#

# SetHandler server-info

# Order deny,allow

# Deny from all

# Allow from .your_domain.com

#

#

# 据报有人试图利用一个老的1.1漏洞

# 这个漏洞与CGI脚本在Apache服务器上分布有关

# 通过取消下面几行的注释符，可以将此类攻击记录转移到phf.apache.org上的记录脚本上

# 或者也可以利用脚本scriptsupport/phf_abuse_log.cgi记录在本地服务器上

#

#

# Deny from all

# ErrorDocument 403

#

#

# 代理服务器标识取消下列行的注释符可激活代理服务器

#

#

# ProxyRequests On

#

# Order deny,allow

# Deny from all

# Allow from .your_domain.com

#

#

# 激活/取消处理HTTP/1.1 &quot;Via:&quot; 报头

# (&quot;Full&quot;：加入服务器版本; &quot;Block&quot;：取消所有外发的Via: 报头)

# 可设置值: Off | On | Full | Block

#

# ProxyVia On

#

# 可修改下列各行并取消注释符来激活缓存

# (没有CacheRoot标识就不使用缓存)

#

# CacheRoot &quot;E:/Program Files/Apache Group/Apache/proxy&quot;

# CacheSize 5

# CacheGcInterval 4

# CacheMaxExpire 24

# CacheLastModifiedFactor 0.1

# CacheDefaultExpire 1

# NoCache a_domain.com another_domain.edu joes.garage_sale.com

### 部分 3: 虚拟主机

#

# 虚拟主机: 如果希望在一台服务器上实现多个域名和主机名的服务，

# 可设置VirtualHost来实现Most configurations

# 大部分的设置使用基于名称的虚拟主机，这样服务器就不必为IP地址操心

# 这些用星号在下面的标识中标出

#

# 在试图设置虚拟主机前

# 请阅读&lt;&gt;中的文档

# 以了解细节问题

#

# 可用命令行参数 -S来确认虚拟主机的设置

#

#

# 使用基于名称的虚拟主机

#

#NameVirtualHost *

apache MPM

apache2 主要的优势就是对多处理器的支持更好，在编译时同过使用-with-mpm选项来决定apache2的工作模式。如果知道当前的apache2使用什么工作机制，可以通过httpd -l命令列出apache的所有模块，就可以知道其工作方式：

服务器的优化

(MPM: Multi-Processing Modules)

apache2主要的优势就是对多处理器的支持更好，在编译时同过使用-with-mpm选项来决定apache2的工作模式。如果知道当前的apache2使用什么工作机制，可以通过httpd -l命令列出apache的所有模块，就可以知道其工作方式：

prefork：

如果httpd -l列出prefork.c，则需要对下面的段进行配置：

StartServers 5 #启动apache时启动的httpd进程个数。

MinSpareServers 5 #服务器保持的最小空闲进程数。

MaxSpareServers 10 #服务器保持的最大空闲进程数。

MaxClients 150 #最大并发连接数。

MaxRequestsPerChild 1000 #每个子进程被请求服务多少次后被kill掉。0表示不限制，推荐设置为1000。

在该工作模式下，服务器启动后起动5个httpd进程(加父进程共6个，通过ps -ax|grephttpd命令可以看到)。当有用户连接时，apache会使用一个空闲进程为该连接服务，同时父进程会fork一个子进程。直到内存中的空闲进程达到MaxSpareServers。该模式是为了兼容一些旧版本的程序。我缺省编译时的选项。

worker：

如果httpd -l列出worker.c，则需要对下面的段进行配置：

StartServers 2 #启动apache时启动的httpd进程个数。

MaxClients 150 #最大并发连接数。

MinSpareThreads 25 #服务器保持的最小空闲线程数。

MaxSpareThreads 75 #服务器保持的最大空闲线程数。

ThreadsPerChild 25 #每个子进程的产生的线程数。

MaxRequestsPerChild 0 #每个子进程被请求服务多少次后被kill掉。0表示不限制，推荐设置为1000。

该模式是由线程来监听客户的连接。当有新客户连接时，由其中的一个空闲线程接受连接。服务器在启动时启动两个进程，每个进程产生的线程数是固定的 (ThreadsPerChild决定)，因此启动时有50个线程。当50个线程不够用时，服务器自动fork一个进程，再产生25个线程。

perchild：

如果httpd -l列出perchild.c，则需要对下面的段进行配置：

NumServers 5 #服务器启动时启动的子进程数

StartThreads 5 #每个子进程启动时启动的线程数

MinSpareThreads 5 #内存中的最小空闲线程数

MaxSpareThreads 10 #最大空闲线程数

MaxThreadsPerChild 2000 #每个线程最多被请求多少次后退出。0不受限制

MaxRequestsPerChild 10000 #每个子进程服务多少次后被重新fork。0表示不受限制。

该模式下，子进程的数量是固定的，线程数不受限制。当客户端连接到服务器时，又空闲的线程提供服务。 如果空闲线程数不够，子进程自动产生线程来为新的连接服务。该模式用于多站点服务器。

ab(Stress Testing)

ab -c 1000  URL

ab -n 1000 URL

[http://cqfish.blog.51cto.com/622299/138726](http://cqfish.blog.51cto.com/622299/138726)

.ab [options] [ [http://]hostname[:port]/path](http://]hostname[:port]/path)

参数

-n requests     Number of requests to perform

//在测试会话中所执行的请求个数。默认时，仅执行一个请求

-c concurrency Number of multiple requests to make

//一次产生的请求个数。默认是一次一个。

-t timelimit    Seconds to max. wait for responses

//测试所进行的最大秒数。其内部隐含值是-n 50000。它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。

-p postfile     File containing data to POST

//包含了需要POST的数据的文件.

-T content-type Content-type header for POSTing

//POST数据所使用的Content-type头信息。

-v verbosity    How much troubleshooting info to print

//设置显示信息的详细程度 - 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。 -V 显示版本号并退出。

-w              Print out results in HTML tables

//以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。

-i              Use HEAD instead of GET

// 执行HEAD请求，而不是GET。

-x attributes   String to insert as table attributes

//

-y attributes   String to insert as tr attributes

//

-z attributes   String to insert as td or th attributes

//

-C attribute    Add cookie, eg. Apache=1234\. (repeatable)

//-C cookie-name=value 对请求附加一个Cookie:行。 其典型形式是name=value的一个参数对。此参数可以重复。

-H attribute    Add Arbitrary header line, eg. Accept-Encoding: gzip

Inserted after all normal header lines. (repeatable)

-A attribute    Add Basic WWW Authentication, the attributes

are a colon separated username and password.

-P attribute    Add Basic Proxy Authentication, the attributes

are a colon separated username and password.

//-P proxy-auth-username:password 对一个中转代理提供BASIC认证信任。用户名和密码由一个:隔开，并以base64编码形式发送。无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。

-X proxy:port   Proxyserver and port number to use

-V              Print version number and exit

-k              Use HTTP KeepAlive feature

-d              Do not show percentiles served table.

-S              Do not show confidence estimators and warnings.

-g filename     Output collected data to gnuplot format file.

-e filename     Output CSV file with percentages served

-h              Display usage information (this message)

//-attributes 设置 属性的字符串. 缺陷程序中有各种静态声明的固定长度的缓冲区。另外，对命令行参数、服务器的响应头和其他外部输入的解析也很简单，这可能会有不良后果。它没有完整地实现 HTTP/1.x; 仅接受某些预想的响应格式。 strstr(3)的频繁使用可能会带来性能问题，即, 你可能是在测试ab而不是服务器的性能。

参数很多,一般我们用 -c 和 -n 参数就可以了. 例如:

打开cmd，输入 以下代码。

cd C:\Apache2.2\bin

ab -n 1000 -c 100 [http://url/](http://url/)

这个表示同时处理100个请求并运行1000次index.php文件。以下是打印出来的内容。

This is ApacheBench, Version 2.0.41-dev &lt;$Revision: 1.121.2.12 $&gt; apache-2.0

Copyright (c) 1996 Adam Twiss, Zeus Technology Ltd,http://url/

Copyright (c) 1998-2002 The Apache Software Foundation,   [http://url/](http://url/)

Benchmarking 127.0.0.1 (be patient)

Completed 100 requests

Completed 200 requests

Completed 300 requests

Completed 400 requests

Completed 500 requests

Completed 600 requests

Completed 700 requests

Completed 800 requests

Completed 900 requests

Finished 1000 requests

Server Software:        Apache/2.2.8

//平台apache 版本2.2.8

Server Hostname:        zf.guqin.com

//服务器主机名

Server Port:            80

//服务器端口

Document Path:          /index.php

//测试的页面文档

Document Length:        1018 bytes

//文档大小

Concurrency Level:      1000

//并发数

Time taken for tests:   8.188731 seconds

//整个测试持续的时间

Complete requests:      1000

//完成的请求数量

Failed requests:        0

//失败的请求数量

Write errors:           0

Total transferred:      1361581 bytes

//整个场景中的网络传输量

HTML transferred:       1055666 bytes

//整个场景中的HTML内容传输量

Requests per second:    122.12 [#/sec] (mean)

//大家最关心的指标之一，相当于 LR 中的每秒事务数，后面括号中的 mean 表示这是一个平均值(指的是吞吐率?)

Time per request:       8188.731 [ms] (mean)

//大家最关心的指标之二，相当于 LR 中的平均事务响应时间，后面括号中的 mean 表示这是一个平均值(指的是用户平均请求等待时间?)

Time per request:       8.189 [ms] (mean, across all concurrent requests)

//大家最关心的指标之三，指的是服务器平均请求处理时间(每个请求实际运行时间的平均值?)

Transfer rate:          162.30 [Kbytes/sec] received

//平均每秒网络上的流量，可以帮助排除是否存在网络流量过大导致响应时间延长的问题

Connection Times (ms)

                      min    mean[+/-sd] median   max

Connect:        4 646 1078.7     89    3291

Processing:   165 992 493.1    938    4712

Waiting:         118 934 480.6    882    4554

Total:              813 1638 1338.9   1093    7785

//网络上消耗的时间的分解，各项数据的具体算法还不是很清楚

Percentage of the requests served within a certain time (ms)

50%   1093

66%   1247

75%   1373

80%   1493

90%   4061

95%   4398

98%   5608

99%   7368

100%   7785 (longest request)

//整个场景中所有请求的响应情况。在场景中每个请求都有一个响应时间，其中50％的用户响应时间小于1093 毫秒，60％ 的用户响应时间小于1247 毫秒，最大的响应时间小于7785 毫秒

由于对于并发请求，cpu实际上并不是同时处理的，而是按照每个请求获得的时间片逐个轮转处理的，所以基本上第一个Time per request时间约等于第二个Time per request时间乘以并发请求数。

&lt;Directory&gt;

Apache 核心指令中， [\\l &quot;limit&quot; \\t &quot;blank&quot;](http://www.phpchina.com/manual/apache/mod/core.html) &lt;Limit&gt;/&lt;LimitExcept&gt; 配置段 用于对指定的 HTTP 方法进行访问控制， &lt;Directory&gt;/&lt;Files&gt;/&lt;Location&gt; 配置段则是用于将它们封装起来的指令集作用于指定的目录、文件或网络空间 ( 详见 [\\t &quot;blank&quot;](http://fengchangjian.com/?p=735) 『 Apache 核心 (Core) 指令 &lt;Location&gt; 和 &lt;Directory&gt; 区别』 ) 。因此，将 &lt;Limit&gt;/&lt;LimitExcept&gt; 配置段和 &lt;Directory&gt;/&lt;Files&gt; /&lt;Location&gt; 配置段联合起来，就能实现 Apache 对指定 HTTP 请求进行访问控制的功能。它们的配置形式 ( 以 &lt;Directory&gt; 和 &lt;Limit&gt;/&lt;LimitExcept&gt; 说明 ) 可以表示如下 :

| 1 |
| --- |

2

3

4

5

6

7 &lt;Directory &quot;C:/apache/www&quot;&gt;

&lt;Limit DELETE TRACE OPTIONS&gt;

   # 禁止 DELETE TRACE OPTIONS 请求

   Order allow,deny

   Deny from all

&lt;/Limit&gt;

&lt;/Directory&gt; 上述配置表示，对于任何用 DELETE 、 TRACE 、 OPTIONS 方法访问 C:/apache/www 目录里资源文件的请求都会被 apache 服务器拒绝 (403 Fobidden) 。还可以这样表示 :

| 1 |
| --- |

2

3

4

5

6

7 &lt;Directory &quot;C:/apache/www&quot;&gt;

&lt;LimitExcept GET POST HEAD&gt;

   # 禁止除 GET POST HEAD 以外的请求

   Order allow,deny

   Deny from all

&lt;/Limit&gt;

&lt;/Directory&gt; 这段配置表示，对于任何用 GET 、 POST 、 HEAD 方法访问 C:/apache/www 目录里资源文件的请求都会被 apache 服务器拒绝 (403 Fobidden) 。相当于前一个 (&lt;Limit&gt;) 是黑名单策略，后一个 (&lt;LimitExcept&gt;) 是白名单策略。

log format

**定制日志文件格式**

[\\\\l &quot;logformat&quot;](http://www.phpchina.com/resource/manual/apache/mod/mod_log_config.html) LogFormat 和 [\\\\l &quot;customlog&quot;](http://www.phpchina.com/resource/manual/apache/mod/mod_log_config.html) CustomLog 指令的格式化参数是一个字符串。这个字符串会在每次请求发生的时候，被记录到日志中去。它可以包含将被原样写入日志的文本字符串以及 C 风格的控制字符 &quot;\n&quot; 和 &quot;\t&quot; 以实现换行与制表。文本中的引号和反斜杠应通过 &quot;\&quot; 来转义。

请求本身的情况将通过在格式字符串中放置各种 &quot; % &quot; 转义符的方法来记录，它们在写入日志文件时，根据下表的定义进行转换：

| **格式字符串** | **描述** |
| --- | --- |
| %% | 百分号 ( _Apache2.0.44_ _或更高的版本_ ) |
| %a | 远端 IP 地址 |
| %A | 本机 IP 地址 |
| %B | 除 HTTP 头以外传送的字节数 |
| %b | 以 CLF 格式显示的除 HTTP 头以外传送的字节数，也就是当没有字节传送时显示 - 而不是 0 。 |
| %{ _Foobar_ }C | 在请求中传送给服务端的 cookie _Foobar_ 的内容。 |
| %D | 服务器处理本请求所用时间，以微为单位。 |
| %{ _FOOBAR_ }e | 环境变量 _FOOBAR_ 的值 |
| %f | 文件名 |
| %h | 远端主机 |
| %H | 请求使用的协议 |
| %{ _Foobar_ }i | 发送到服务器的请求头 _Foobar:_ 的内容。 |
| %l | 远端登录名 ( 由 identd 而来，如果支持的话 ) ，除非 [\\\\l &quot;identitycheck&quot;](http://www.phpchina.com/resource/manual/apache/mod/core.html) IdentityCheck |

mod_pagespeed

为了帮助提升各类网站的访问速度， Google 发布了一个名为 mod_pagespeed 的自动化 Apache 优化模块，目前支持 CentOS, RHEL, Ubuntu, Debian, Fedora 等 Linux 发行版，用户只需要下载并安装相应的 Deb 或 RPM 包就可以完全自动优化 Apache Http 服务器了。

mod_pagespeed 可以做到：

   不需要对网站 CMS 系统进行改变即可应用。

   加速模块可以自行对网络传输的 html 字节优化及对图象 、css 进入压缩优化传输

   智能缓存是一大亮点，它可以自动智能缓存，加速下载

目前这套优化模块已经应用具于有 850万客户的 GoDaddy 服务器上，而且反响良好。根据此前的一些实践来看， 通过 mod_pagespeed 可以对 Web 性能的多个方面，包括缓存、客户端与服务器之间的连接、载荷大小等进行优化，最大可将页面加载时间缩短 50% 。

项目主页： [http://code.google.com/speed/page-speed/docs/module.html](http://code.google.com/speed/page-speed/docs/module.html)

插件下载地址： [http://code.google.com/speed/page-speed/download.html](http://code.google.com/speed/page-speed/download.html)

开源项目地址： [http://code.google.com/p/modpagespeed/](http://code.google.com/p/modpagespeed/)

Redhat包的直接下载地址： [https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-stable_current_x86_64.rpm](https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-stable_current_x86_64.rpm)