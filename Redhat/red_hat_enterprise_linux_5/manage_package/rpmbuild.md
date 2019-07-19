### rpmbuild {#rpmbuild}

Usage: rpmbuild [OPTION...]

* 使用命令：rpmbuild ·[OPTION]

Build options with [ &lt;specfile&gt; | &lt;tarball&gt; | &lt;source package&gt; ]:

*建立包的选项有：[ 从文件&lt;specfile&gt;建立 |从 &lt;tarball&gt;包建立 |从 &lt;source package&gt;包建立]

*从文件&lt;specfile&gt;建立

 -bp      build through %prep (unpack sources and apply patches) from &lt;specfile&gt;  # -bp 从&lt;specfile&gt;文件的%prep段开始建立（解开源码包并打补丁）

 -bc      build through %build (%prep, then compile) from &lt;specfile&gt;                      # -bc 从&lt;specfile&gt;文件的%build

 -bi      build through %install (%prep, %build, then install) from &lt;specfile&gt;

 -bl      verify %files section from &lt;specfile&gt;                       # 检查&lt;specfile&gt;文件的%files段

 -ba      build source and binary packages from &lt;specfile&gt;  # 建立源码和二进制包

 -bb      build binary package only from &lt;specfile&gt;              #只建立二进制包

 -bs      build source package only from &lt;specfile&gt;             #只建立源码包

*从 &lt;tarball&gt;包建立

 -tp      build through %prep (unpack sources and apply patches) from &lt;tarball&gt;

 -tc      build through %build (%prep, then compile) from &lt;tarball&gt;

 -ti      build through %install (%prep, %build, then install) from &lt;tarball&gt;

 -ta      build source and binary packages from &lt;tarball&gt;   #建立源码和二进制包

 -tb      build binary package only from &lt;tarball&gt;          #只建立二进制包

 -ts      build source package only from &lt;tarball&gt;          #只建立源码包

*从 &lt;source package&gt;包建立

 --rebuild    build binary package from &lt;source package&gt;   #建立二进制包

 --recompile  build through %install (%prep, %build, then install) from &lt;source package&gt;

*rpmbuild的其他使用项

 --buildroot=DIRECTORY  override build root   #确定以root目录建立包

 --clean        remove build tree when done  #完成打包后清除BUILD下的文件目录

 --nobuild      do not execute any stages of the build   #不进行BUILD的阶段

 --nodeps       do not verify build dependencies         #不检查建立包时的关联文件

 --nodirtokens  generate package header(s) compatible with (legacy) rpm[23] packaging

 --rmsource        remove sources when done  #完成打包后清除sources

 --rmspec          remove specfile when done #完成打包后清除specfile

 --short-circuit   skip straight to specified stage (only for c,i)  #跳过

 --target=CPU-VENDOR-OS    override target platform    #确定包的最终使用平台

Common options for all rpm modes:  #所有rpm都可使用的选项

 -D, --define=MACRO EXPR     define MACRO with value EXPR              #预定义

 -E, --eval=EXPR          print macro expansion of EXPR             #显示大量EXPR扩展信息

 --macros=&lt;FILE:...&gt;    read &lt; FILE:... &gt; instead of default file(s)     #读&lt; FILE:... &gt;文件代替默认文件

 --nodigest                   dont verify package digest(s)         #不检查包的说明信息

 --nosignature              dont verify package signature(s)   #不检查包的签名信息

 --rcfile=&lt;FILE:...&gt;        read &lt; FILE:... &gt; instead of default file(s)   #读&lt; FILE:... &gt;文件代替默认文件

 -r, --root=ROOT           use ROOT as top level directory (default: &quot;/&quot;)     #使ROOT为最高级别的路径

 --querytags                 display known query tags          #显示已知的有疑问的地方

 --showrc                      display final rpmrc and macro configuration     #显示最终的配置信息

 --quiet                         provide less detailed output       #提供少量的信息

 -v, --verbose               provide more detailed output             #提供大量的详细的信息

 --version                     print the version of rpm being used     #显示rpm包的版本

Options implemented via popt alias/exec:

*附加选项

 --dbpath=DIRECTORY   use database in DIRECTORY                                        

 --with=&lt;option&gt;          enable configure &lt;option&gt; for build  #建立时允许配置的选项

 --without=&lt;option&gt;     disable configure &lt;option&gt; for build   #建立时不允许配置的选项

Help options:

*帮助选项

 -?, --help                    Show this help message  #显示帮助信息

 --usage                       Display brief usage message  #显示使用方法的信息