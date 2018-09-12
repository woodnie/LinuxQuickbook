#### /etc/inetd.conf {#etc-inetd-conf}

1 、前言  

Inetd.conf文件是Linux系统中的重要文件之一。它保存了系统提供internet服务的数据库。通过这个文件，你可以对这些服务加以控制，如打开/关闭某项服务，使它们更为安全的运行等等很多。希望这篇文章能尽量解释地完整。  

2、域  

在inetd.conf文件中每项有效的条目中都应该包含以下的域。  

* 服务名  

* 套接字类型  

* 协议类型  

* wait/nowait[.max]  

* 用户名[.组]  

* 服务程序  

* 服务程序的参数  

当然如果你要定义Sun-RPC服务，在inetd.conf文件则需要以下的类型域：  

* 服务名/版本  

* 套接字类型  

* rpc/协议类型  

* wait/nowait[.max]  

* 用户名[.组]  

* 服务程序  

* 服务程序的参数  

服务名是在/etc/services文件中经过定义的有效服务名称(如telnet,echo等)。如果服务被用来定义Sun-RPC服务，它就必须在/etc/rpc文件中定义。  

套接字类型域包含以下几种：  

* stream - stram  

* dgram - datagram  

* raw - raw  

* rdm - reliabl! y delivered message  

* seqpacket - sequenced packet  

此域取决于使用何种的套接字类型.  

协议类型域必须是已经在/etc/protocols文件中定义过的类型。最常见的是tcp和udp,Sun-RPC服务要在协议前加上&quot;rpc/&quot;(如rpc/tcp或者rpc/udp)  

Wait/nowait域只用于数据报套接字，其它的都使用nowait参数。如果服务是多线程的，意味着在与对端建立连接后将释放套接字， inetd进程可以通过些套接字接收更多的消息，这时些用&quot;nowait&quot;条目。如果服务是单线程，表示服务将在同一个socket中处理所有的外来数据报，直到超时，这种情况下使用&quot;wait&quot;条目。Max参数，用一个点与wait/nowait隔开，定义了inetd进程在一分钟之内最大产生的实例数目。  

用户域定义了服务的使用者。组参数，通过点与用户名隔开，定义了除/etc/passwd文件中之外的可以运行服务的组ID。  

服务程序是在套接字请求时执行的程序的完整路径。如果是inted进程内置的服务，此处应为&quot;internally&quot;。  

服务程序参数提供程序运行的所需的参数，同样的，如果是内置服务，此处也为&quot;internally&quot;。  

3、服务  

现在来看一下不同的服务，以便加深理解。  telnet  stream  tcp     ;nowait  root    /usr/sbin/tcpd  in.telnetd  

* 服务名:  telnet  

* 套接字类型:  stream  

* 协议类型:  tcp  

* Wait/Nowait[.max]: nowait  

* 用户名[.组]:  root  

* 服务程序:  /usr/sbin/tcpd  

* 参数:  in.telnetd  

echo  dgram  udp    wait     root  internal  

* 服务名:  echo  

* 套接字类型:  dgram  

* 协议类型:  udp  

* Wait/Nowait[.max]: wait  

* 用户名[.组]:  root  

* 服务程序:  internal  

rstatd/1-3 dgram rpc/udp wait root /usr/sbin/tcpd rpc.rstatd  

* 服务名:  rstatd/1-3  

* 套接字类型:  dgram  

* 协议类型: rpc/udp  

* Wait/Nowait[.max]: wait  

* 用户名[.组]:  root  

* 服务程序:  /usr/sbin/tcpd  

*&amp;nb! sp;参数:  rpc.rstatd  

4、开启&amp; 关闭 服务  

非常简单，只要在想要关闭的服务前面加上一个#，比如想要关闭23端囗，被telnet使用，只要象下面这样。  

#telnet  stream  tcp    nowait  root    /usr/sbin/tcpd  in.telnetd  

这时，telnet服务已经关闭了，以后，如果我想让朋友通过telnet访问我的计算机，我只需要把#去掉，就象这样。  

telnet  stream  tcp    nowait  root    /usr/sbin/tcpd  in.telnetd  

这时，telnet服务又被开启，就是这么简单。重新启动inetd进程让改动生效，用下面的命令。  

james:~ # killall -HUP inetd  

5、守护进程  

有时候在服务程序参数域中，你会看到一些选项，如：  

smtp stream  tcp     nowait  root    /usr/sbin/sendmail    sendmail -bs  

在上一行的末尾，有&quot;-bs&quot;! ，表明使用b和s参数，这同使用下面的命令有着同样的效果：  

hoodl um:~ # sendmail -bs  

因此，如果你想为守护进程使用某项参数，只要把它们加入到服务程序参数域就可以了。具体的参数可以通过man进行查询。  

6、TCP Wrappers  

TCP Wrappers是保护网络服务的应用，通常用在第6列-服务程序域。  

telnet  stream  tcp    nowait  root    /usr/sbin/tcpd  in.telnetd  

TCP Wrappers使用两个文件，/etc/hosts.allow和/etc/hosts.deny，限制某项服务的使用。Hosts.allow文件内是允许访问服务的主机列表，hosts.deny内含禁止访问服务的主机。  

7、结论  

从网上翻来的，如果对您有帮助，我很高兴。