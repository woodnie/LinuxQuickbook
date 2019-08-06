### mkisofs {#mkisofs}

mkisofs

mkisofs 是Unix系统中常用来制作ISO映像档的工具，搭配之前介绍的cdrecord便可以方便的将自己想要备份的东西烧录出来，是个不可不知道的小技巧。

虽然mkisofs的manpage和cdrecord一样&quot;落落长&quot;，但是常用的选项也只有那几个，现在示范几个mkisofs的语法：

mkisofs -o image_name.iso /home/willie

将/home/willie这个目录制作成image_name.iso映像档。

mkisofs -o ha.iso -r /

将/这个目录制作成ha.iso映像档，并指定为Rock Ridge格式。

使用Rock Ridge格式，可以保存档案相关的权限。

mkisofs -J -r /tmp | cdrecord speed=4 -v -data -

这个范例就比较炫了，除了加上-J选项使用Joliet格式外(可使用unicode储存中文档名)，并直接丢给cdrecord作on the fly烧录，省了制作映像档的步骤。

常用的选项大概就这几个，日后有空再来补充多段烧录的语法。

用mkisofs制作iso文件

mkisofs(make iso file system)

　　功能说明：建立ISO 9660映像文件。

　 　语　　法：mkisofs [-adDfhJlLNrRTvz][-print-size][-quiet][-A &lt;应用程序ID&gt;][-abstract &lt;摘要文件&gt;][-b &lt;开机映像文件&gt;][-biblio ][-c &lt;开机文件名称&gt;][-C &lt;盘区编号，磁区编号&gt;][-copyright &lt;版权信息文件&gt;][-hide &lt;目录或文件名&gt;][-hide-joliet &lt;文件或目录名&gt;][-log-file &lt;记录文件&gt;][-m &lt;目录或文件名&gt;][-M &lt;开机映像文件&gt;][-o &lt;映像文件&gt;][-p &lt;数据处理人&gt;][-P &lt;光盘发行人&gt;][-sysid &lt;系统ID &gt;][-V &lt;光盘ID &gt;][-volset &lt;卷册集ID&gt;][-volset-size &lt;光盘总数&gt;][-volset-seqno &lt;卷册序号&gt;][-x &lt;目录&gt;][目录或文件]

　　补充说明：mkisofs可将指定的目录与文件做成ISO 9660格式的映像文件，以供刻录光盘。

　　参　　数：

　　-a或--all mkisofs通常不处理备份文件。使用此参数可以把备份文件加到映像文件中。

　　-A&lt;应用程序ID&gt;或-appid&lt;应用程序ID&gt; 指定光盘的应用程序ID。

　　-abstract&lt;摘要文件&gt; 指定摘要文件的文件名。

　　-b&lt;开机映像文件&gt;或-eltorito-boot&lt;开机映像文件&gt; 指定在制作可开机光盘时所需的开机映像文件。

　　-biblio 指定ISBN文件的文件名，ISBN文件位于光盘根目录下，记录光盘的ISBN。

　　-c&lt;开机文件名称&gt; 制作可开机光盘时，mkisofs会将开机映像文件中的全-eltorito-catalog&lt;开机文件名称&gt;全部内容作成一个文件。

　　-C&lt;盘区编号，盘区编号&gt; 将许多节区合成一个映像文件时，必须使用此参数。

　　-copyright&lt;版权信息文件&gt; 指定版权信息文件的文件名。

　　-d或-omit-period 省略文件后的句号。

　　-D或-disable-deep-relocation ISO 9660最多只能处理8层的目录，超过8层的部分，RRIP会自动将它们设置成ISO 9660兼容的格式。使用-D参数可关闭此功能。

　　-f或-follow-links 忽略符号连接。

　　-h 显示帮助。

　　-hide&lt;目录或文件名&gt; 使指定的目录或文件在ISO 9660或Rock RidgeExtensions的系统中隐藏。

　　-hide-joliet&lt;目录或文件名&gt; 使指定的目录或文件在Joliet系统中隐藏。

　　-J或-joliet 使用Joliet格式的目录与文件名称。

　　-l或-full-iso9660-filenames 使用ISO 9660 32字符长度的文件名。

　　-L或-allow-leading-dots 允许文件名的第一个字符为句号。

　　-log-file&lt;记录文件&gt; 在执行过程中若有错误信息，预设会显示在屏幕上。

　　-m&lt;目录或文件名&gt;或-exclude&lt;目录或文件名&gt; 指定的目录或文件名将不会房入映像文件中。

　　-M&lt;映像文件&gt;或-prev-session&lt;映像文件&gt; 与指定的映像文件合并。

　　-N或-omit-version-number 省略ISO 9660文件中的版本信息。

　　-o&lt;映像文件&gt;或-output&lt;映像文件&gt; 指定映像文件的名称。

　　-p&lt;数据处理人&gt;或-preparer&lt;数据处理人&gt; 记录光盘的数据处理人。

　　-print-size 显示预估的文件系统大小。

　　-quiet 执行时不显示任何信息。

　　-r或-rational-rock 使用Rock Ridge Extensions，并开放全部文件的读取权限。

　　-R或-rock 使用Rock Ridge Extensions。

　　-sysid&lt;系统ID&gt; 指定光盘的系统ID。

　　-T或-translation-table 建立文件名的转换表，适用于不支持Rock Ridge Extensions的系统上。

　　-v或-verbose 执行时显示详细的信息。

　　-V&lt;光盘ID

从网上复制来的，太多了有点烦哈，呵呵，其实就一句顶用 mkisofs -o ABC.iso /media/cdrom

其中／media/cdrom就是我的挂载上去的光盘文件，当然也可以是文件系统里的一个文件或一个目录，如sources目录，而ABC.iso就是我想制作的iso光盘文件，文件名可以自己取！