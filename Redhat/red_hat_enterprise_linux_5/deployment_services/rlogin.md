### rlogin {#rlogin}

远程用户启动rlogin访问你的本地主机，此时做如下安全性检查：

　　1\. 本地rlogind在本地/etc/passwd文件中寻找远程用户名，没有则拒绝访问

　　2\. /etc/passwd中存在远程用户名，rlogind在/etc/hosts.equiv寻找远程主机名，找到则允许访问。

　　3\. /etc/hosts.equiv无远程主机名，rlogind在$HOME/.rhosts寻找远程主机名，找到且该项后无用户名则允许访问，找到且该项后有用户名，若远程用户名位于其中，则允许访问。注意这里的$HOME是与远程用户同名的本机用户的主目录。

　　4\. 若通过了/etc/passwd的检查，但没有通过/etc/hosts.equiv或者$HOME/.rhosts的检查，远程用户给出口令可以登录本机，但无法使用rcp、rsh等。反之则可以使用rcp或者rsh。

　　5\. /etc/hosts.equiv中的+意味着任意主机，此时与/etc/hosts无关。netterm下rlogin除root外的所有用户可以成功，solaris下简单的rlogin hostname同netterm，但是rlogin -l username hostname不成功。这个事实说明netterm下rlogin的时候，指定的参数实际上是远程主机的当前用户名。还说明hosts.equiv文件不支持rlogin -l username hostname，不支持root的rlogin。

　　值得注意的是两个+号出现在$HOME/.rhosts中是非常危险的，意味着任意主机任意用户都可以不用口令登录到你的用户上来。.rhosts中的第一个+意味着所有主机，与 /etc/hosts文件无关。.rhosts中的第二个+意味着所有用户，与/etc/passwd文件无关。若只有一个+，netterm中的 rlogin可以成功，但来自sco root下的rlogin -l jhli hostname不成功，来自sco jhli的rlogin -l jhli hostname和rlogin hostname都成功。说明当.rhosts文件中只有主机名时，不支持rlogin -l username hostname，这点和/etc/hosts.equiv一样。若+ root，则netterm因无法实现rlogin -l username hostname而失败(只能实现rlogin hostname)，来自sco root下的rlogin -l jhli hostname成功。+ +则都成功。注意，$HOME/.rhosts支持root的rlogin。

　　当这两个文件中没有使用+号代表所有主机时，与/etc/hosts文件有关。这两个文件中指定的hostname必须是/etc/hosts文件中出现的，而且必须是主机名，IP 地址和别名无效。在/etc/hosts文件中别名位于第三列。做主机判断时与在远程主机上hostname命令所显示的东西无关，与远程主机上的 /etc/hosts定义无关。如果出现与这里讲述不符合的，是因为缓冲没有得到刷新的缘故，可以通过ping相关名字得到检验。此外，若没有使用+ 号，rlogin localhost、rlogin 127.0.0.1的效果和rlogin 134.*.*.*、rlogin zhuzhou的效果不一样，前者的检查不会通过。当hosts.equiv中定义为localhost时，效果则反过来。

　　6\. rlogin -l username hostname和rlogin hostname的检查有点细微的差别，首先在/etc/passwd的检查中是以-l的参数为准的，没有-l参数才使用远程主机的当前用户名，所谓当前用户名是考虑了su出来的用户。其次，$HOME也是先以-l参数为准。第三，$HOME/.rhosts文件中的用户名不是针对-l参数来的，而是针对远程主机的当前用户名来的，但是对于这个远程主机的当前用户名，并不要求出现在/etc/passwd文件中。这第6条尤其重要，许多不成功的$HOME /.rhost就是因为对第6条的不了解。

　　7\. $HOME/.rhosts文件的权限问题，chmod 0都没有影响，不过当时是处理root用户。对于一般用户，一定要保证.rhosts文件对于$HOME的属主是可读的。一般来说，这个文件最好 chmod 400。对于root，如果不希望某个用户建立自己的.rhosts或者想替该用户建立.rhosts后不允许该用户自己修改，可以通过chown root .rhosts然后chmod 444 .rhosts实现。除了root，如果$HOME/.rhosts文件的属主和$HOME的属主不一致，检查将失败。

　　8\. $HOME/.rhosts文件中每行只跟一个用户名，若想多个指定，只好分多行指定用户名。

