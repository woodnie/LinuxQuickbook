#### crontab {#crontab}

crontab

NAME

      crontab - maintain crontab files for individual users (ISC Cron V4.1)

SYNOPSIS

      crontab [-u user] file

      crontab [-u user] [-l | -r | -e] [-i] [-s]

1\. 用户crontab

命令行中file是命令文件的名字。如果在命令行中指定了这个文件，那么执行 crontab命令，则将这个文件拷贝到crontabs目录下；如果在命令行中没有制定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将他们也存放在crontab目录下。

使用命令crontab -u user -r 选项的作用是从/usr/spool/cron/crontabs目录下删除用户定义的文件crontab；

使用命令crontab -u user -l 选项的作用是显示用户crontab文件的内容。

使用命令crontab -u user -e 命令编辑用户user的cron(c)作业。用户通过编辑文件来增加或修改任何作业请求。

使用命令crontab -u user -r 即可删除当前用户的所有的cron作业。

作业与它们预定的时间储存在文件/usr/spool/cron/crontabs/username里。username是用户名，在相应的文件中存放着该用户所要运行的命令。命令执行的结果，无论是标准输出还是错误输出，都将以邮件形式发给用户。

文件里的每一个请求必须包含以spaces和 tabs分割的六个域。前五个字段可以取整数值，指定何时开始工作，第六个域是字符串，称为命令字段，其中包括了crontab调度执行的命令。

选项：

[root@node ~]# crontab -e    #创建

[root@node ~]# crontab -l     #列举

[root@node ~]# crontab -r     #删除

[root@node ~]# crontab -e    #编辑

有效字段：

分钟 0-59

小时 0-23

日期 0-31

月份 0-12

星期 0-6(0=星期天)

e.g.:

#以 Min   Hour  Date  Month  Week   Command 的顺序定义

[root@node ~]# man 5 crontab  

EXAMPLE CRON FILE

      # use /bin/sh to run commands, no matter what /etc/passwd says

      SHELL=/bin/sh

      # mail any output to paul, no matter whose crontab this is

      MAILTO=paul

      #

      # run five minutes after midnight, every day

      5 0 * * *       $HOME/bin/daily.job &gt;&gt; $HOME/tmp/out 2&gt;&amp;1

      # run at 2:15pm on the first of every month -- output mailed to paul

      15 14 1 * *     $HOME/bin/monthly

      # run at 10 pm on weekdays, annoy Joe

      0 22 * * 1-5    mail -s &quot;Its 10pm&quot; joe%Joe,%%Where are your kids?%

      23 0-23/2 * * * echo &quot;run 23 minutes after midn, 2am, 4am ..., everyday&quot;

      5 4 * * sun     echo &quot;run at 5 after 4 every sunday&quot;

      */10     8-17   *     *    *        /usr/bin/free   #在8:00-17:00，每隔10分钟，运行一次命令  

      */2       *         *     *    3,5    /usr/bin/free   #在每周三，周五 每隔2分钟，运行一次命令

访问控制：

/etc/cron.allow

/etc/cron.deny

2.系统crontab

2.1.系统crontab

[root@node ~]# cat /etc/crontab

SHELL=/bin/bash

PATH=/sbin:/bin:/usr/sbin:/usr/bin

MAILTO=root

HOME=/

# run-parts

01 * * * * root run-parts /etc/cron.hourly

02 4 * * * root run-parts /etc/cron.daily

22 4 * * 0 root run-parts /etc/cron.weekly

42 4 1 * * root run-parts /etc/cron.monthly

2.2额外的系统crontab文件

/etc/cron.d/ #包含额外的系统crontab文件

3.日常cron工作

3.1.tmpwatch

tmpwatch删除/tmp     目录下204小时（10天）内未访问过的文件

tmpwatch删除/var/tmp目录下720小时（30天）内未访问过的文件

3.2.logrotate

让日志文件不要变得太大

/etc/logrotate.conf文件的配置自由度高

3.3.logwatch

提供系统活动概要

报告可疑信息

配置文件: /etc/logwatch/conf/logwatch.conf

4.anacron系统

anacron用于当计算机关机一段时间重启后，告诉计算机怎么运行cron的任务。

[root@node ~]# cat /etc/anacrontab  #anacron配置文件

# /etc/anacrontab: configuration file for anacron

# See anacron(8) and anacrontab(5) for details.

SHELL=/bin/sh

PATH=/sbin:/bin:/usr/sbin:/usr/bin

MAILTO=root

1       65    cron.daily     run-parts /etc/cron.daily    #如果在1 天内没有运行/etc/cron.daily，     那么在65分钟后运行它

7       70    cron.weekly    run-parts /etc/cron.weekly   #如果在7 天内没有运行/etc/cron.weekly， 那么在65分钟后运行它

30      75    cron.monthly   run-parts /etc/cron.monthly  #如果在30天内没有运行/etc/cron.monthly，那么在65分钟后运行它

配置文件说明：

Field 1: 如果在这日子里没有运行这些任务…

Field 2: 在重新引导后等待这么多分钟后运行它

Field 3: 任务识别器

Field 4: 要执行的任务

5.

@reboot: 在每次開機時執行。

@yearly: 等同 0 0 1 1 * 寫法，即每年一月一日零時零分。

@annually: 與 @yearly 相同。

@monthly: 在每月一號零時零分執行。

@weekly: 在星期天零時零分執行。 Run once a week, &quot;0 0 * * 0″.

@daily: 每天零時零分。

@midnight: 與 @daily 相同。

@hourly: 每小時零分執行。5.

@reboot: 在每次開機時執行。

@yearly: 等同 0 0 1 1 * 寫法，即每年一月一日零時零分。

@annually: 與 @yearly 相同。

@monthly: 在每月一號零時零分執行。

@weekly: 在星期天零時零分執行。 Run once a week, &quot;0 0 * * 0″.

@daily: 每天零時零分。

@midnight: 與 @daily 相同。

@hourly: 每小時零分執行。