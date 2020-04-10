#### /etc/issue {#etc-issue}

/etc/issue

[root@localhost ~]# cat /etc/issue

Red Hat Enterprise Linux Server release 5.5 (Tikanga)

Kernel \r on an \m

\r \n ( 反斜线在这里是转义字符，它给其后跟随的字符赋予另外的意义。可用Ctrl-Alt-F2来查看这些转义字符)

[root@localhost ~]# man mingetty  #  ISSUE ESCAPES 对系统定义的转义符进行说明

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

ISSUE ESCAPES

 mingetty recognizes the following escapes sequences which might be embedded in the /etc/issue file:

         \d     insert current day (localtime),

      \l     insert line on which mingetty is running,

      \m     inserts machine architecture (uname -m),

      \n     inserts machines network node hostname (uname -n),

      \o     inserts domain name,

      \r     inserts operating system release (uname -r),

      \t     insert current time (localtime),

      \s     inserts operating system name,

      \u     resp.  \U  the current number of users which are currently logged in.  \U inserts &quot;n users&quot;, where as \u only inserts &quot;n&quot;.

      \v     inserts operating system version (uname -v).

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

/etc/issue 文件在登录提示前显示，如下：

[root@localhost ~]# cat /etc/issue         #修改/etc/issue文件内容

Welcome！                                         #添加的内容

Red Hat Enterprise Linux Server release 5.5 (Tikanga)

Kernel \r on an \m

[root@localhost ~]# logout       #推出当前登录，可看到修改后的信息

当用户在一个控制台登录系统，修改/etc/issue文件后

在另一个控制台的登录提示符下键入 Ctrl-D 即可重新加载/etc/issue文件内容

按enter键刷新提示符