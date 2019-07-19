### rpm {#rpm}

rpm

rpm 包命名规则:

name-version-release.architecture.rpm

version：代表该项目的开源版本

release：代表红帽公司对开源代码另加的内部补丁的版本

安装/删除：

[root@localhost ~]# rpm -ivh &lt; rpm package name &gt;      #安装一个包 v 显示进度；h hash信息；

[root@localhost ~]# rpm -Uvh &lt; rpm package name &gt;      #升级一个包，-U 如果升级一个没有安装的包，则进入安装

[root@localhost ~]# rpm -Fvh &lt; rpm package name &gt;      #升级一个包，-F(--fresh)如果升级一个没有安装的包，则程序退出

[root@localhost ~]# rpm -e   &lt; rpm package name &gt;      #移走一个包

rpm查询：

[root@localhost ~]# rpm -q   &lt; rpm package name&gt;          #查询一个包是否被安装

[root@localhost ~]# rpm -qi  &lt; rpm package name&gt;          #得到被安装的包的信息

[root@localhost ~]# rpm -ql  &lt; rpm package name&gt;          #列出被安装包中有哪些文件

[root@localhost ~]# rpm -qil &lt; rpm package name&gt;          #可综合好几个参数一起用

[root@localhost ~]# rpm -qa                           #列出系统中所有被安装的rpm package

[root@localhost ~]# rpm -qd                           #列出文档文件

[root@localhost ~]# rpm -qc                           #显示bind以未配置的状态安装

[root@localhost ~]# rpm -qf   &lt; 文件 &gt;                 #列出服务器上的一个文件属于哪一个RPM包  

[root@localhost ~]# rpm -qilp &lt; rpm package name&gt;     #列出一个未被安装进系统的RPM包文件中包含有哪些文件

rpm校验：

[root@node ~]# rpm -V                  #根据rpm数据库来校验指定软件包安装后是否有所改变

[root@node ~]# rpm -Va                 #根据rpm数据库来校验已安装的所有软件包安装后是否有所改变

[root@node ~]# rpm -Vp                 #根据软件包所包含的文件来校验已安装的包安装后是否有所改变

[root@node ~]# rpm --import  key path  #import key

[root@node ~]# rpm -K package          #校验包的完整性

--force     即使覆盖属于其它包的文件也强迫安装

--nodeps 如果该RPM包的安装依赖其它包，即使其它包没装，也强迫安装。

--root      指定安装路径

--whatrequires capability   所有需要安装程序的软件包

--whatprovides capability   所有提供特定能力的软件包

--requires       查询软件包先决条件

--provides       查询软件包

--scripts         在进行安装和删除操作时运行的脚本

--changelog    软件包修订历史记录

--queryformat  格式化定制格式的信息

++++++++++++++++++++++++++++++++

安装xxx.src.rpm包的方法

方法一：

（1）rpm -ivh xxx.src.rpm   执行rpm安装命令

（2）cd /usr/src/redhat/SPECS 切换目录到/usr/src/redhat/SPECS （src.rpm包默认的解压目录）

（3）rpmbuild -bp my-package.spec 执行rpmbuild会生成源码包

（4）cd /usr/src/redhat/BUILD/my-package   切换到生成的源码包

（5）./configure 编译配置

（6）make 编译

（7）make install 安装

方法二：

（1）rpm -ivh xxx.src.rpm

（2）cd /usr/src/redhat/SPECS

（3）rpmbuild -bb my-package.spec

执行rpmbuild操作，会在/usr/src/redhat/RPMS/i386（不同的包，产生的路径不相同，可能会是i686、noarch等）下创建一个或多个的rpm包

（4）rpm -ivh /usr/src/redhat/RPMS/i386/my-package.rpm

注：当执行rpmbuild -bb my-package.spec 出现错误时，例：Error: Architecture is not included : i386，此错误表示该软件包不支持i386平台（默认的rpmbuild为i386平台），需指定到别的平台，指定参数为--target=i686

rpmbuild -bb -target=i686 my-package.spec

i386软件包可以在任何x86平台下使用，无论是i686还是x86_64的机器；而i686的软件包一般都对cpu进行了优化，具有向后的兼容性，不具有向前的兼容性。