　　9\. sco unix下$HOME/.rhosts中若用+代表主机，与其他Unix系统不同，这里的+没有任何特殊的意义，所以检查将失败。但是用户名仍然可以用+代表。并且sco unix下若root的.rhosts文件go+w，则检查失败。

　　10\. 建议man rhosts看看，各个系统有许多细微的差别。

hosts.equiv文件

转载一篇文章 [http://blog.csdn.net/opl001/archive/2007/05/15/1609810.aspx](http://blog.csdn.net/opl001/archive/2007/05/15/1609810.aspx) ，感觉这篇文章的内容很有用，收录下来以备检索。

一、hosts.equiv文件的用途

hosts.equiv文件是为了便于远程主机在本地计算机上执行远程命令而设计的。

/etc/hosts.equiv和$HOME/.rhosts定义了哪些计算机和用户可以不用提供口令就在本地计算机上执行远程命令，如rexec, rcp, rlogin等等。这些不需要提供口令的计算机和用户称为受信任的。

当本地计算机收到执行远程命令的请求时，相应的远程命令服务进程，如rlogind，首先检查/etc/hosts.equiv来确认请求是否来自受信任的计算机和用户。如果这个文件不存在或者虽然存在但不包括相应的计算机和用户，服务进程就会去检查$HOME/.rhosts文件。

/etc/hosts.equiv的权限必须设置为只有root能够写，建议权限为600。如果这个文件被设置为同组或其它用户可写，远程命令服务进程就会忽略它的存在。

如果远程命令是由root用户发起的，远程命令服务进程会忽略/etc/hosts.equiv文件的存在而去直接检查/.rhosts文件。

在指定受信任的计算机和用户时要非常小心，因为这有可能会造成安全漏洞。

二、hosts.equiv文件的格式

添加对计算机/用户的信任：

hostname: 信任计算机hostname上的所有普通用户

hostname username：信任计算机hostname上的用户username

+：信任所有计算机上的所有普通用户

禁止对计算机/用户的信任：

如果计算机名和用户都没有在/etc/hosts.equiv中被定义为受信任的，那么它们就是不受信任的。另外，您还可以用以下方法明确地禁止对计算机/用户的信任。

-hostname：不信任计算机hostname上的所有用户

hostname -username: 不信任计算机hostname上的用户username

hosts.equiv与NIS：

在/etc/hosts.equiv中也可以指定是否信任NIS网络组（NETGROUP）。

+@netgroup: 信任网络组netgroup中的所有计算机

-@netgroup：禁止信任网络组netgroup中的所有计算机

hostname +@netgroup: 信任来自计算机hostname的所有网络组netgroup的成员用户的请求

hostname -@netgroup: 禁止信任来自计算机hostname的所有网络组netgroup的成员用户的请求

/etc/hosts.equiv中记录的顺序：

在/etc/hosts.equiv文件中，记录的顺序十分重要。远程命令服务进程在检查/etc/hosts.equiv文件时会在第一个匹配发现后返回，也就是说，下面这个例子中的禁止信任记录是不起作用的：

hostname

hostname -user1

计算机hostname上的用户user1将能够在不提供口令的情况下在本地计算机上执行远程命令。而下面这个例子能够提供期望中的结果：

hostname -user1

hostname

三、/etc/hosts.equiv示例

1、允许远程计算机emerald 和 amethyst 上的所有用户在本地执行远程命令而无须提供口令：

emerald

amethyst

2、允许远程计算机emerald 上的所有用户和 amethyst 上的用户 greygory 在本地执行远程命令而无须提供口令：

emerald

amethyst gregory

3、允许用户 peter 从任何远程计算机在本地执行远程命令而无须提供口令：

emerald

amethyst gregory

+ peter

4、允许所有是 century 网络组成员的远程计算机上的所有用户在本地执行远程命令而无须提供口令：

emerald

amethyst gregory

+ peter

+@century

5、允许所有在计算机citrine上又是 engineers 网络组成员的用户在本地执行远程命令而无须提供口令：

emerald

amethyst gregory

+ peter

+@century

citrine +@engineers

6、 允许所有是 servers 网络组成员的远程计算机上的所有属于 sysadmins 网络组的用户在本地执行远程命令而无须提供口令：

emerald

amethyst gregory

+ peter

+@century

citrine +@engineers

+++++

每一个远程机器都有一个文件（/etc/hosts.equiv），包括了一个信任主机名集共享用户名的列表。本地用户名和远程用户名相同的用户，可以在 /etc/hosts.equiv 文件中列出的任何机器上登录到远程主机，而不需要密码口令。个人用户可以在主目录下设置相似的个人文件（通常叫 .rhosts）。此文件中的每一行都包含了两个名字 - 主机名和用户名，两者用空格分开。.rhosts 文件中的每一行允许一个登录到主机名的名为用户名的用户无需密码就可以登陆到远程主机。如果在远程机的 /etc/hosts.equiv 文件中找不到本地主机名，并且在远程用户的 .rhosts 文件中找不到本地用户名和主机名时，远程机就会提示密码。列在 /etc/hosts.equiv 和 .rhosts 文件中的主机名必须是列在主机数据库中的正式主机名，昵称均不许使用。为安全起见，.rhosts 文件必须归远程用户或根所有。

　　远程终端类型和本地终端类型（在 TERM 变量环境中给定）相同。如果服务器支持，终端或窗口尺寸会被拷贝到远程系统中，同时大小的变化也能反映出来。所有的回声现象都发生在远程站点，以致于远程登录都是透明的（除了延迟情况）。流控制借助 &lt;CTRL-S&gt; 和 &lt;CTRL-Q&gt; 实现，并且输入输出中断也得到很好的处理。

编辑本段命令

　　rlogin 的安全版本 slogin 和另外两个 UNIX 工具 ssh 、scp 都属于 Secure Shell suite，这是一个用于替代早期工具的界面和协议。

　　协议结构

　　rlogin 命令是：rlogin [-8EL] [-ec ] [-l 用户名] 主机名可选标记

　　可选标记：

　　-8EL 总是支持8位数据路径，除非开始字符和停止字符不是 Ctrl-S 和 Ctrl-Q，否则 Rlogin 命令使用7位数据路径，去除校验位。

　　-e Character 改变换码符。替换为 Character 选择的字符。

　　-f 引起认证（Credentials）转发。如果当前不是采用的 Kerberos 5认证方法，该标记就会被忽略。如果当前认证没有标记为可转发，将导致认证失败。

　　-F 引起认证（Credentials）转发。另外远程系统上的认证被标记为可转发（允许它们传送到另一个远程系统上）。如果当前认证没有标记为可转发，将导致认证失败。

　　-k realm 如果远程站领域与本地系统领域不同，那么允许用户指定远程站的领域。为达到该目的，领域与DCE信元同义。如果当前不是采用的 Kerberos 5认证方法，该标记就会被忽略。

　　-l User 将远程用户名改为你所指定的名字。否则将你的本地用户名应用于远程主机。

　　主机名-建立远程登录会话的远程机器

　　远程登录是Internet上较早提供的服务。用户通过Telnet命令使自己的计算机暂时成为远地计算机的终端，直接调用远地计算机的资源和服务。利用远程登录，用户可以实时使用远地计算机上对外开放的全部资源，可以查询数据库、检索资料，或利用远程计算完成只有巨型机才能做的工作。此外， Internet的许多服务是通过Telnet访问来实现的。

　　与远程计算机连接后，可实现资源共享。

编辑本段软件方式实现远程登录

　　目前鉴于用户对于远程登录的需求和用户偏于应用型的特征，很多软件公司研发了远程共享和远程控制软件来实现远程登录，比如像国外的Mikogo，Teamviewer，Netviewer、pcanywhere,以及国内的网络人远程控制软件、花生壳等，而Windows系统远程桌面也能实现相似的操作。

　　通过软件方式实现远程登录，首先需要安装远程登录软件，多数情况需要两个甚至多个终端同时安装。其次，正常情况下服务器可以做到24小时工作，但常常禁止安装此类远程控制软件，这就意味着可能在利用软件进行远程登录公司服务器的时候需要同时开启两台机器和一台服务器，所以常常在移动办公中，需要同时开启公司电脑以作为中转，通过远程操控公司电脑实现远程登录。现在远程控制软件已经日渐专业化、品牌化，在操作性和安全性方面都有很大的进步，网络人等远程控制还采用U盾加密来保护远程控制的安全。使用软件进行登录的优点就是简单易学，操作方便，体积小，绿色，响应速度快，画质清晰等特点，比如像Mikogo本身只有1.6MB, 国产的网络人远程控制软件也只是1M左右，适用于着重应用的用户。

　　远程登录逐渐被人们认识和广泛地应用于学习，工作和生活中，更多、更方便的远程登录方式也会慢慢丰富市场，满足各种不同阶层、不同目的的用户需求。