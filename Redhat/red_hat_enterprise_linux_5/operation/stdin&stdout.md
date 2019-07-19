### stdin&amp;stdout {#stdin-stdout}

标准输入输出

标准输入(STDIN)设备    ：键盘

标准输出(STDOUT)输出：显示器

标准错误(STDEER)输出：显示器

重定向标准输入符号：

&lt;

下面的例子tr 输出效果相同：

[root@localhost ~]# tr A-Z a-z &lt; .bash_profile

[root@localhost ~]# cat .bash_profile | tr A-Z a-z

下面的例子cat 输出效果相同：

[root@node ~]# cat /etc/passwd

[root@node ~]# cat &lt;/etc/passwd

重定向多行标准输入:

&lt;&lt;结束字符

[root@node ~]# mail -s &quot;Please call me&quot; root@node &lt;&lt;end

&gt; please call me ！

&gt; when you see this email.

&gt; end

重定向标准输出符号：

|          ##把标准输出重定向给一个程序(管道输入)

&gt;        ##把标准输出重定向到一个文件

2&gt;     ##把标准错误重定向到一个文件

&amp;&gt;     ##把所有输出重定向到一个文件

&gt;&gt;      ##把相应的输出重定向追加到一个文件

2&gt;&amp;1 ##把标准错误输出重定向给标准输出(要通过管道来发送所有输出时有用)

1.&amp;2

( )       ##合并多个输出

90    [j]&lt;&gt;filename

91       # 为了读写&quot;filename&quot;, 把文件&quot;filename&quot;打开, 并且将文件描述符&quot;j&quot;分配给它.

92       # 如果文件&quot;filename&quot;不存在, 那么就创建它.

93       # 如果文件描述符&quot;j&quot;没指定, 那默认是fd 0, stdin.

94       #

95       # 这种应用通常是为了写到一个文件中指定的地方.

96       echo 1234567890 &gt; File    # 写字符串到&quot;File&quot;.

97       exec 3&lt;&gt; File             # 打开&quot;File&quot;并且将fd 3分配给它.

98       read -n 4 &lt;&amp;3             # 只读取4个字符.

99       echo -n . &gt;&amp;3             # 写一个小数点.

100       exec 3&gt;&amp;-                 # 关闭fd 3.

101       cat File                  # ==&gt; 1234.67890

102       # 随机访问.

command1 | tee file | command2   #重导向到多个目标(tee)

例：

标准输出重定向给一个程序：

[wood@node1 ~]$ ls -l /etc | less

[wood@node1 ~]$ echo &quot;test email.&quot;| mail -s  &quot;test&quot;  root@node1    # -s 选项指定邮件主题

[wood@node1 ~]$ echo &quot;test print&quot; | lpr                                                # 使用默认打印机来打印test print

[wood@node1 ~]$ echo &quot;test print&quot; | lpr -p default_printer                  # -P 选项指定打印机

标准错误重定向：(wood为非root用户)

[wood@node1 ~]$ find /etc/ -name passwd &gt;&gt;find.out 2&gt;&gt;find.err

[wood@node1 ~]$ wc find.out find.err  -l                                              #wc查看追加效果

合并多个输出：

[wood@node1 ~]$ (cal 2007 ; cal 2008 ) | less

[wood@node1 ~]$  cal 2007 ; cal 2008    | lpr     #只能打印出2008年日历

[wood@node1 ~]$ (cal 2007 ; cal 2008 ) | lpr      #打印2007和2008年日历

[wood@node1 ~]$ (date ; who | wc -l &gt;&gt;logfile)   #在文件logfile中维护登录的用户数量和时间戳

重导向到多个目标(tee)：

[root@node1 ~]# ls -Rl /etc|tee stage1.out|sort|tee stage2.out|uniq -c|tee stage3.out|sort -r|tee stage4.out