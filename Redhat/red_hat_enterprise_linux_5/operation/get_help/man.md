#### man {#man}

man page 使用技巧

man page 基本操作：

空格键                      #向下翻一页

[Page Up]                #向上翻一页

[Page Down]           #向下翻一页

[Home]                     #定位到首页

[End]                         #定位到末页

方向键

字符查找：

/ 字符串                  #向下搜寻此字符串

?字符串                  #向上搜寻此字符串

n                              #继续向下搜寻

N                             #进行反向搜寻

[root@localhost ~]# man -f   keywords  #搜索所有man page,列举出在NAME中包含该keywords的所有说明书页

[root@localhost ~]# man -k  keywords  #搜索所有man page,列举出在NAME和SYNOPSIS中包含该keywords的所有说明书页

[root@localhost ~]# man -K keywords  #会显示每个包含关键字的说明书页，病询问是否阅读

[root@localhost ~]# man  3  C函数名   #会显示该C函数的功能说明

[root@localhost ~]# man -k  keywords  #如果没结果

[root@localhost ~]# makewhatis           #生成以下whatis数据库

man page 基本内容：

NAME：名称和作用的简单描述

SYNOPSIS：用法概述，包括可用选项

DESCRIPTION：对命令功能的比较详细的描述

OPTIONS：注意列举选项

FILE：和该命令相关的文件

EXAMPLES：举例说明命令用法

SEE ALSO：其他参考资料

以man为例：

[root@localhost ~]# man man

NAME

      man - format and display the on-line manual pages

SYNOPSIS

      man  [-acdfFhkKtwW] [--path] [-m system] [-p string] [-C config_file] [-M pathlist] [-P pager] [-B browser] [-H

      htmlpager] [-S section_list] [section] name ...

DESCRIPTION

      man formats and displays the on-line manual pages.  If you specify section, man only looks in that  section  of

      the  manual.  name is normally the name of the manual page, which is typically the name of a command, function,

      or file.  However, if name contains a slash (/) then man interprets it as a file specification, so that you can

      do man ./foo.5 or even man /cd/foo/bar.1.gz.

      See below for a description of where man looks for the manual page files.

OPTIONS

      -C  config_file

             Specify the configuration file to use; the default is /etc/man.config.  (See man.config(5).)

      -M  path

             Specify  the  list  of  directories  to search for man pages.  Separate the directories with colons.  An

             empty list is the same as not specifying -M at all.  See SEARCH PATH FOR MANUAL PAGES.

..........

Linux说明书（man page）被划归为以下几个部分(章)：

man 1 ：用户命令（一般使用者的命令）

man 2 ：系统调用（系统调用的命令）

man 3 ：库调用   （C语言函数库的命令）

man 4 ：特殊文件（有关驱动程序和系统设备的解释）

man 5 ：文件格式（配置文件的解释 ）

man 6 ：游戏       （游戏程序的命令）

man 7 ：杂项       （其他的软件或是程序的命令）

man 8 ：管理命令（有关系统维护的命令）