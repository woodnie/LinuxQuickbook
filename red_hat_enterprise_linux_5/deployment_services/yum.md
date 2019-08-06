### yum {#yum}

配置yum

pup  #图形化跟新程序

priut #图形化查看，安装，删除

1.准备YUM源的安装环境，安装VSFTPD服务或HTTPD服务，这里以VSFTPD服务为例。

2.将RHEL5光盘内的所有软件包复制到/var/ftp/pu

在RHEL5的任何一处版本的光盘中都有以下几个软件包分组目录

Server：常用软件包和应用软件包

VT：虚拟化软件包

Cluster：集群软件包

ClusterStorage：集群存储的软件包

3.重建光盘软件包的软件依赖性

在每一个软件包分组的目录内都有一个repodata的目录，该目录内有特定的文件，如下所示：

[root@localhost ~]# cd /var/ftp/pub/Server/repodata/       ##Server,VT,Cluster,ClusterStorage等目录中都存在此类文件

[root@localhost repodata]# ls

comps-rhel5-server-core.xml  filelists.xml.gz  other.xml.gz  primary.xml.gz  repomd.xml

文件的具体功能如下：

文件repomd.xml功能：包含每一个分组的软件包的时间值，软件包的加密码验证信息等

文件primary.xml.gz功能：包含每一个分组中的软件包的依赖性信息

文件filelists.xml.gz功能：包含每一个分组中的软件包的详细信息

文件other.xml.gz功能：包含每一组软件包的其他信息，如软件包更新日志

文件comps-rhel5-vt.xml功能：包含每一个软件包的分组信息，及对组内的软件包优化参

[root@localhost Server]# rpm -vih createrepo-0.4.4-2.fc6.noarch.rpm    ##createrepo软件包提供creatrepo命令帮助创建包依赖关系

重建Server软件组的依赖性关系如下：

[root@localhost ]# createrepo -g /var/ftp/pub/Server/repodata/comps-rhel5-server-core.xml /var/ftp/pub/Server

注：(如果创建过程中出现&quot;/var/ftp/pub/Server/repodata/.olddata&quot;目录已经存在请删除这个目录重新创建一次就可以了.)

重建VT软件组的依赖性关系如下：

[root@localhost ]# createrepo -g /var/ftp/pub/VT/repodata/comps-rhel5-vt.xml /var/ftp/pub/VT/

重建Cluster软件组的依赖性关系如下：

[root@localhost ]#createrepo -g /var/ftp/pub/Cluster/repodata/comps-rhel5-cluster.xml  /var/ftp/pub/Cluster

重建ClusterStorage软件组的依赖性关系如下：

[root@localhost ]# createrepo -g /var/ftp/pub/ClusterStorage/repodata/comps-rhel5-cluster-st.xml /var/ftp/pub/ClusterStorage/

4.建立YUM软件仓库的配置文件：

进入软件仓库配置文件目录建立repo配置文件

[root@localhost ~]# vi /etc/yum.repo.d/yum.repo

[root@localhost ~]# vi /etc/yum.repo.d/yum.repo  

[Server]                                  #yun仓库名,可有多个不同命仓库

name=Red Hat Enterprise Linux Server      #yum仓据描述

baseurl=                                  #yum仓库源路径

enabled=1                                 #是否启用该仓库：0禁用，1启用

gpgcheck=1                                #是否启用GPG认证，即检查是否为

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release  ##GPG认证文件路径

在同一个系统中只能使用一种YUM源的安装方式：( http:// ftp:// file:// )

baseurl=file:///var/ftp/pub/Server

baseurl=ftp://192.168.0.254/pub/Server

baseurl=http://192.168.0.254/Server

5.一个YUM仓库配置文件的例子

[base]

name=Red Hat Enterprise Linux

baseurl=ftp://127.0.0.1/pub/Server

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

#YUM repo for RHEL 5

[local-Server]

name=local-Server

baseurl=file:///media/Server

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[local-VT]

name=local-VT

baseurl=file:///media/VT

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[local-Cluster]

name=local-Cluster

baseurl=file:///media/Cluster

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[local-ClusterStorage]

name=local-ClusterStorage

baseurl=file:///media/ClusterStorage

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

#YUM repo for RHEL 6

[local-Server]

name=local-Server

baseurl=file:///media/Server

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[local-HA]

name=local-HA

baseurl=file:///media/HighAvailability

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[local-LB]

name=local-LB

baseurl=file:///media/LoadBalancer

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[local-RS]

name=local-RS

baseurl=file:///media/ResilientStorage

enabled=1

gpgcheck=1

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[root@node ~]# cat /etc/yum.conf

[main]

cachedir=/var/cache/yum

keepcache=0

debuglevel=2

logfile=/var/log/yum.log

distroverpkg=redhat-release

tolerant=1

exactarch=1

obsoletes=1

gpgcheck=1

plugins=1

声明：:当第一次使用yum或yum资源库有更新时,yum会自动下载所有所需的headers放置于/var/cache/yum目录下,所需根据本地网络的速度和源的速度而定.

###########################

mkdir /media/CentOS/

2 mount /dev/cdrom /media/CentOS/

3

4 yum -y --disablerepo=\* --enablerepo=c5-media install gcc gcc-c++ autoconf \

5 libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 \

6 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 \

7 bzip2-devel  ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel \

8 krb5 krb5-devel libidn libidn-devel openssl openssl-devel libtool  libtool-libs \

9 libevent-devel libevent openldap openldap-devel nss_ldap openldap-clients \

10 openldap-servers libtool-ltdl libtool-ltdl-devel bison