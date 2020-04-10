### du {#du}

du

NAME

      du - estimate file space usage

1\. 报告每个目录的磁盘用量

2.包括每个子目录的总量

语　法：du [-abcDhHklmsSx] [-L &lt;符号连接&gt;][-X &lt;文件&gt;][--block-size][--exclude=&lt;目录或文件&gt;] [--max-depth=&lt;目录层数&gt;][--help][--version][目录或文件] 常用参数： -a或-all 为每个指定文件显示磁盘使用情况，或者为目录中每个文件显示各自磁盘使用情况。 -c或-total 除了显示目录或文件的大小外，同时也显示所有目录或文件的总和。 -D或-dereference-args 显示指定符号连接的源文件大小。        -L&lt;符号连接&gt;或-dereference&lt;符号连接&gt; 显示选项中所指定符号连接的源文件大小。        -l或-count-links 重复计算硬件连接的文件。 -h或-human-readable 以K，M，G为单位，提高信息的可读性。         -H或-si 与-h参数相同，但是K，M，G是以1000为换算单位,而不是以1024为换算单位。 -b或  - bytes 显示目录或文件大小时，以byte为单位        -k或 - kilobytes   以1024 bytes为单位。         -m或 - megabytes 以1MB为单位。 -s或-summarize 仅显示总计，即只显示指定目录的大小，而不显示子目录的大小。-S或-separate-dirs 显示每个目录的大小时，并不含其子目录的大小。 -x或-one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。 -X&lt;文件&gt;或-exclude-from=&lt;文件&gt; 在&lt;文件&gt;指定目录或文件。 -exclude=&lt;目录或文件&gt; 略过指定的目录或文件。 -max-depth=&lt;目录层数&gt; 超过指定层数的目录后，予以忽略。 -help 显示帮助。 -version 显示版本信息。

应用程序-&gt;系统工具 -&gt;磁盘分析器 - 图形化地显示磁盘使用量

[root@node ~]# baobab

e.g.:

[root@node ~]# du -hs  /home  #只显示指定目录/home大小

[root@node ~]# du -h    /home  #显示指定下所有文件的大小

[root@node ~]# du -h -max-depth=0 user  #-max-depth＝n表示只深入到第n层目录，此处设置为0，即表示不深入到子目录。

[root@node ~]# du -h -exclude=*xyz*        #列出当前目录中的目录名不包括xyz字符串的目录的大小

[root@node ~]# du -0h user                          #想在一个屏幕下列出更多的关于user目录及子目录大小的信息

                                                                         # -0选项表示每列出一个目录的信息，不换行，而是直接输出下一个目录的信息。