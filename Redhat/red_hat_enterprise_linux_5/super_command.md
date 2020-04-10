## super command {#super-command}

1\. 删除0字节文件

find -type f -size 0 -exec rm -rf {} \;

2.查看进程

按内存从大到小排列

ps -e  -o &quot;%C  : %p : %z : %a&quot;|sort -k5 -nr

3.按cpu利用率从大到小排列

ps -e  -o &quot;%C  : %p : %z : %a&quot;|sort  -nr

4.打印说cache里的URL

grep -r -a  jpg /data/cache/* | strings | grep &quot;http:&quot; | awk -Fhttp: {print &quot;http:&quot;$2;}

5.查看http的并发请求数及其TCP连接状态：

netstat -n | awk /^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}

6\. sed -i /Root/s/no/yes/ /etc/ssh/sshd_config  sed在这个文里Root的一行，匹配Root一行，将no替换成yes.

7.1.如何杀掉mysql进程：

ps aux|grep mysql|grep -v grep|awk {print $2}|xargs kill -9   (从中了解到awk的用途)

pgrep mysql |xargs kill -9 [网友:&amp;FROST]

killall -TERM mysqld

kill -9 `cat /usr/local/apache2/logs/httpd.pid`  试试查杀进程PID

8.显示运行3级别开启的服务:

ls /etc/rc3.d/S* |cut -c 15-  (从中了解到cut的用途，截取数据)

9.如何在编写SHELL显示多个信息，用EOF

cat Apache的并发请求数及其TCP连接状态：

netstat -n | awk /^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}

15.因为同事要统计一下服务器下面所有的jpg的文件的大小,写了个shell给他来统计.原来用xargs实现,但他一次处理一部分,搞的有多个总和....,下面的命令就能解决啦.

find / -name *.jpg -exec wc -c {} \;|awk {print $1}|awk {a+=$1}END{print a}

CPU的数量（多核算多个CPU，cat /proc/cpuinfo |grep -c processor）越多，系统负载越低，每秒能处理的请求数也越多。

16  CPU负载  # cat /proc/loadavg

检查前三个输出值是否超过了系统逻辑CPU的4倍。  

18  CPU负载  #mpstat 1 1

检查%idle是否过低(比如小于5%)

19  内存空间  # free

检查free值是否过低  也可以用 # cat /proc/meminfo

20  swap空间  # free

检查swap used值是否过高  如果swap used值过高，进一步检查swap动作是否频繁：

# vmstat 1 5

观察si和so值是否较大

21  磁盘空间  # df -h

检查是否有分区使用率(Use%)过高(比如超过90%)  如发现某个分区空间接近用尽，可以进入该分区的挂载点，用以下命令找出占用空间最多的文件或目录：

# du -cks * | sort -rn | head -n 10

22  磁盘I/O负载  # iostat -x 1 2

检查I/O使用率(%util)是否超过100%

23  网络负载  # sar -n DEV

检查网络流量(rxbyt/s, txbyt/s)是否过高

24  网络错误  # netstat -i

检查是否有网络错误(drop fifo colls carrier)  也可以用命令：# cat /proc/net/dev

25 网络连接数目  # netstat -an | grep -E &quot;^(tcp)&quot; | cut -c 68- | sort | uniq -c | sort -n

26  进程总数  # ps aux | wc -l

检查进程个数是否正常 (比如超过250)

27  可运行进程数目  # vmwtat 1 5

   列给出的是可运行进程的数目，检查其是否超过系统逻辑CPU的4倍

28  进程  # top -id 1

观察是否有异常进程出现

29  网络状态  检查DNS, 网关等是否可以正常连通

30  用户  # who | wc -l

检查登录用户是否过多 (比如超过50个)  也可以用命令：# uptime

31  系统日志  # cat /var/log/rflogview/*errors

检查是否有异常错误记录  也可以搜寻一些异常关键字，例如：

# grep -i error /var/log/messages

# grep -i fail /var/log/messages

# egrep -i error|warn /var/log/messages 查看系统异常

32  核心日志  # dmesg

检查是否有异常错误记录

33  系统时间  # date

检查系统时间是否正确

34  打开文件数目  # lsof | wc -l

检查打开文件总数是否过多

35  日志  # logwatch -print  配置/etc/log.d/logwatch.conf，将 Mailto 设置为自己的email 地址，启动mail服务 (sendmail或者postfix)，这样就可以每天收到日志报告了。

缺省logwatch只报告昨天的日志，可以用# logwatch -print -range all 获得所有的日志分析结果。

可以用# logwatch -print -detail high 获得更具体的日志分析结果(而不仅仅是出错日志)。

36.杀掉80端口相关的进程

lsof -i :80|grep -v &quot;PID&quot;|awk {print &quot;kill -9&quot;,$2}|sh

37.清除僵死进程。

ps -eal | awk { if ($2 == &quot;Z&quot;) {print $4}} | kill -9

38.tcpdump 抓包 ，用来防止80端口被人攻击时可以分析数据

# tcpdump -c 10000 -i eth0 -n dst port 80 &gt; /root/pkts

39.然后检查IP的重复数 并从小到大排序 注意 &quot;-t\ +0&quot;  中间是两个空格

# less pkts | awk {printf $3&quot;\n&quot;} | cut -d. -f 1-4 | sort | uniq -c | awk {printf $1&quot; &quot;$2&quot;\n&quot;} | sort -n -t\ +0

40.查看有多少个活动的php-cgi进程

netstat -anp | grep php-cgi | grep ^tcp | wc -l