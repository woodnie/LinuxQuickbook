### snmp {#snmp}

安装snmp服务

CentOS/RedHat下可以只用yum命令进行安装。

$  yum -y install net-snmp net-snmp-devel

若要使用snmpwalk进行安装检测，则还需要安装net-snmp-utils包

$  yum -y install net-snmp-utils

Ubuntu可以通过apt-get install snmp snmpd 进行安装

设置安全的验证方式

将SNMP代理程序暴露给网络上的所有主机是很危险的，为了防止其它主机访问您的SNMP代理程序，我们需要在SNMP代理程序上加入身份验证机制。 SNMP支持不同的验证机制，这取决于不同的SNMP协议版本，云监控目前支持v2c和v3两个版本，其中v2c版本的验证机制比较简单，它基于明文密码和授权IP来进行身份验证，而v3版本则通过用户名和密码的加密传输来实现身份验证，我们建议使用v3，当然，只要按照以下的介绍进行配置，不论是v2c 版本还是v3版本，都可以保证一定的安全性，您可以根据情况来选择。

注意一点，SNMP协议版本和SNMP代理程序版本是两回事，刚才说的v2c和v3是指SNMP协议的版本，而Net-SNMP是用来实现SNMP协议的程序套件，目前它的最新版本是刚才提到的5.4.2.1。

v2c

先来看如何配置v2c版本的SNMP代理，我们来创建snmpd的配置文件，默认情况下它是不存在的，我们来创建它，如下：

sdo:~ # vi /usr/local/snmp/share/snmp/snmpd.conf

然后我们需要创建一个只读帐号，也就是read-only community，在snmpd.conf中添加以下内容：

rocommunity sdomonitor 114.80.132.9 rocommunity sdomonitor 58.215.169.26 rocommunity sdomonitor 58.215.169.27

如果想要检测服务是否成功开启，则还需要在snmpd.conf中添加：

rocommunity sdomonitor 127.0.0.1

注意，这里的&quot;rocommunity&quot;表示这是一个只读的访问权限，云监控只可以从您的服务器上获取信息，而不能对服务器进行任何设置。

紧接着的&quot;sdomonitor&quot;相当于密码，很多平台喜欢使用&quot;public&quot;这个默认字符串。这里的&quot;sdomonitor&quot;只是一个例子，您可以设置其它字符串作为密码。

最右边的&quot;60.195.249.83&quot;代表指定的监控点IP，这个IP地址是云监控专用的监控点，这意味着只有云监控有权限来访问您的SNMP代理程序。

所以，以上这段配置中，只有&quot;sdomonitor&quot;是需要您进行修改的，同时在云监控上添加服务器的时候，需要提供这个字符串。

v3

当然，我们建议您使用v3版本来进行身份验证。对于一些早期版本的Linux分发版，其内置的SNMP代理程序可能并不支持v3，所以我们建议您按照前边介绍的方法，编译和安装最新的Net-Snmp。

v3支持另一种验证方式，需要创建一个v3的帐号，我们同样修改以下配置文件：

sdo:~ # vi /usr/local/snmp/share/snmp/snmpd.conf

然后添加一个只读帐号，如下：

rouser sdomonitor auth

可以看到，在v3中，&quot;rouser&quot;用于表示只读帐号类型，随后的&quot;sdomonitor&quot;是指定的用户名，后边的&quot;auth&quot;指明需要验证。

接下来，我们还要添加&quot;sdomonitor&quot;这个用户，这就是v3中的特殊机制，我们打开以下配置文件：

sdo:~ # vi /var/net-snmp/snmpd.conf

这个文件会在snmpd启动的时候被自动调用，我们需要在它里边添加创建用户的指令，如下：

createUser sdomonitor MD5 mypassword

这行配置的意思是创建一个名为&quot;sdomonitor&quot;的用户，密码为&quot;mypassword&quot;，并且用MD5进行加密传输。这里要提醒的是：

密码至少要有8个字节

这是SNMP协议的规定，如果小于8个字节，通信将无法进行。

值得注意的是，一旦snmpd启动后，出于安全考虑，以上这行配置会被snmpd自动删除，当然，snmpd会将这些配置以密文的形式记录在其它文件中，重新启动snmpd是不需要再次添加这些配置的，除非您希望创建新的用户。

以上配置中的用户名、密码和加密方式，在云监控添加服务器的时候需要添加。

启动snmp服务

$ service snmpd start

用以下命令检查服务是否启动成功

$ snmpwalk -v 2c -c sdomonitor 127.0.0.1 system

如果要关闭，则可以直接kill这个进程，如下：

$ killall -9 snmpd 或者$ service snmpd stop

增强的安全机制

您可以在安全组中设置，将UDP161端口只开放给云监控的3台测试机ip 114.80.132.9,58.215.169.26,58.215.169.27，而不是开放给所有网络

具体方法是：

在云主机界面登陆控制台，选择安全组，选择高级设置，选择custom UDP，161端口，源ip输入上述3个ip，点击添加规则，提交之后生效并且能够在右侧看到相关UDP的信息。