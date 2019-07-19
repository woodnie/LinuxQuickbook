### chage {#chage}

chage

NAME

      chage - change user password expiry information # 密码时效策略

选项：

 -m, --mindays     最小天数    将两次改变密码之间相距的最小天数设为&quot;最小天数&quot;

 -M, --maxdays     最大天数    将两次改变密码之间相距的最大天数设为&quot;最大天数&quot;

 -W, --warndays    警告天数    将过期警告天数设为&quot;警告天数&quot;

 -E, --expiredate  过期日期    将帐户过期时间设为&quot;过期日期&quot;

 -d, --lastday     最近日期    将最近一次密码设置时间设为&quot;最近日期&quot;

 -h, --help                   显示此帮助信息并退出

 -I, --inactive    失效密码    将因过期而失效的密码设为&quot;失效密码&quot;

 -l, --list                   显示帐户年龄信息

[root@node ~]# chage [option] username  #

[root@node ~]# chage               username  #不带选项将以交互模式进行设置