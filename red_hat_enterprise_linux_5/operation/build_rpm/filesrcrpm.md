#### file.src.rpm {#file-src-rpm}

原文： [http://fedora.linuxsir.org/main/?q=src.spec.html](http://fedora.linuxsir.org/main/?q=src.spec.html)

提要：

初学Linux的弟兄可能会看到file.src.rpm 格式的软件包，但不知道如何使用；现简单介绍一下，太详细的我也不太知道；我毕竟不是计算机科班出身的，简单一点的应用还算过得去，更深一点的，只能靠弟兄们了~

本文主要介绍file.src.rpm和file.spec 的简单用法；

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

正文：

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

源码包file.tar.gz、file.tar.bz2 或 file.src.rpm 包都可以编译出我们可安装执行的file.rpm包；有些软件开发者自己会写把软件包的源码包打包成file.src.rpm file.tar.gz file.tar.gz 等，也会写可执行file.rpm 编译的脚本文件file.spec；

如果我们技术可行，也是可以自己来写编译脚本file.spec文件；通过file.spec 文件，我们可以分享其它弟兄；也可以从他人那里得到file.spec 文件，然后我们自己修改修改，就可以编译出与自己系统适合的rpm 包；

一、通过file.src.rpm和file.spec 编译rpm 包为我所用；

我们在Fedora/Redhat或者其它基于RPM包管理的系统，可以看到 file.src.rpm 和file.rpm ；file.src.rpm 是源码包的rpm格式；我们也可以安装它，安装后出现在 /usr/src/redhat/SOURCE的目录；

举例：比如我们要用unrar-3.5.2-1.2.fc4.src.rpm和unrar.spec来编译出rpm包；

首先我们下载两个文件：unrar-3.5.2-1.2.fc4.src.rpm和unrar.spec

#wget [http://ftp.freshrpms.net/pub/freshrpms/fedora/linux/4/unrar/unrar-3.5.2-1.2.fc4.src.rpm](http://ftp.freshrpms.net/pub/freshrpms/fedora/linux/4/unrar/unrar-3.5.2-1.2.fc4.src.rpm)

# wget [http://svn.rpmforge.net/svn/trunk/rpms/unrar/unrar.spec](http://svn.rpmforge.net/svn/trunk/rpms/unrar/unrar.spec)

下载完成：

[root@localhost beinan]# ls unrar*

unrar-3.5.2-1.2.fc4.src.rpm  unrar.spec

安装源码包unrar-3.5.2-1.2.fc4.src.rpm；看一看安装在哪了呢？

[root@localhost beinan]# rpm -ivh unrar-3.5.2-1.2.fc4.src.rpm

安装在这里：

[root@localhost beinan]# ls /usr/src/redhat/SOURCES/unrar

unrar.1                unrarsrc-3.5.2.tar.gz

然后我们通过unrar.spec 来执行，其实他是一个写好的编译脚本；

[root@localhost beinan]# rpmbuild --ba unrar.spec

编译完成：

[root@localhost beinan]# ls /usr/src/redhat/RPMS/i386/

unrar-3.5.2-1.i386.rpm  unrar-debuginfo-3.5.2-1.i386.rpm

是不是可以安装呢？

[root@localhost beinan]# rpm -ivh /usr/src/redhat/RPMS/i386/unrar-3.5.2-1.i386.rpm

Preparing... ########################################### [100%]

1:unrar ########################################### [100%]

是不是可用？

[root@localhost beinan]# unrar x mydoc.rar

清理垃圾文件：如果您经常用这种办法编译RPM 包，主要清理一下 /usr/src/redhat内各个目录的内容；

二、通过file.tar.gz 、file.tar.bz2 和 file.sepc 来编译rpm ；

我们可以把file.tar.gz 或者 file.tar.bz2放到/usr/src/redhat/SOURCE 目录中，然后执行file.spec 文件；有时有些软件开发者会把file.spec 放在源码包中；这时我们就要解压后来查看是否是存在；如果有大多是用下面的办法：

[root@localhost beinan]# cp file.tar.gz /usr/src/redhat/SOURCES/

[root@localhost beinan]# tar zxvf file.tar.gz

[root@localhost beinan]# cd filedir

[root@localhost beinan]# rpmbuild --ba file.spec

三、如果没有spec 文件，我应该怎么办？

如果您只是想安装这个软件，可以用下面的办法；

[root@localhost beinan]# tar zxvf file.tar.gz 或者 tar file.tar.bz2

[root@localhost beinan]# cd filedir

[root@localhost beinan]# ./configure --help

注：可以用--help 来查看参数；如果您不懂如何加，一般就默认就好，试着编译几个就知道了；

[root@localhost beinan]# make

[root@localhost beinan]# make install

[root@localhost beinan]# make clean

有些软件只提供比较老的file.spec 文件，我们可以改造改造file.spec 文件，以适合软件的新版本；您也可以到 [http://svn.rpmforge.net/svn/trunk/rpms](http://svn.rpmforge.net/svn/trunk/rpms) 查找是不是有对应的file.spec 文件；

有些软件不提供file.spec 文件，并且您也在其实资源站上也找不到类似的文件，而且您还想从源码包打一个file.rpm 包和大家交流，那只好自己学习编写了；

我们可以把 [http://svn.rpmforge.net/svn/trunk/rpms](http://svn.rpmforge.net/svn/trunk/rpms) 当作学习资源，也是一件快乐的事；

后记： 为初学者了解file.src.rpm 的用处而写，老鸟不必看了；如果补充点file.spec 编写规则及学习方法，欢迎中。。。。。。