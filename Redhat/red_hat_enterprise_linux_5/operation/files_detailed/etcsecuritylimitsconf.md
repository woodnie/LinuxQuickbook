#### /etc/security/limits.conf {#etc-security-limits-conf}

#&lt;domain&gt; can be:

#        - an user name

#        - a group name, with @group syntax

#        - the wildcard *, for default entry

#        - the wildcard %, can be also used with %group syntax,

#                 for maxlogin limit

#

#&lt;type&gt; can have the two values:

#        - &quot;soft&quot; for enforcing the soft limits

#        - &quot;hard&quot; for enforcing hard limits

#

#&lt;item&gt; can be one of the following:

#        - core - limits the core file size (KB)  --- 限制内核文件的大小

#        - data - max data size (KB)  ---------------最大数据大小

#        - fsize - maximum filesize (KB)-------------最大文件大小

#        - memlock - max locked-in-memory address space (KB)--最大锁定内存地址空间

#        - nofile - max number of open files-----------打开文件的最大数目

#        - rss - max resident set size (KB)------------最大持久设置大小

#        - stack - max stack size (KB)-----------------最大栈大小

#        - cpu - max CPU time (MIN)--------------------最多CPU时间

#        - nproc - max number of processes-------------进程的最大数目

#        - as - address space limit---------------------地址空间限制

#        - maxlogins - max number of logins for this user----此用户允许登录的最大数目

#        - maxsyslogins - max number of logins on the system--系统允许登录的最多用户数

#        - priority - the priority to run user process with--用户进程的优先级运行

#        - locks - max number of file locks the user can hold--用户可以持有的文件锁最大数量

#        - sigpending - max number of pending signals---未处理信号的最大数量

#        - msgqueue - max memory used by POSIX message queues (bytes)--内存使用最大POSIX消息队列

#        - nice - max nice priority allowed to raise to--允许的最大优先级

#        - rtprio - max realtime priority----最大实时优先