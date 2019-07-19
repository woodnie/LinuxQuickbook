### yum {#yum}

yum 命令的使用方法

1.yum命令简单介绍

yum(yellowdog updater modified)的理念是使用一个中心仓库(repository)管理一部分甚至一个distribution的应用程序相互关系，根据计算出来的软件依赖关系进行相关的升级、安装、删除等等操作，减少了Linux用户头痛的dependencies的问题。

yum通过一个或者多个配置文件描述对应的repository的网络地址，通过http或者ftp协议在需要的时候从repository获得必要的信息，下载相关的软件包。这样，本地用户通过建立不同的repository的描述说明，在有网络连接时就能方便进行系统的升级维护工作

主要功能是：

1).更方便的添加/删除/更新RPM包.

2).能自动解决包的倚赖性问题.

3).能便于管理大量系统的更新问题

4).此命令主要集中于rhel系列的linux系统中

5).ubuntu等的系统一般使用的是apt命令

2.yum简要特点

1).可以同时配置多个资源库(Repository)

2).简洁的配置文件(/etc/yum.conf)

3).自动解决增加或删除rpm包时遇到的倚赖性问题

4).使用方便

5).保持与RPM数据库的一致性

3.yum工具安装

[root@node ~]# rpm -ivh yum***********.rpm

4.主要命令

[root@node ~]# yum install packagename

[root@node ~]# yum remove  packagename

[root@node ~]# yum update   packagename

[root@node ~]# yum list available     #列举以安装的软件包

[root@node ~]# yum list installed      #列举已安装的软件包

5.yum一些常用命令解释及实例：

5.1系统中rpm包列表显示

[root@node ~]# yum list                     #列出资源库中所有可以安装或更新的rpm包

[root@node ~]# yum list mozilla        #列出资源库中特定的可以安装或更新以及已经安装的rpm包

[root@node ~]# yum list mozilla*       #可以在rpm包名中使用匹配符，如列出所有以mozilla开头的rpm包

[root@node ~]# yum list *mozilla*    #搜索任何包含mozilla字符的包

[root@node ~]# yum list updates      #列出资源库中所有可以更新的rpm包

[root@node ~]# yum list installed      #列出已经安装的所有的rpm包

[root@node ~]# yum list extras          #列出已经安装的但是不包含在资源库中的rpm包（注:通过其它网站下载安装的rpm包）

[root@node ~]# yum info       #

5.2查找搜索系统rpm包

[root@node ~]# yum search mozilla         #搜索匹配特定字符的rpm包  注:在rpm包名,包描述等中搜索

[root@node ~]# yum provides realplay     #搜索有包含特定文件名的rpm包

5.3rpm包的安装

[root@node ~]# yum install mozilla          #安装rpm包，同时自动安装其所依赖的软件包            

[root@node ~]# yum install xmms-mp3   #安装rpm包,使xmms可以播放mp3                                                    

[root@node ~]# yum install mplayer         #安装mplayer,同时自动安装相关的软件

5.4rpm包的删除

[root@node ~]# yum remove licq           #删除licq包,同时删除与该包有倚赖性的包   注:同时会提示删除licq-gnome,licq-qt,licq-text,非常方便

[root@node ~]# yum remove mozilla     # 删除rpm包，同时删除倚赖于该包所有的软件包

5.5rpm包的更新

[root@node ~]# yum check-update     #检查可更新的rpm包   检查有哪些可更新的rpm包

[root@node ~]# yum -y update             #系统更新(更新所有可以升级的rpm包,包括kernel)

[root@node ~]# yum update kernel kernel-source  #更新指定的rpm包,如更新kernel和kernel source  

[root@node ~]# yum upgrade                                    #大规模的版本升级,与yum update不同的是,连旧的淘汰的包也升级

5.6yum缓存(/var/cache/yum/)的相关参数

对于经常应用yum命令的服务器缓存里面的数据会越来越多可以用下面的命令清空缓存文件以释放一些硬盘空间

[root@node ~]# yum clean packages              #清除暂存中rpm包文件  清除暂存中rpm包文件

[root@node ~]# yum clean headers                 #清除暂存中rpm头文件

[root@node ~]# yum clean oldheaders            #清除暂存中旧的rpm头文件

[root@node ~]# yum clean      或

[root@node ~]# yum clean all   #清除暂存中旧的rpm头文件和包文件  (注:相当于yum clean packages + yum clean oldheaders)

注意：在您正确配置了repository及yum客户端后，使用yum时，如果报&quot;&quot;这样的错误，很可能是您客户端yum缓存造成的。

           执行yum clean all后，一般就可以解决。

6.安全的更新rpm包

[root@node ~]# rpm --import [http://ftp.sjtu.edu.cn/centos/5/os/i386/RPM-GPG-KEY-CentOS-5](http://ftp.sjtu.edu.cn/centos/5/os/i386/RPM-GPG-KEY-CentOS-5)        # 安装GPG key

[root@node ~]# rpm -qa gpg-pubkey*                                     #检查GPG Key

[root@node ~]# rpm -qi gpg-pubkey-e42d547b-3960bdf1   #显示Key信息

[root@node ~]# rpm -e gpg-pubkey-e42d547b-3960bdf1    #删除Key