方法二：# rpmbuild --rebuild xxx.src.rpm

参数详解：

一、安装  

命令格式：  

rpm -i ( or --install) options file1.rpm ... fileN.rpm  

参数：  

file1.rpm ... fileN.rpm  将要安装的RPM包的文件名  

详细选项：  

-h (or --hash) 安装时输出hash记号 (``#)    

--test         只对安装进行测试，并不实际安装。  

--percent      以百分比的形式输出安装的进度。  

--excludedocs  不安装软件包中的文档文件  

--includedocs  安装文档  

--replacepkgs  强制重新安装已经安装的软件包  

--replacefiles 替换属于其它软件包的文件  

--force        忽略软件包及文件的冲突  

--noscripts    不运行预安装和后安装脚本  

--prefix &lt;path&gt; 将软件包安装到由 &lt;path&gt; 指定的路径下  

--ignorearch    不校验软件包的结构  

--ignoreos      不检查软件包运行的操作系统  

--nodeps        不检查依赖性关系  

--ftpproxy &lt;host&gt;   用 &lt;host&gt; 作为 FTP代理    

--ftpport &lt;port&gt;    指定FTP的端口号为 &lt;port&gt;  

通用选项  

-v  显示附加信息  

-vv 显示调试信息  

--root &lt;path&gt; 让RPM将&lt;path&gt;指定的路径做为&quot;根目录&quot;，这样预安装程序和后安装程序都会安装到这个目录下  

--rcfile &lt;rcfile&gt; 设置rpmrc文件为 &lt;rcfile&gt;    

--dbpath &lt;path&gt;   设置RPM 资料库存所在的路径为 &lt;path&gt;  

二、删除  

命令格式：  

rpm -e ( or --erase) options pkg1 ... pkgN  

参数  

pkg1 ... pkgN ：要删除的软件包  

详细选项  

--test      只执行删除的测试  

--noscripts 不运行预安装和后安装脚本程序  

--nodeps    不检查依赖性  

通用选项  

-vv           显示调试信息  

--root &lt;path&gt; 让RPM将&lt;path&gt;指定的路径做为&quot;根目录&quot;，这样预安装程序和后安装

程序都会安装到这个目录下  

--rcfile &lt;rcfile&gt; 设置rpmrc文件为 &lt;rcfile&gt;  

--dbpath &lt;path&gt;   设置RPM 资料库存所在的路径为 &lt;path&gt;  

三、升级  

命令格式  

rpm -U ( or --upgrade) options file1.rpm ... fileN.rpm  

参数  

file1.rpm ... fileN.rpm 软件包的名字  

详细选项  

-h (or --hash) 安装时输出hash记号 (``#)    

--oldpackage   允许&quot;升级&quot;到一个老版本  

--test         只进行升级测试  

--excludedocs  不安装软件包中的文档文件  

--includedocs  安装文档  

--replacepkgs  强制重新安装已经安装的软件包  

--replacefiles 替换属于其它软件包的文件  

--force        忽略软件包及文件的冲突  

--percent      以百分比的形式输出安装的进度。  

--noscripts    不运行预安装和后安装脚本    

--prefix &lt;path&gt; 将软件包安装到由 &lt;path&gt; 指定的路径下  

--ignorearch    不校验软件包的结构  

--ignoreos      不检查软件包运行的操作系统  

--nodeps        不检查依赖性关系  

--ftpproxy &lt;host&gt; 用 &lt;host&gt; 作为 FTP代理    

--ftpport &lt;port&gt;  指定FTP的端口号为 &lt;port&gt;  

通用选项  

-v  显示附加信息  

-vv 显示调试信息  

--root &lt;path&gt; 让RPM将&lt;path&gt;指定的路径做为&quot;根目录&quot;，这样预安装程序和后安装程序都会安装到这个目录下  

--rcfile &lt;rcfile&gt; 设置rpmrc文件为 &lt;rcfile&gt;    

--dbpath &lt;path&gt;   设置RPM 资料库存所在的路径为 &lt;path&gt;  

四、查询  

命令格式：  

rpm -q ( or --query) options  

参数：  

pkg1 ... pkgN ：查询已安装的软件包  

详细选项  

-p &lt;file&gt;(or ``-) 查询软件包的文件  

-f &lt;file&gt;           查询&lt;file&gt;属于哪个软件包  

-a                  查询所有安装的软件包  

--whatprovides &lt;x&gt;  查询提供了 &lt;x&gt;功能的软件包    

-g &lt;group&gt;          查询属于&lt;group&gt; 组的软件包  

--whatrequires &lt;x&gt; 查询所有需要 &lt;x&gt; 功能的软件包  

信息选项  

&lt;null&gt; 显示软件包的全部标识  

-i 显示软件包的概要信息  

-l 显示软件包中的文件列表  

-c 显示配置文件列表  

-d 显示文档文件列表  

-s 显示软件包中文件列表并显示每个文件的状态  

--scripts 显示安装、卸载、校验脚本  

--queryformat (or --qf) 以用户指定的方式显示查询信息  

--dump 显示每个文件的所有已校验信息    

--provides 显示软件包提供的功能  

--requires (or -R) 显示软件包所需的功能  

通用选项  

-v 显示附加信息  

-vv 显示调试信息  

--root &lt;path&gt; 让RPM将&lt;path&gt;指定的路径做为&quot;根目录&quot;，这样预安装程序和后安装程序都会安装到这个目录下  

--rcfile &lt;rcfile&gt; 设置rpmrc文件为 &lt;rcfile&gt;    

--dbpath &lt;path&gt; 设置RPM 资料库存所在的路径为 &lt;path&gt;  

五、校验已安装的软件包  

命令格式：  

rpm -V ( or --verify, or -y) options  

参数  

pkg1 ... pkgN 将要校验的软件包名  

软件包选项  

-p &lt;file&gt; Verify against package file &lt;file&gt;    

-f &lt;file&gt; 校验&lt;file&gt;所属的软件包  

-a Verify 校验所有的软件包  

-g &lt;group&gt; 校验所有属于组 &lt;group&gt;  的软件包  

详细选项  

--noscripts 不运行校验脚本    

--nodeps    不校验依赖性  

--nofiles   不校验文件属性  

通用选项  

-v   显示附加信息  

-vv  显示调试信息  

--root &lt;path&gt; 让RPM将&lt;path&gt;指定的路径做为&quot;根目录&quot;，这样预安装程序和后安装程序都会安装到这个目录下  

--rcfile &lt;rcfile&gt; 设置rpmrc文件为 &lt;rcfile&gt;    

--dbpath &lt;path&gt;   设置RPM 资料库存所在的路径为 &lt;path&gt;  

六、校验软件包中的文件  

语法：  

rpm -K ( or --checksig) options file1.rpm ... fileN.rpm  

参数：  

file1.rpm ... fileN.rpm 软件包的文件名  

Checksig--详细选项  

--nopgp 不校验PGP签名    

通用选项  

-v  显示附加信息  

-vv 显示调试信息  

--rcfile &lt;rcfile&gt; 设置rpmrc文件为 &lt;rcfile&gt;    

七、其它RPM选项  

--rebuilddb 重建RPM资料库  

--initdb    创建一个新的RPM资料库  

--quiet     尽可能的减少输出  

--help      显示帮助文件  

--version   显示RPM的当前版本  

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

一 RPM介绍

RPM 前是Red Hat Package Manager 的缩写，本意是Red Hat 软件包管理，顾名思义是Red Hat 贡献出来的软件包管理；现在应为RPM Package Manager的缩写。在Fedora 、Redhat、Mandriva、SuSE、YellowDog等主流发行版本，以及在这些版本基础上二次开发出来的发行版采用； RPM包中除了包括程序运行时所需要的文件，也有其它的文件；一个RPM 包中的应用程序，有时除了自身所带的附加文件保证其正常以外，还需要其它特定版本文件，这就是软件包的依赖关系。

RPM可以让用户直接以binary方式安装软件包，并且可替用户查询是否已经安装了有关的库文件；在用RPM删除程序时，它又会聪明地询问用户是否要删除有关的程序。如果使用RPM来升级软件，RPM会保留原先的配置文件，这样用户就不用重新配置新的软件了。RPM保留一个数据库，这个数据库中包含了所有的软件包的资料，通过这个数据库，用户可以进行软件包的查询。RPM虽然是为Linux而设计的，但是它已经移值到SunOS、Solaris、AIX、Irix等其它UNIX系统上了。RPM遵循GPL版权协议，用户可以在符合GPL协议的条件下自由使用及传播RPM。

二 RPM包分类

我个人认为rpm分为两大类，

1 二进制类包，包括rpm安装包（一般分为i386和x86等几种）和调式信息包等。

2 源码类包，源码包和开发包应该归位此类。

它们之间的关系是，最先我们按rpm打包要求改造软件项目源码，当符合要求之后就可以使用rpmbuild命令来生成不同的rpm包，同时生成的包之间版本是直接对应的，比如相同的源码包将生成完全相同的二进制rpm包。当你在网上查找rpm包时，一般你可以在RPMS目录中找到预编译的二进制包，而源码包则会在SRPMS目录内。

我们这里提到的RPM制作就是指改造软件源代码使之符合RPM打包要求的过程，这也可以等价为RPM源码包的制作过程，因为当你有了源码包就可以直接编译得到二进制安装包和其他任意包。

三 RPM包制作介绍

RPM包的制作，即是RPM源码包的制作。

这里我想说说RPM包工作的原理，这将有助于全面的了解RPM包管理系统的知识。

RPM是为解决源码包不易安装（需要编译）和软件包相互之间依赖（是RPM包管理器可以一定程度解决依赖问题）问题，它通过在探测源码包在build和install阶段的动作获得最终生成的需要安装的系统里的文件，并记录下一些必要的操作（比如安装完成后执行某项操作），然后把此组成为一个整体，当在用户安装此包时把前面获得的所有问题和记录的所有操作原原本本的作用的实际系统上。

为一个普通的源码打RPM包，需要下面一些操作，首先需要对项目的Makefile作必要的改造以支持RPM打包操作（实际上此操作不是绝对的，SPEC文档和Makefile的是协调统一工作的，只要他们之间配合好了其他都无所谓，我们一般只是推荐大家尽量按行业标准规范操作而已）；其次是针对当前项目撰写SPEC文档，SPEC文档包括了RPM打包过程的操作内容和新生成的RPM包的基本信息等，它的作用对象是打包程序rpmbuild。

四 RPM包制作过程

1 准备打包环境

fedora系统下使用如下命令安装rpmbuild

#yum install rpmbuild

rpmbuild的工作目录如下，

~/rpmbuild

~/rpmbuild/SOURCES

~/rpmbuild/SPECS

~/rpmbuild/BUILD

~/rpmbuild/RPMS

~/rpmbuild/RPMS/i386

~/rpmbuild/SRPMS

如果你的用户目录主目录下没有类似目录结构，你可以通过一个工具软件来自动配置和生成，如下。

＃yum install rpmdevtools下了运行自动配置命令自动生成如上目录，并配置一些必要操作。＃rpmdev-setuptreerpmdev-setuptree命令默认将再当前用户主目录下创建一个RPM构建根目录结构，如果需要改变次默认位置，可以修改配置文件:~/.rpmmacros中变量_topdir对应的值即可。

一般rpmbuild会在当前用户的主目录下自动建立如上目录结构，如果在你对应用户的构建目录中没有自动建立如上目录，你可以通过手动方式建立。上面目录的使用是这样分配的，SOURCES放置打包资源，包括源码打包文件和补丁文件等；SPECS目录放置SPEC文档；BUILD打包过程中的工作目录；RPMS目录存放生成的二进制包，RPM包根据硬件平台不同分类，i386表示生成i386结构的包将存放在该目录下;SRPMS目录存放生成的源码包。

2 撰写SPEC文档

SPEC撰写是打包RPM的核心，也算是最难的一步，好在我们可以从参照一个简单的模板文件开始，在可以实现基本功能的基础上再一步一步的扩充文档内容，直至完全达到要求。下面是一个简单的SPEC文档，其中包括了一些说明信息（注：#后面的内容为说明信息），该SPEC文档是对一个测试的软件项目hellorpm写的，hellorpm软件包编译后仅有一个执行文件、一个手册文件和一个项目说文件。

hellorpm.spec文档的内容如下：

----------------------------------------------------------

#软件包简要介绍

Summary: hellorpm is a test program。

#软件包的名字

Name: hellorpm        

#软件包的主版本号          

Version: 2.2.6          

#软件包的次版本号            

Release: 1  

#源代码包，默认将在上面提到的SOURCES目录中寻找                        

Source0: %{name}-%{version}.tar.gz  

#授权协议

License: GPL                  

#定义临时构建目录，这个地址将作为临时安装目录在后面引用

BuildRoot:%{_tmppath}/%{name}-%{version}-%{release}-root

#软件分类

Group: Development/Tools  

#软件包的内容介绍              

%description                        

The hellorpm program is a test.

#表示预操作字段，后面的命令将在源码代码BUILD前执行

%prep                    

#构建BUILD环境，将解压源码压缩包到BUILD目录

%setup -q      

#BUILD字段，将通过直接调用源码目录中自动构建工具完成源码编译操作        

%build      

#调用源码目录中的configure命令            

./configure        

#在源码目录中执行自动构建命令make    

make            

#安装字段        

%install    

#调用源码中安装执行脚本            

make DESTDIR=$RPM_BUILD_ROOT install

#文件说明字段，声明多余或者缺少都将可能出错

%files              

#设置文件权限属性      

%defattr(-,root,root)      

#声明/usr/local/bin/hellorpm将出现在软件包中      

/usr/local/bin/hellorpm      

#声明并设置文件属性  

%doc %attr(0444,root,root) /usr/local/man/man1/hellorpm.1  

#同上，声明文档文件

%doc README  

-----------------------------------------------------------------------------------

这个文档需要说明的一点：

BuildRoot:%{_tmppath}/%{name}-%{version}-%{release}-root

上面BuildRoot变量表示的是源码的临时按照目录，rpmbuild就是通过次目录获得将要按照到系统中的所有文件，而在SPEC文档后面make install 命令中的参数DESTDIR=$RPM_BUILD_ROOT即是对该参数的引用，这个参数将传给Makefile文件一告诉自动构建工具应该安装文件那里（实际上我再前文提到过的Makefile需要作一些改造以适应RPM的构建就包括此操作，你的Makefile文件中至少要知道在RPM构建过程中引用此参数的值去控制安装操作的目标）。

如上一个简单的SPEC文档撰写完成，下面把一个名为hellorpm-2.2.6.tar.gz的源码压缩文件放到rpmbuild根目录下的SOURCES目录下（注，确保此归档文件解压后的目录为hellorpm-2.2.6，否则会有问题）。

到此一个完整的rpm打包环境已经构建完成，下面我们就可以开始构建二进制和源代码RPM包。

3 构建RPM包

构建RPM包是有命令rpmbuild在SPEC的指导下完成。

开始构建操作，首先进入到当前用户的rpmbuild根目录（即上面提到的目录环境）。

#cd ~/rpmbuild/

执行如何命令，-ba表示build all，即生成包括二进制包和源代码包的所有RPM包，下来如果正常的话，rpmbuild将正常退出，同时在RPMS目录和SRPMS目录中将生成对应的RPM包。

#rpmbuild -ba SPECS/hellorpm.spec

这里仅仅介绍了一个最简单软件的最简单的RPM的打包操作过程，诸如带有共享文件的需要进行复杂配置的具有复杂依赖关系的等等的项目的打包以及后期的维护，包括补丁的制作我将在下来的时间完成补充更新，今天时间不早了，该休息了！

注：费了大半夜的功夫，搞出这么个令人不满意的文档，我思考着，这样做有多少意义呢？不敢重复发明轮子的，站到巨人的肩膀你才能看得更远，是这样吗？

是不是下周开始立个计划，每周至少翻译三篇fedora官网的文档给自己练练手。那糟糕的英语，唉！

参考资料：

[http://www.linuxsir.org/main/?q=node/50](http://www.linuxsir.org/main/?q=node/50) RPM 的介绍和应用（北南兄）

[http://www.ibm.com/developerworks/cn/linux/management/package/rpm/part3/](http://www.ibm.com/developerworks/cn/linux/management/package/rpm/part3/) 用 RPM 打包软件

[http://hlee.javaeye.com/blog/343499](http://hlee.javaeye.com/blog/343499) RPM包rpmbuild SPEC文件深度说明