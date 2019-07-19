#### build {#build}

定制RPM包

随着RedHat Linux的风靡全球，其软件包管理工具及格式RPM也得到推广。基于RPM源代码开放、安装卸载简单、升级维护方便及查询功能强大的特点，越来越多的开发者喜欢采用RPM格式来发布自己的软件包。RPM包里面都包含可执行的二进制程序，这个程序和Windows的软件包中的.exe文件类似是可执行的。

制作RPM软件包并不是一件复杂的工作，其中的关键在于编写SPEC软件包描述文件。要想制作一个rpm软件包就必须写一个软件包描述文件 （SPEC）。这个文件中包含了软件包的诸多信息，如软件包的名字、版本、类别、说明摘要、创建时要执行什么指令、安装时要执行什么操作、以及软件包所要包含的文件列表等等。

一、编写spec脚本

  由前面的日志了解到，生成rpm除了源码外，最重要的就是懂得编写.spec脚本。rpm建包的原理其实并不复杂，可以理解为按照标准的格式整理一些信息，包括：软件基础信息，以及安装、卸载前后执行的脚本，对源码包解压、打补丁、编译，安装路径和文件等。

  实际过程中，最关键的地方，是要清楚虚拟路径的位置，以及宏的定义。

二、关键字

spec脚本包括很多关键字，主要有：

Name: 软件包的名称，后面可使用%{name}的方式引用

Summary 软件包的内容概要

Version 软件的实际版本号，例如：1.0.1等，后面可使用%{version}引用

Release 发布序列号，例如：1linuxing等，标明第几次打包，后面可使用%{release}引用

Group 软件分组，建议使用标准分组

License 软件授权方式，通常就是GPL

Source 源代码包，可以带多个用Source1、Source2等源，后面也可以用%{source1}、%{source2}引用

BuildRoot 这个是安装或编译时使用的&quot;虚拟目录&quot;，考虑到多用户的环境，一般定义为：

%{_tmppath}/%{name}-%{version}-%{release}-root

或

%{_tmppath}/%{name}-%{version}-%{release}-buildroot-%(%{__id_u} -n}

该参数非常重要，因为在生成rpm的过程中，执行make install时就会把软件安装到上述的路径中，在打包的时候，同样依赖&quot;虚拟目录&quot;为&quot;根目录&quot;进行操作。

后面可使用$RPM_BUILD_ROOT 方式引用。

URL 软件的主页

Vendor 发行商或打包组织的信息，例如RedFlag Co,Ltd

Disstribution 发行版标识

Patch 补丁源码，可使用Patch1、Patch2等标识多个补丁，使用%patch0或%{patch0}引用

Prefix %{_prefix} 这个主要是为了解决今后安装rpm包时，并不一定把软件安装到rpm中打包的目录的情况。这样，必须在这里定义该标识，并在编写%install脚本的时候引用，才能实现rpm安装时重新指定位置的功能

Prefix %{_sysconfdir} 这个原因和上面的一样，但由于%{_prefix}指/usr，而对于其他的文件，例如/etc下的配置文件，则需要用%{_sysconfdir}标识

Build Arch 指编译的目标处理器架构，noarch标识不指定，但通常都是以/usr/lib/rpm/marcros中的内容为默认值

Requires 该rpm包所依赖的软件包名称，可以用&gt;=或&lt;=表示大于或小于某一特定版本，例如：

libpng-devel &gt;= 1.0.20 zlib

※&quot;&gt;=&quot;号两边需用空格隔开，而不同软件名称也用空格分开

还有例如PreReq、Requires(pre)、Requires(post)、Requires(preun)、Requires(postun)、BuildRequires等都是针对不同阶段的依赖指定

Provides 指明本软件一些特定的功能，以便其他rpm识别

Packager 打包者的信息

%description  软件的详细说明

三、spec脚本主体

spec脚本的主体中也包括了很多关键字和描述，下面会一一列举。我会把一些特别需要留意的地方标注出来。

%prep  预处理脚本

%setup -n %{name}-%{version}  把源码包解压并放好

通常是从/usr/src/asianux/SOURCES里的包解压到/usr/src/asianux/BUILD/%{name}-%{version}中。

一般用%setup -c就可以了，但有两种情况：一就是同时编译多个源码包，二就是源码的tar包的名称与解压出来的目录不一致，此时，就需要使用-n参数指定一下了。

%patch  打补丁

通常补丁都会一起在源码tar.gz包中，或放到SOURCES目录下。一般参数为：

%patch -p1 使用前面定义的Patch补丁进行，-p1是忽略patch的第一层目录

%Patch2 -p1 -b xxx.patch 打上指定的补丁，-b是指生成备份文件

%configure  这个不是关键字，而是rpm定义的标准宏命令。意思是执行源代码的configure配置

在/usr/src/asianux/BUILD/%{name}-%{version}目录中进行 ，使用标准写法，会引用/usr/lib/rpm/marcros中定义的参数。

