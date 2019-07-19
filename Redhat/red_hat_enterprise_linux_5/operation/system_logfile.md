### system logfile {#system-logfile}

inux 日志文件utmp、wtmp、lastlog、messages

1、有关当前登录用户的信息记录在文件utmp中；======who命令

　　2、登录进入和退出纪录在文件wtmp中；========w命令

　　3、最后一次登录文件可以用lastlog命令察看；

　　4、messages======从syslog中记录信息

　　注意：wtmp和utmp文件都是二进制文件，他们不能被诸如tail命令剪贴或合并（使用cat命令）。用户

　　需要使用who、w、users、last和ac来使用这两个文件包含的信息。

　　例子：

　　last命令往回搜索wtmp来显示自从文件第一次创建以来登录过的用户

　　users用单独的一行打印出当前登录的用户，每个显示的用户名对应一个登录会话

　　w命令查询utmp文件并显示当前系统中每个用户和它所运行的进程信息

　　who命令查询utmp文件并报告当前登录的每个用户

　　ac命令根据当前的/var/log/wtmp文件中的登录进入和退出来报告用户连结的时间（小时）

　　utmp文件，它记录当前登录进系统的各个用户；

　　包含下列结构的一个二进制记录写入这两个文件中：

　　struct utmp {

　　char ut_line[8]; /* tty line: &quot;ttyh0&quot;, &quot;ttyd0&quot;, &quot;ttyp0&quot;, ... */

　　char ut_name[8]; /* login name */

　　long ut_time; /* seconds since Epoch */

　　};

　　登录时，login程序填写这样一个结构，然后将其写入到utmp文件中，同时也将其添写到wtmp文件中。注销时， init进程将utmp文件中相应的记录擦除(每个字节都填以0 )，并将一个新记录添写到wtmp文件中。读wtmp文件中的该注销记录，其ut_name字段清除为0。在系统再启动时，以及更改系统时间和日期的前后，都在wtmp文件中添写特殊的记录项。who( 1 )程序读utmp文件，并以可读格式打印其内容。后来的UNIX版本提供last( 1 )命令，它读wtmp文件并打印所选择的记录。wtmp文件，它跟踪各个登录和注销事件。

　　wted

　　wtmp/utmp日志编辑程序。你可以使用这个工具编辑所有wtmp或者utmp类型的文件。

　　z2

　　utmp/wtmp/lastlog日志清理工具。可以删除utmp/wtmp/lastlog日志文件中有关某个用户名的所有条目。不过，如果用于Linux系统需要手工修改其源代码，设置日志文件的位置。