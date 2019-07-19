#### tomcat {#tomcat}

[http://blog.chinaunix.net/uid-509190-id-3056209.html](http://blog.chinaunix.net/uid-509190-id-3056209.html)

整合apache和tomcat构建Web服务器 (

linux是最常用的web服务器，本节我们将通过整合apache和tomcat构建一个java/jsp运行平台，详细介绍web服务器的搭建过程。

一、  apache与tomcat整合的必要性

Apache是最流行的Web服务器，开放源代码，支持跨平台的应用（可以运行在几乎所有的Linux、Unix、Windows系统平台上），尤其对Linux的支持相当完美。

apache的优点有：

? 功能强大，apache自带了很多功能模块，可根据需求编译自己需要的模块。

? 配置简单，apache的配置文件非常简单，通过简单的配置可实现强大功能。

? 速度飞快，apache处理静态页面文件效率非常高，可以应对大并发和高负荷访问请求。

? 性能稳定，apache在高负荷请求下性能表现卓越，执行效率非常高。

但是apache也有自身的缺点：

? 只支持静态网页，对于jsp、php等动态网页不支持

? Apache是以进程为基础的结构，进程要比线程消耗更多的系统开支，因此，不太适合于多处理器环境。

Tomcat是Sun和Apache合作做出来的JSPServer，有如下优点：

? 支持Servlet和JSP，可以很好的处理动态网页。

? 跨平台性好：Tomcat是Java程序，所以只要有JDK就可以使用,不需要考虑操作系统平台。

但是，tomcat也有自身缺点：

? 处理静态页面效率不高：Tomcat本身可以做为Web Server，但是tomcat在处理静态页面时没有Apache迅速。

? 可配置性不强：tomcat不像Apache一样配置简单,稳定、强壮。

综上所述，通过相互的整合刚好弥补了各自的缺点，通过整合可以实现：

? 客户端请求静态页面时，由Apache服务器响应请求。

? 客户端请求动态页面时，则是Tomcat服务器响应请求。

? 通过apache信息过滤，实现网站动、静页面分离，保证了应用的可扩展性和安全性。

既然要让Apache和Tomcat协调工作,就必需有一个连接器把它们联系起来，这就是下面要提到的Connector，下个小节具体讲述Connector的选择和使用。

二、 Apache和Tomcat连接器

Apache是模块化的web服务器，这意味着核心中只包含实现最基本功能的模块。扩展功能可以作为模块动态加载来实现。为了让apache和tomcat协调工作，开源爱好者们开发出了很多可以利用的模块，在Apache2.2版本之前，一般有两个模块可供选择：mod_jk2和mod_jk，mod_jk2模块是比较早的一种连接器，在动、静页面过滤上可以使用正则表达式，因此使用配置灵活，但是mod_jk2模块现在已经没有开发人员支持了，版本更新也就此停止。继承jk2模块的是mod_jk模块，mod_jk模块支持Apache 1.x和2.X系列版本，现在一般都使用mod_jk做Apache和Tomcat的连接器。

在 Apache2.2版本以后，又出现了两种连接器可供选择，那就是http-proxy和proxy-ajp模块，apache的proxy（代理）模块可以实现双向代理，功能非常强大，从连接器的实现原理看，用http-proxy模块实现也是很自然的事情，只需打开tomcat的http功能，然后用 apache的proxy代理功能将动态请求交给tomcat处理，而静态数据交给apache自身就可以了。proxy-ajp模块是专门为 tomcat整合所开发的，通过ajp协议专门代理对tomcat的请求。根据官方的测试，proxy-ajp的执行效率要比http-proxy高，因此在Apache2.2以后的版本，用proxy-ajp模块作为apache和tomcat的连接器是个不错的选择。

需要说明的是，这些连接功能的实现，都是通过在apache中加载相应的功能模块实现，比如上面提到的mod_jk、mod_jk2、proxy-ajp模块，都要事先通过源码编译出对应的模块，然后通过apache配置文件动态加载，实现连接器功能。这点也是apache的优势所在。

在下面的讲述中，我们将重点讲述mod_jk作为连接器的安装配置与实现。

三、 Apache与tomcat以及JK模块的安装

下面以Red Hat Enterprise Linux Server release 5操作系统为例，详细介绍apache+tomcat+jk的安装过程。

1．安装apache

Apache 目前有几种主要版本，包括1.3.x、2.0.x以及2.2.x等等，在 1.3.x以前的版本中通常取名以apache开头，2.x以后版本则用httpd开头来命名。

apache的官方地址为 [http://httpd.apache.org/](http://httpd.apache.org/) ，这里以源码的方式进行安装，我们下载的版本是Apache2.0.59，下载后的压缩包文件为httpd-2.0.59.tar.gz。

下面是具体的编译安装过程：

[root@webserver ~]#tar -zxvf  httpd-2.0.59.tar.gz

[root@webserver ~]#cd httpd-2.0.59

[root@webserver ~]#./configure --prefix=/usr/local/apache2 \

--enable-modules=most \

--enable-mods-shared=all \

--enable-so \

[root@webserver ~]#make

[root@webserver ~]#make install

Apache 安装步骤以及选项的含义已经在第五章有详细的介绍，这里不在详述，这里我们设定apache的安装路径为/usr/local /apache2，&quot;--enable-modules=most&quot;表示将大部分模块静态编译到httpd二进制文件中，&quot;--enable-mods- shared=all&quot;表示动态加载所有模块，如果去掉-shared话，是静态加载所有模块。

2．安装tomcat

Tomcat的官方地址 [http://jakarta.apache.org/](http://jakarta.apache.org/) ，这里以二进制方式安装，我们只需下载对应的二进制版本即可，这里使用的版本是tomcat-5.0.30，下载后的压缩包文件为jakarta- tomcat-5.0.30.tar.gz，把此安装包放到/usr/local目录下，通过解压即可完成tomcat的安装。

基本步骤如下：

[root@webserver local]# tar -zxvf jakarta-tomcat-5.0.30.tar.gz

[root@webserver local]#mv jakarta-tomcat-5.0.30  tomcat5.0.30

由于解压后的目录名字太长，不易操作，因此可以直接将解压后的目录重命名适合记忆的名字，这里我们将jakarta-tomcat-5.0.30重命名为tomcat5.0.30，软件名称加上软件版本的格式便于记忆。

3．安装JDK

在tomcat运行环境下，JDK是必不可少的软件，因为tomcat只是一个Servlet/JSP容器，底层的操作都需要JDK来完成。

JDK的安装也非常简单，只需到 [http://java.sun.com/](http://java.sun.com/) 下载对应的JDK即可，这里我们下载的版本是JDK1.6,对应的文件为jdk-6u7-linux-i586.bin，下载时将所需软件包文件保存在/usr/local目录下，安装步骤如下：

[root@webserver ~]#cd  /usr/local

[root@webserver local]#chmod 755 jdk-6u7-linux-i586.bin

[root@webserver local]#./jdk-6u7-linux-i586.bin

然后根据提示输入&quot;yes&quot;，程序会自动完成安装，安装完毕，会在/usr/local/下产生一个 jdk1.6.0_07目录，这个就是JDK的程序目录了。

[root@localhost local]# /usr/local/jdk1.6.0_07/bin/java -version

java version &quot;1.6.0_07&quot;

Java(TM) SE Runtime Environment (build 1.6.0_07-b06)

Java HotSpot(TM) Server VM (build 10.0-b23, mixed mode)

从上面输出可以看出，JDK在我们的linux下运行正常。并且版本为1.6.0_07。

4．安装JK模块

为了更灵活的使用mod_jk连接器，这里我们采用源码方式编译出所需要的JK模块，JK的源码可以从这个地址去下载， [http://archive.apache.org/dist/jakarta/tomcat-connectors/jk/](http://archive.apache.org/dist/jakarta/tomcat-connectors/jk/) ，但是不保证此连接永久有效，这里采用的JK版本为jk-1.2.15。

下载后的JK源码压缩包文件为jakarta-tomcat-connectors-current-src.tar.gz，这里也将此压缩包放到/usr/local下，具体安装步骤如下：

[root@webserver ~]# cd /usr/local/

[root@webserver local]# tar xzvf jakarta-tomcat-connectors-1.2.15-src.tar.gz

[root@webserver local]# cd jakarta-tomcat-connectors-1.2.15-src/jk/native

[root@webserver native]#chmod 755 buildconf.sh

[root@webserver native]# ./buildconf.sh

[root@webserver native]#./configure \ --with-apxs=/usr/local/apache2/bin/apxs  #这里指定的是apache安装目录中apxs的位置

[root@webserver native]# make

[root@webserver native]# make install

[root@webserver native]# cd apache-2.0

[root@webserver native]#ll mod_jk.so

-rwxr-xr-x 1 root root 477305 Oct  9 08:49 mod_jk.so

可以看到有mod_jk.so文件生成，这就是我们需要的JK连接器，默认情况下JK模块会自动安装到/usr/local/apache2/modules目录下，如果没有自动安装到此目录，手动拷贝此文件到modules目录即可。

四、 apache与tomcat整合配置

本节详细讲述apache和tomcat整合的详细配置过程，这里假定web服务器的IP地址为192.168.60.198，测试的jsp程序放置在/webdata/www目录下，如果没有此目录，需要首先创建这个目录，因为在下面配置过程中，会陆续用到/webdata/www这个路径。

1．创建Tomcat workers

Tomcat worker是一个服务于web server、等待执行servlet/JSP的Tomcat实例，创建tomcat workers需要增加3个配置文件，分别是Tomcat workers配置文件workers.properties，URL映射文件uriworkermap.properties和JK模块日志输出文件 mod_jk.log，mod_jk.log文件会在apache启动时自动创建，这里只需创建前两个文件即可。

（1）tomcat workers配置文件

定义Tomcat workers的方法是在apache的conf目录下编写一个名为&quot;workers.properties&quot;的属性文件，使其作为apache的插件来发挥作用，下面讲述workers.properties配置说明。

worker.list用来定义Workers列表，当apache启动时，workers.properties作为插件将初始化出现在worker.list列表中的workers。

定义worker类型的格式：

worker.worker名字.type=

例如：

定义一个名为&quot;tomcat12&quot;的worker，其使用ajp12协议与tomcat 进程通讯：

　　worker.tomcat12.type=ajp12

定义一个名为&quot;tomcat13&quot;的worker，其使用ajp13协议与tomcat 进程通讯：

　　worker.remote.type=ajp13

　　定义一个名为&quot;tomcatjni&quot;的worker，其使用JNI的方式与tomcat 进程通讯

　　worker.tomcatjni.type=jni

　　定义一个名为&quot;tomcatloadbalancer&quot;的worker，作为对多个tomcat进程的负载平衡使用：

　　worker.tomcatloadbalancer.type=lb

设置worker属性的格式为：

worker.worker名字.属性＝

这里只说明ajp13协议支持的几个常用属性：

? Host：监听ajp13请求的的tomcat worker主机地址

? Port：tomcat worker主机监听的端口。默认情况下tomcat在ajp13协议中使用的端口为8009。

? lbfactor：当tomcat用作负载均衡时，此属性被使用，表示此tomcat worker节点的负载均衡权值。

下面是我们的workers.properties文件内容：

[root@localhost ~]#vi /usr/local/apache2/conf/workers.properties

worker.list=tomcat1

worker.tomcat1.port=8009

worker.tomcat1.host=localhost

worker.tomcat1.type=ajp13

worker.tomcat1.lbfactor=1

（2）URL过滤规则文件uriworkermap.properties

也就是URI 映射文件，用来指定哪些 URL 由 Tomcat 处理，也可以直接在 httpd.conf 中配置这些 URI，但是独立这些配置的好处是 JK 模块会定期更新该文件的内容，使得我们修改配置的时候无需重新启动 Apache 服务器。

下面是我们的一个映射文件的内容：

[root@localhost ~]#vi  /usr/local/apache2/conf/uriworkermap.properties

/*=tomcat1

!/*.jpg=tomcat1

!/*.gif=tomcat1

!/*.png=tomcat1

!/*.bmp=tomcat1

!/*.html=tomcat1

!/*.htm=tomcat1

!/*.swf=tomcat1

!/*.css= tomcat1

!/*.js= tomcat1

在上面的配置文件中，&quot;/*=tomcat1&quot;表示将所有的请求都交给tomcat1来处理，而这个&quot;tomcat1&quot;就是我们在 workers.properties文件中由worker.list指定的。这里的&quot;/&quot;是个相对路径，表示存放网页的根目录，这里是上面假定的 /webdata/www目录。

&quot;!/*.jpg=tomcat1&quot;则表示在根目录下，以&quot;*.jpg&quot;结尾的文件都不由JK进行处理，其它设置含义类似，也就是让apache处理图片、js文件、css文件以及静态html网页文件。

特别注意，这里有个先后顺序的问题，JK模块在处理网页根目录文件的时候，会首先过滤掉不让自己处理的设定，剩下的设定自己全部处理。

例如上面的设定中，JK模块会首先在/webdata/www目录过滤掉所有图片、flash、js文件、css文件和静态网页，将剩下的文件类型自己全部处理。

2．Apache的配置

（1）apache的目录结构

上面我们通过源码方式把apache安装到了/usr/local/apache2下，详细的目录结构如表8.1所示：

表8.1

目录名称 目录作用

bin Apache二进制程序及服务程序目录

lib 库文件目录

conf 主配置文件目录

logs 日志文件目录

htdocs 默认web应用根目录

cgi-bin 默认的cgi目录

modules 动态加载模块目录，上面生成的JK模块，就放在了这个目录下。

manual Apache使用文档目录

man Man帮助文件目录

error 默认的错误应答文件目录

include 包含头文件的目录

icons Apache图标文件目录

（2）apache的配置文件

? /usr/local/apache2/conf/httpd.conf(apache主要配置文件)

httpd.conf是包含若干指令的纯文本文件，配置文件的每一行包含一个指令,指令是不区分大小写的，但是指令的参数却对大小写比较敏感，&quot;#&quot;开头的行被视为注解并被忽略，但是，注解不能出现在指令的后边。配置文件中的指令对整个web服务器都是有效的。

? /usr/local/apache2/bin/apachectl (apache启动/关闭程序)

可以通过&quot;/usr/local/apache2/bin/apachectl start/stop/restart&quot;的方式启动/关闭/重启apache进程。apachectl其实是个shell脚本，它可以自动检测 httpd.conf的指令设定，让apache在最优的方式下启动。

? /usr/local/apache2/bin/httpd

httpd是一个启动apache的二进制文件。

? /usr/local/apache2/modules

Apache是模块化的web服务器，所有编译的模块默认都会放到这个目录下，然后可以在httpd.conf文件中指定模块位置，动态加载！

? /usr/local/apache2/logs/access_log

/usr/local/apache2/logs/error_log

这两个分别为apache的访问日志文件和错误日志文件，通过监测这两个文件，我们可以了解apache的运行状态。

（3）httpd.conf基本设定

httpd.conf配置文件有3个部分组成，分别是：全局变量、配置主服务器、配置虚拟主机。

下面我们详解讲述下/usr/local/apache2/conf/httpd.conf文件各个指令的含义。

[root@webserver ~]#vi /usr/local/apache2/conf/httpd.conf

全局变量配置部分

ServerRoot &quot;/usr/local/apache2&quot;

ServerRoot用于指定守护进程httpd的运行目录, httpd在启动之后自动将进程的当前目录切换到这个指定的目录下，可以使用相对路径和绝对路径。

PidFile logs/httpd.pid

PidFile指定的文件将记录httpd守护进程的进程号，由于httpd能自动复制其自身，因此apache启动后，系统中就有多个httpd进程，但只有一个进程为最初启动的进程，它为其他进程的父进程，对父进程发送信号将影响所有的httpd进程。

Timeout 300

Timeout用来定义客户端和服务器端程序连接的超时间隔，单位为秒，超过这个时间间隔，服务器将断开与客户端的连接。

KeepAlive On

KeepAlive 用来定义是否允许用户建立永久连接，On为允许建立永久连接，Off表示拒绝用户建立永久连接，例如，要打开一个含有很多图片的页面，完全可以建立一个 tcp连接将所有信息从服务器传到客户端即可，而没有必要对每个图片都建立一个tcp连接。此选项建议设置为On。

MaxKeepAliveRequests 100

 MaxKeepAliveRequests用来定义一个tcp连接可以进行HTTP请求的最大次数，设置为0代表不限制请求次数，这个选项与上面的KeepAlive相互关联，当KeepAlive设定为On，这个设置开始起作用。

KeepAliveTimeout 15

 KeepAliveTimeout用来限定一次连接中最后一次请求完成后延时等待的时间，如果超过了这个等待时间，服务器就断开连接。

&lt;IfModule prefork.c&gt;

ServerLimit  300

StartServers         5

MinSpareServers      5

MaxSpareServers     20

MaxClients         300

MaxRequestsPerChild  2000

&lt;/IfModule&gt;

&lt;IfModule worker.c&gt;

StartServers         2

MaxClients         150

MinSpareThreads     25

MaxSpareThreads     75

ThreadsPerChild     25

MaxRequestsPerChild  0

&lt;/IfModule&gt;

上面的两段指令其实是对web服务器的使用资源进行的设置，apache可以运行在prefork和worker两种模式下，可以通过/usr/local /apache2/bin/httpd -l来确定当前apache运行在哪种模式，在编译apache时，如果指定&quot;--with-mpm=MPM&quot;参数，那么apache默认运行在 prefork模式下，如果指定的是&quot;--with-mpm=worker&quot;参数，那么默认运行在worker模式下。如果没有做任何模式指定，那么 apache默认也运行在prefork模式下。

prefork采用预派生子进程方式，用单独的子进程来处理不同的请求，进程之间彼此独立。

? StartServers表示在启动apache时，就自动启动的进程数目。

? MinSpareServers设置了最小的空闲进程数，这样可以不必在请求到来时再产生新的进程，从而减小了系统开销以增加性能。

? MaxSpareServers设置了最大的空闲进程数，如果空闲进程数大于这个值，apache会自动关闭这些多余进程，如果这个值设置的比小MinSpareServers，则Apache会自动把其调整为 MinSpareServers+1。

? MaxRequestsPerChild设置了每个子进程可处理的最大请求数，也就是一个进程能够提供的最大传输次数，当一个进程的请求超过此数目时，程序连接自动关闭。0意味着无限，即子进程永不销毁。这里我们设置为2000，已经基本能满足中小型网站的需要。

? MaxClients 设定Apache可以同时处理的请求数目。是对Apache性能影响最大的参数，默认值150对于中小网站基本够了，但是对于大型网站，是远远不够的，如果请求总数已达到这个值，那么后面的请求就必须排队，这就是系统资源充足而网站访问却很慢的主要原因。理论上这个值设置的越大，可以处理的请求的越多，但是apache默认限制不能超过256，如果要设置的值大于256，可以直接使用ServerLimit指令加大MaxClients。这里我们设置的值是300。

相对于prefork，worker是全新的支持多线程和多进程的混合模型，由于是使用线程来处理请求，所以可以处理更多的请求，对系统资源的使用开销也比较小。

? MinSpareThreads设置了最少的空闲线程数。

? MaxSpareThreads设置了最多的空闲线程数。

? MaxClients 设定同时连入客户端的最大数。如果现有子进程中的线程总数不能满足请求的负载，控制进程将派生出新的子进程。默认最大子进程数是16，加大时需要通过 ServerLimit来进行声明，ServerLimit最大值为20000，注意，如果指定了ServerLimit，那么此值乘以 ThreadsPerChild必须大于等于MaxClients，而MaxClients必须是ThreadsPerChild的整数倍，否则 Apache将会自动调节到一个相应值。

? ThreadsPerChild设定每个子进程的工作线程数，此选项在worker模式下与性能密切相关，默认最大值为64，如果系统负载很大，不能满足需求的话，需要使用 ThreadLimit指令，此指令默认最大值为20000，Worker模式下所能同时处理请求总数由子进程数乘以ThreadsPerChild值来确定，保证大于等于MaxClients的设定值。

Listen 80

此指令是设置apache的监听端口，默认的http服务都是运行在80端口下，当然也可以修改为其它端口。

LoadModule access_module modules/mod_access.so

LoadModule auth_module modules/mod_auth.so

LoadModule jk_module modules/mod_jk.so

加载mod_jk模块，上面我们已经生成了JK模块，并且放到了modules目录下，这里这需指定加载即可。

……以下省略……

配置主服务器

User nobody

Group nobody

这里是设定执行httpd的用户和组，默认是nobody用户启动apache，这里将组也设置为nobody。

ServerAdmin you@example.com

这里指定的是网站管理员的邮件地址，如果apache出现问题，会发信到这个邮箱。

ServerName www.example.com:80

这里是指定系统的主机名，如果没有指定，会以系统的hostname为依据。特别注意，这里设定的主机名一定要能找到对应的IP地址（主机名和IP的对应关系可以在/etc/hosts设置）。

UseCanonicalName Off

设定是否使用标准的主机名，如果设置为On，则以ServerName指定的主机名为主。如果web主机有多个主机名，请设置为Off。

DocumentRoot &quot;/usr/local/apache2/htdocs&quot;

此指令非常重要，是用来放置网页的路径，apache会默认到这个路径下寻找网页，并显示在浏览器上。

&lt;Directory /&gt;

#这里的&quot;/&quot;是相对路径，表示DocumentRoot指定的目录。

Options FollowSymLinks

AllowOverride None

&lt;/Directory&gt;

&lt;Directory &quot;/usr/local/apache2/htdocs&quot;&gt;

Options Indexes FollowSymLinks

Order allow,deny

Allow from all

&lt;/Directory&gt;

上面这段信息是对DocumentRoot指定目录的权限设定，有3个必须知道的参数：

Options 表示在这个目录内能够执行的操作，主要有5个可设定的值：

? Indexes：此参数表示，如果在DocumentRoot指定目录下找不到以index打头的文件时，就将此目录下所有文件列出来，很不安全，不建议使用这个参数。

? FollowSymLinks：表示在DocumentRoot指定目录下允许符号链接到其它目录。

? ExecCGI：表示允许在DocumentRoot指定的目录下执行cgi操作。

? Includes：准许SSI(Server-side Includes)操作。

? MultiViews：不常用，根据语言的不同显示不通的信息提示。

AllowOverride 通过设定的值决定是否读取目录中的.htaccess文件，来改变原来所设置的权限，其实完全可以在httpd.conf中设置所有的权限，但是这样对 apache使用者的其它用户如果要修改一些权限的话，就比较麻烦了，因此apache预设可以让用户以自己目录下的. htaccess文件复写权限，常用的选项有两个：

? All：表示可以读取.htaccess文件的内容，修改原来的访问权限。

? None：表示不读取.htaccess文件，权限统一控制。

Order 用来控制目录和文件的访问授权，常用的组合有2个：

? Deny,Allow：表示先检查禁止的设定，没有禁止的全部允许。

? Allow,Deny：表示先检查允许的设定，没有允许的全部禁止。

DirectoryIndex index.html index.htm index.jsp index.html.var

这里是对apache打开网站默认首页的设定，apache在打开网站首页时一般会查找index.*之类的网页文件，DirectoryIndex指令就是设置apache依次寻找能打开网站首页的顺序，例如我们要打开 www.ixdba.net 网站，apache会首先在DocumentRoot指定的目录下寻找index.html，也就是 www.ixdba.net/index.html ，如果没有找到index.html网页，那么apache会接着查找index.htm，如果找到就执行 www.ixdba.net/index.htm 打开首页，以此类推。

UserDir public_html

UserDir用于设定用户个人主页存放的目录，默认为&quot;public_html&quot;目录，例如有个用户为ixdba，如果他的根目录为/home/ixdba，那么他的默认主页存放路径为/home/ixdba/public_html。

AccessFileName .htaccess

定义每个用户目录下的访问控制文件的文件名，默认为.htaccess，

TypesConfig conf/mime.types

TypesConfig用来定义在哪里查询mime.types文件

HostnameLookups Off

用来指定apache在日志中记录访问端地址是ip还是域名，如果为Off，则记录IP地址，如果是On，记录域名信息，建议设置为Off。

ErrorLog logs/error_log

指定错误日志文件的位置

CustomLog logs/access_log common

指定apache访问日志文件的位置和记录日志的模式。

ServerTokens Full

这个指令定义包含在HTTP回应头中的信息类型，默认为&quot;Full&quot;，表示在回应头中将包含操作系统类型和编译信息，可以设为Full|OS|Minor|Minimal|Major|Prod列各值中的一个，Full包含的信息最多，而Prod最少。

ServerSignature On

此指令有3个选项，On、Off和Email，On选项表示在apache的出错页面会显示apache版本以及加载的模块信息，Email选项与On相同，但是还会多出一个包含管理员邮件地址的mailto连接。Off表示不显示任何上面信息。

Alias /icons/ &quot;/usr/local/apache2/icons/&quot;

&lt;Directory &quot;/usr/local/apache2/icons&quot;&gt;

   Options Indexes MultiViews

   AllowOverride None

   Order allow,deny

   Allow from all

&lt;/Directory&gt;

上面这段信息是apache中对别名的设定，当访问 [http://ip](http://ip) 或域名/icons时，由于Alias的原因，apache不会去DocumentRoot指定的目录查找文件，而是直接访问/usr/local/apache2/icons 目录下对应的文件信息。而&lt;Directory&gt;标签就是对这个目录权限的设定。

ScriptAlias /cgi-bin/ &quot;/usr/local/apache2/cgi-bin/&quot;

&lt;Directory &quot;/usr/local/apache2/cgi-bin&quot;&gt;

   AllowOverride None

   Options None

   Order allow,deny

   Allow from all

&lt;/Directory&gt;

这段信息和上面的Alias设定类似，只不过这个是设置cgi脚本的执行权限而已，apache默认在/usr/local/apache2/cgi-bin目录下具有cgi脚本执行权限。

JkWorkersFile /usr/local/apache2/conf/workers.properties

JkMountFile   /usr/local/apache2/conf/uriworkermap.properties

JkLogFile /usr/local/apache2/logs/mod_jk.log

JkLogLevel info

JkLogStampformat &quot;[%a %b %d %H:%M:%S %Y]&quot;

上面这5行是对JK连接器属性的设定，第一、二行指定Tomcat workers配置文件以及对网页的过滤规则，第三行指定JK模块的日志输出文件，第四行指定日志输出级别，最后一行指定日志输出格式。

虚拟主机的设定

NameVirtualHost *

表示启用虚拟主机，如果开启虚拟主机，上面DocumentRoot指令指定的配置将失效，以虚拟主机中指定的DocumentRoot为主。

&lt;VirtualHost *&gt;

   ServerAdmin webmaster@ixdba.net

   DocumentRoot /webdata/www

   ServerName 192.168.60.198

   ErrorLog logs/error_log

   CustomLog logs/access_log common

JkMountFile  conf/uriworkermap.properties

&lt;/VirtualHost&gt;

上面这段是添加一个虚拟主机，其实虚拟主机是通过不同的ServerName 来区分的，这里为了演示方便，使用IP代替域名。我们经常看到在一个web服务器上有很多个网站，并且每个站点都不相同，这就是通过虚拟主机技术实现的。

每个虚拟主机用&lt;VirtualHost&gt;标签设定，各个字段含义如下：

ServerAdmin：表示虚拟主机的管理员邮件地址。

DocumentRoot：指定虚拟主机站点文件路径。

ServerName：虚拟主机的站点域名

ErrorLog：指定虚拟主机站点错误日志输出文件。

CustomLog：指定虚拟主机站点访问日志输出文件。

JkMountFile：指定对此虚拟主机的URL映射文件。

例如，我们要在一个服务器上建立3个网站，只需配置下面3个虚拟主机即可：

&lt;VirtualHost *:80&gt;

   ServerAdmin webmaster_www@ixdba.net

   DocumentRoot /webdata/html

   ServerName www.ixdba.net

   ErrorLog logs/www.error_log

   CustomLog logs/www.access_log common

&lt;/VirtualHost&gt;

&lt;VirtualHost *:80&gt;

   ServerAdmin webmaster_bbs@ixdba.net

   DocumentRoot /webdata/bbs

   ServerName bbs.ixdba.net

   ErrorLog logs/bbs.error_log

   CustomLog logs/bbs.access_log common

&lt;/VirtualHost&gt;

&lt;VirtualHost *:80&gt;

   ServerAdmin webmaster_mail@ixdba.net

   DocumentRoot /webdata/mail

   ServerName mail.ixdba.net

   ErrorLog logs/mail.error_log

   CustomLog logs/mail.access_log common

&lt;/VirtualHost&gt;

这样，就建立了3个虚拟主机，对应的站点域名分别是 www.ixdba.net 、bbs.ixdba.net、mail.ixdba.net，接下来的工作就是将这3个站点域名对应的IP全部解析到一台web服务器即可。

3．Tomcat的配置

（1）tomcat的目录机构

在上面的操作中，Tomcat安装在了/usr/local/tomcat5.5.12下，tomcat每个目录的含义如表8.2所示：

表8.1

目录名称 目录作用

bin 存放各种平台下启动和关闭Tomcat的脚本文件

comm 存在Tomcat服务器及所有的web应用程序可以访问的JAR文件

conf 存放Tomcat的各种配置文件,最重要的是 server.xml和web.xml

logs 存放Tomcat日志文件

server 此目录又包含lib和webapps 2个主要目录，webapps存放Tomcat自带的两个WEB应用admin应用和manager应用，lib存放Tomcat服务器所需的各种JAR文件

shared 此目录又包含lib目录，lib目录主要存放所有web应用都可以访问的jar文件（但是不能被Tomcat服务器访问）

webapps Tomcat的主要Web发布目录，默认情况下把Web应用文件放于此目录

work 存放JSP编译后产生的class文件

（2）server.xml的配置

server.xml 是tomcat的核心配置文件，为了支持与apache的整合，在tomcat中也需要配置虚拟主机，server.xml是一个有标签组成的文本文件，找到默认的&lt;Host&gt;标签，在此标签结尾，也就是&lt;/Host&gt;后面增加如下虚拟主机配置：

&lt;Host name=&quot;192.168.60.198&quot; debug=&quot;0&quot; appBase=&quot;/webdata/www&quot; unpackWARs=&quot;true&quot;&gt;

   &lt;Context path=&quot;&quot; docBase=&quot;&quot; debug=&quot;1&quot;/&gt;

&lt;/Host&gt;

其中：

? name：指定虚拟主机名字，这里为了演示方便，用IP代替。

? debug：指定日志输出级别

? appBase：存放Web应用程序的基本目录，可以是绝对路径或相对于$CATALINA_HOME的目录，默认是$CATALINA_HOME/webapps。

? unpackWARs：如果为true，则tomcat会自动将WAR文件解压后运行，否则不解压而直接从WAR文件中运行应用程序。

? autoDeploy：如果为true，表示Tomcat启动时会自动发布appBase目录下所有的Web应用（包括新加入的Web应用）

? path：表示此Web应用程序的url入口，如为&quot;/jsp&quot;，则请求的URL为 [http://localhost/jsp/](http://localhost/jsp/)

? docBase：指定此Web应用的绝对或相对路径，也可以为WAR文件的路径。

这样tomcat的虚拟主机就创建完成了。

注意：tomcat的虚拟主机一定要和apache配置的虚拟主机指向同一个目录，这里统一指向到/webdata/www目录下，所以接下来只需在/webdata/www中放置jsp程序即可。

在server.xml中，还需要注意的几个标签有：

   &lt;Connector port=&quot;8080&quot; maxHttpHeaderSize=&quot;8192&quot;

maxThreads=&quot;150&quot; minSpareThreads=&quot;25&quot; maxSpareThreads=&quot;75&quot;

enableLookups=&quot;false&quot; redirectPort=&quot;8443&quot; acceptCount=&quot;100&quot; connectionTimeout=&quot;20000&quot; disableUploadTimeout=&quot;true&quot; /&gt;

这是tomcat对http访问协议的设定，http默认的监听端口为8080，在apache和tomcat整合的配置中，是不需要开启tomcat的http监听的，为了安全期间，建议注释掉此标签，关闭http默认的监听端口。

     &lt;Connector port=&quot;8009&quot;

              enableLookups=&quot;false&quot; redirectPort=&quot;8443&quot; protocol=&quot;AJP/1.3&quot; /&gt;

上面这段是tomcat对ajp13协议的设定，ajp13协议默认的监听端口为8009，整合apache和tomcat必须启用该协议，JK模块就是通过ajp协议实现apache和tomcat协调工作的。

（3）配置tomcat启动脚本

Tomcat 的bin目录主要存放各种平台下启动和关闭tomcat的脚本文件，在linux下主要有catalina.sh、startup.sh和 shutdown.sh 3个脚本，而startup.sh和shutdown.sh其实都用不同的参数调用了catalina.sh脚本。

Tomcat在启动的时候会去查找jdk的安装路径，因此，我们需要配置系统环境变量，这里详细讲述下linux下环境变量的设定。

? /etc/profile：是配置系统全局环境变量，系统中所有应用都可以使用这个环境变量。

? ~/.bash_profile：是用户环境变量，每个用户可以通过配置这个文件而设定不同的环境变量。

针对java环境变量的设置，可以在/etc/profile中指定JAVA_HOME，也可以在启动tomcat的用户环境变量.bash_profile中指定JAVA_HOME，这里我们在catalina.sh脚本中指定java环境变量，编辑catalina.sh文件，添加如下内容：

# OS specific support.  $var _must_ be set to either true or false.

JAVA_HOME=/usr/local/jdk1.6.0_07

export JAVA_HOME

cygwin=false

os400=false

上面加粗部分是新加内容，其它为catalina.sh文件原有内容。通过JAVA_HOME指定了JDK的安装路径，然后通过export设置生效。

4．测试apache与tomcat整合

到这里为止，apache与tomcat整合配置已经完毕了，接下来我们通过添加jsp程序来测试整合的结果，看是否达到了预期的效果。

这里我们将/usr/local/tomcat5.5.12/webapps/ROOT/目录下的所有文件拷贝到/webdata/www下，然后启动tomcat与apache服务，执行步骤如下：

[root@localhost~]#cp -r /usr/local/tomcat5.5.12/webapps/ROOT/*  /webdata/www

[root@localhost ~]# /usr/local/tomcat5.5.12/bin/startup.sh

[root@localhost ~]# /usr/local/apache2/bin/apachectl start

启动服务完毕，就可以访问站点了，输入 [http://192.168.60.198](http://192.168.60.198) ，如果能访问到tomcat默认的jsp页面，表示tomcat解析成功，接着，在/webdata/www下建立一个test.html的静态页面，内容如下：

&lt;html&gt;

   &lt;head&gt;

&lt;meta http-equiv=&quot;Content-Type&quot; content=&quot;text/html; charset=iso-8859-1&quot;&gt;

   &lt;title&gt;Administration&lt;/title&gt;

&lt;/head&gt;

&lt;body&gt;

apache and tomcat sussessful,

This is html pages!

&lt;/body&gt;

&lt;/html&gt;

通过访问 [http://192.168.60.198/test.html](http://192.168.60.198/test.html) ，应该出现：

apache and tomcat sussessful ,This is html pages!

则表示静态页面也可以正确解析。

由于tomcat也能处理静态的页面和图片等资源文件，那么如何才能确定这些静态资源文件都是由apache处理了呢，知道这个很重要，因为做apache和tomcat集成的主要原因就是为了实现动、静资源分离处理。

一个小技巧，可以通过apache和tomcat提供的异常信息报错页面的不同来区分这个页面或者文件是被谁处理的，例如输入 [http://192.168.60.198/test.html](http://192.168.60.198/test.html) ，则显示了页面内容，那么随便输入一个网页 [http://192.168.60.198/test1.html](http://192.168.60.198/test1.html) ，服务器上本来是不存在这个页面的，因此会输出报错页面，根据这个报错信息就可以判断页面是被apache或者tomcat处理的。同理，对于图片、js文件和css文件等都可以通过这个方法去验证。