另一种不标准的写法是，可参考源码中的参数自定义，例如：

引用

CFLAGS=&quot;$RPM_OPT_FLAGS&quot; CXXFLAGS=&quot;$RPM_OPT_FLAGS&quot; ./configure --prefix=%{_prefix}

%build  开始构建包

在/usr/src/asianux/BUILD/%{name}-%{version}目录中进行make的工作 ，常见写法：

引用

make %{?_smp_mflags} OPTIMIZE=&quot;%{optflags}&quot;

都是一些优化参数，定义在/usr/lib/rpm/marcros中

%install  开始把软件安装到虚拟的根目录中

在/usr/src/asianux/BUILD/%{name}-%{version}目录中进行make install的操作。这个很重要，因为如果这里的路径不对的话，则下面%file中寻找文件的时候就会失败。 常见内容有：

%makeinstall 这不是关键字，而是rpm定义的标准宏命令。也可以使用非标准写法：

引用

make DESTDIR=$RPM_BUILD_ROOT install

或

引用

make prefix=$RPM_BUILD_ROOT install

需要说明的是，这里的%install主要就是为了后面的%file服务的。所以，还可以使用常规的系统命令：

引用

install -d $RPM_BUILD_ROOT/

cp -a * $RPM_BUILD_ROOT/

%clean 清理临时文件

通常内容为：

引用

[ &quot;$RPM_BUILD_ROOT&quot; != &quot;/&quot; ] &amp;&amp; rm -rf &quot;$RPM_BUILD_ROOT&quot;

rm -rf $RPM_BUILD_DIR/%{name}-%{version}

※注意区分$RPM_BUILD_ROOT和$RPM_BUILD_DIR：

$RPM_BUILD_ROOT是指开头定义的BuildRoot，而$RPM_BUILD_DIR通常就是指/usr/src/asianux/BUILD，其中，前面的才是%file需要的。

%pre rpm安装前执行的脚本

%post  rpm安装后执行的脚本

%preun  rpm卸载前执行的脚本

%postun rpm卸载后执行的脚本

%files 定义那些文件或目录会放入rpm中

这里会在虚拟根目录下进行，千万不要写绝对路径，而应用宏或变量表示相对路径。 如果描述为目录，表示目录中除%exclude外的所有文件。

%defattr (-,root,root)  指定包装文件的属性，分别是(mode,owner,group)，-表示默认值，对文本文件是0644，可执行文件是0755

%exclude  列出不想打包到rpm中的文件

※小心，如果%exclude指定的文件不存在，也会出错的。

%changelog  变更日志

四、RPM包制作过程

1、 将需要制作的源码包放到/usr/src/redhat/SOURCES 目录下

2、 在/usr/src/redhat/SPECS 目录下编写SPEC脚本

3、 在/usr/src/redhat/SPECS目录下，输入 rpmbuild -bb  &quot;spec文件的名字&quot; 以后，就开始编译了

4、 编译成功以后，在/usr/src/redhat/RPMS/x86_64下会找到所编译成功的RPM包

实际上RPM包的制作原理就是根据spec脚本文件，将软件安装到指定的虚拟目录中，按照指定的格式进行打包。安装RPM包的时候，按照指定的格式安装到指定的目录下。

五、spec脚本文件范例

下面一制作memcached的源码包为例，介绍编写spec脚本

Summary:  Free &amp; open source, high-performance, distributed memory object caching system

Name:     memcached    #软件包的名称

Version:    1.4.5         #软件的实际版本号

Release:    1.139         #发布序列号，后面的139是与系统包区分开

License:    GPL          #软件授权方式，通常就是GPL

Group:     Applications

Source:     memcached-1.4.5.tar.gz   #源代码包

BuildRoot:  %{_tmppath}/%{name}-%{version}-root  #这个是安装或编译时使用的&quot;虚拟目录&quot;

BuildRequires: libevent,libevent-devel  #该rpm包所依赖的软件包名称

Url:           [http://memcached.org/](http://memcached.org/)

%description

Memcached is an in-memory key-value store for small chunks of arbitrary data (strings, objects) from results of database calls, API calls, or page rendering.

%prep   #预处理脚本

%setup -q     #提取源码到 BUILD 目录; -q 指不显示输出（quietly）

%build   #开始构建包

./configure

make

%install

rm -rf %{buildroot}  

make DESTDIR=$RPM_BUILD_ROOT install     #将软件安装到虚拟目录中

%files    #定义哪些些文件或目录会放入rpm中

%defattr(-, root, root, 0755)     #指定包装文件的属性

/usr/local/            #利用rpm安装以后，安装文件的路径

注: 卸载带有依赖关系的RPM包的时候，尽量不要用yum remove 包名卸载， 建议用rpm -e 进行卸载。卸载包的时候，一定要看好需要卸载的包名。