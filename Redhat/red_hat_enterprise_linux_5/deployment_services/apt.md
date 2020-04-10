### apt {#apt}

配置apt

内容简介

如何用APT(Advanced Packaging Tool)维护Red Hat Enterprise Linux (RHEL)，提供对个别RHEL的用户无法升级的问题的解决办法。关键词：APT,Linux，升级，Red Hat Enterprise Linux，RHEL，YUM，RPM，依赖性

几句前言

Linux系统维护中令管理员很头疼的就是软件包之间的依赖性了，往往是你要安装A软件，但是编译的时候告诉你X软件安装之前需要B软件，而当你安装Y软件的时候，又告诉你需要Z库了--好不容易安装好Z库，发现版本还有问题......可能很多朋友都有过这个经历。其实开源社区早就对这个问题尝试进行解决了，不同的发行版推出了各自的工具，比如Yellow Dog的YUM ,Debian的APT(Advanced Packaging Tool)等。而这些软件也被开源软件爱好者们逐渐移植到别的发行版上。

Redhat企业版Linux的的升级往往给管理员们带来不少问题：网站下载速度太慢，不够安全，当然了，更多的人是无法更新的--版权问题。经过一段时间的比较 ，感觉使用APT维护RHEL有着特殊的便利性。 (有的朋友可能会说，yum 也不错阿！是的，yum在很多时候表现的确不错，不过如果使用的Linux是RHEL的话,很难找到适合yum的资料库，&quot;巧妇难为无米之炊&quot;。) 现在把具体方法介绍给大家作为参考。

APT基本介绍

Debian GNU/Linux 是APT的缔造者。初衷是利用工具来解决软件安装时候的依赖性问题。其工作原理大致为：用户安装APT客户端工具，查寻APT服务器端的资料库（repositories）上的RPM软件包信息，并分析软件包之间的依赖性然后下载并进行安装。

安装与配置

首先让我们安装APT工具:

   # wget [http://redhat.uni-klu.ac.at/el3/apt.i386.rpm](http://redhat.uni-klu.ac.at/el3/apt.i386.rpm)

   # rpm -Uvh apt.i386.rpm

安装够简单吧? 我们要编辑配置文件：

   #vi /etc/apt/sources.list.d/dag.list  

添加如下内容（资料库相关的信息）:

   rpm [http://afs.caspur.it/](http://afs.caspur.it/) afs/italia/project/linux/cern/slc302/i386/apt os updates extras

   rpm [http://redhat.uni-klu.ac.at](http://redhat.uni-klu.ac.at) redhat/dag/el3/i386 dag

   rpm-src [http://redhat.uni-klu.ac.at](http://redhat.uni-klu.ac.at) redhat/dag/el3/i386 dag

   rpm [http://apt.sw.be](http://apt.sw.be) redhat/el3/en/i386 dag

   rpm-src [http://apt.sw.be](http://apt.sw.be) redhat/el3/en/i386 dag

注: 第一条 [http://afs.caspur.it/](http://afs.caspur.it/) 的资料库几乎就是Redhat官方站点的内容。在写这篇文章的时候还是有效的。如果要尝试更新Kernel，还可以在第一条后面添加　kernel26 .

如果需要更多Java相关软件，则：

   #vi /etc/apt/sources.list.d/jpackage.list  

(这一步是可选的)添加如下内容:

   rpm [http://redhat.uni-klu.ac.at](http://redhat.uni-klu.ac.at) redhat/jpackage/redhat-es-3/i386 free devel

   rpm-src [http://redhat.uni-klu.ac.at](http://redhat.uni-klu.ac.at) redhat/jpackage/redhat-es-3/i386 free devel

   rpm [http://redhat.uni-klu.ac.at](http://redhat.uni-klu.ac.at) redhat/jpackage/redhat-es-3/generic free devel

   rpm-src [http://redhat.uni-klu.ac.at](http://redhat.uni-klu.ac.at) redhat/jpackage/redhat-es-3/generic free devel

如果要更新KDE的话(这一步可选的):

   #vi /etc/apt/sources.list.d/kde.list  

考虑添加如何内容:

   rpm [http://apt.kde-redhat.org](http://apt.kde-redhat.org) apt/fedora/3.0 stable

   rpm [http://apt.kde-redhat.org](http://apt.kde-redhat.org) apt/fedora/all stable

   rpm [http://apt.kde-redhat.org](http://apt.kde-redhat.org) apt/kde-redhat/3.0 stable unstable

   rpm [http://apt.kde-redhat.org](http://apt.kde-redhat.org) apt/kde-redhat/all stable unstable

当然，这些内容是经过笔者验证的，都是可用的。从一些站点上下载的list 似乎都多多少少有点问题。

注: 如果您发现上述的资料库失效或者是有什么更好的资料库。烦请通知我: DBAnotes@gmail.com .

使用简介

使用相对来说比较简单:

   #apt-get update

   #apt-get upgrade

   #apt-get check  //检查依赖性

   #apt-get -f install // 解决依赖性问题  

如果要安装某工具，比如说iftop,可以这样:

   #apt-cache search iftop

   #apt-get install iftop  

apt自动解决依赖性问题，方便得很。

要注意的是需要导入相应资料库的签名。在相关站点下载GPG key之后，用如下命令导入即可:

   #rpm --import TheKey_youDownload

如果有耐心看到这里的话，可以发现盗版的用户或者是用RHEL进行测试的朋友可以通过这个进行升级了--要不然RHEL的up2date 总是要你输入认证信息的。