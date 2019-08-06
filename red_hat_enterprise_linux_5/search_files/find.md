### find {#find}

find

1\. 文件名查找

[root@localhost ~]# find dir  -name      &quot;filename  &quot;       #在指定目录中，指定文件名搜索，最好只用&quot;&quot; 或者

[root@localhost ~]# find dir  -i               &quot;filename  &quot;       #不区分大小写，搜索

2.用户名查找

與使用者或群組名稱有關的參數：

-uid n ：n 為數字，這個數字是使用者的帳號 ID，亦即 UID ，這個 UID 是記錄在 /etc/passwd 裡面與帳號名稱對應的數字。這方面我們會在第四篇介紹。

-gid n ：n 為數字，這個數字是群組名稱的 ID，亦即 GID，這個 GID 記錄在 /etc/group，相關的介紹我們會第四篇說明～

-user name ：name 為使用者帳號名稱喔！例如 dmtsai

-group name：name 為群組名稱喔，例如 users ；

-nouser    ：尋找檔案的擁有者不存在 /etc/passwd 的人！

-nogroup   ：尋找檔案的擁有群組不存在於 /etc/group 的檔案！

               當你自行安裝軟體時，很可能該軟體的屬性當中並沒有檔案擁有者，

               這是可能的！在這個時候，就可以使用 -nouser 與 -nogroup 搜尋。

-mount /-xdev 只查找当前文件系统

[root@localhost ~]# find dir  -user     username        #在指定目录中，指定用户名，搜索关于该用户的文件

[root@localhost ~]# find dir  -group   groupname     #在指定目录中，指定用户名，搜索关于该用户的文件

[root@localhost ~]# find dir  -uid       uid                      #在指定目录中，指定用户ID，搜索关于该用户的文件

[root@localhost ~]# find dir  -gid       gid                      #在指定目录中，指定群组ID，搜索关于该用户的文件

[root@localhost ~]# find dir  -user     username   -group  groupname      #在指定目录中，指定用户名/组名，搜索关于该用户的文件

3.逻辑查找

[root@localhost ~]# find dir  -not -user  username         #在指定目录中，搜索不属于该用户的文件

[root@localhost ~]# find dir  ! -user       username         #在指定目录中，搜索不属于该用户的文件

[root@localhost ~]# find dir  -user   username  -not -group  groupname      #在指定目录中，指定用户名排除组名，搜索关于该用户的文件

[root@localhost ~]# find dir  -user   username1   -o   username2      # -o(OR运算)搜索属于用户1或者用户2的文件

[root@localhost ~]# find dir  -not \( -user   username1   -o   username2 \)     # 搜索既不属于用户1也不属于用户2的文件

[root@localhost ~]# find dir  -and

4.指定文件权限查找

[root@node ~]# find  / -perm 755      #匹配权限模式恰好是755的文件

[root@node ~]# find  / -perm +222    #只有当任何人都有写权限时， +：3个位置，其中一个位置上的权限匹配即可

[root@node ~]# find  / -perm  -222    #只有当每个人都有写权限时，   -：3个位置，其中每个位置上的权限都必须匹配

e.g.:

[root@node ~]# find / -perm -002 -exec chmod o-w {} \;   #x修正能被other群组写入的文件

5.文件大小条件查找

[root@localhost ~]# find dir  -size

e.g.:

[root@localhost ~]# find dir  -size  1024K     #正好1024K

[root@localhost ~]# find dir  -size  +1024K   #大于1024K

[root@localhost ~]# find dir  -size  -1024K    #小于1024K

6.存取时间查找

[root@node ~]# stat passwd   #查看一个文件的时间戳

 File: `passwd

 Size: 2268            Blocks: 8          IO Block: 4096   regular file

Device: fd00h/64768d    Inode: 622695      Links: 1

Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)

Access: 2010-11-14 00:03:07.000000000 +0800

Modify: 2010-11-14 00:02:44.000000000 +0800

Change: 2010-11-14 00:06:17.000000000 +0800

以天为单位(n*24 hours)：

[root@node ~]# find -atime   #文件最后一次被访问多少天

[root@node ~]# find -mtime  #文件数据最后一次被修改多少天

[root@node ~]# find -ctime   #文件状态最后一次被改变多少天

以分钟为单位(n minutes)：

[root@node ~]# find -amin    #文件最后一次被访问多少分钟

[root@node ~]# find -mmin   #文件数据最后一次被修改多少分钟

[root@node ~]# find -cmin    #文件状态最后一次被改变多少分钟

e.g.:

[root@node ~]# find / -ctime  -10  #查找文件修改日期小于10天的文件

[root@node ~]# find / -ctime +10  #查找文件修改日期大于10天的文件

[root@node ~]# find / -ctime   10  #查找文件修改日期等于10天的文件

[root@node ~]# find / -mmin    -30     #文件最后一次被修改小于30分钟

[root@node ~]# find / -mmin    +30    #文件最后一次被修改大于30分钟

[root@node ~]# find / -mmin      30    #文件最后一次被修改等于30分钟

7.文件类型查找

[root@localhost ~]# find

-type TYPE ：搜尋檔案的類型為 TYPE 的，類型主要有：一般正規檔案 (f),裝置檔案 (b, c), 目錄 (d), 連結檔 (l), socket (s), 及 FIFO (p) 等屬性。

8.使用find来执行命令

[root@localhost ~]# find dir  -ok     comand {} \;      #对搜索到的文件执行相关命令，-ok    运行命令之前    会提示

[root@localhost ~]# find dir  -exec comand {} \;      #对搜索到的文件执行相关命令，-exec运行命令之前不会提示

[root@localhost ~]# find                                               #-print        ：將結果列印到螢幕上，這個動作是預設動作！

e.g:

[root@node ~]# find / -name &quot;*.conf&quot; -exec cp {}  /root/conf_back/ \;   #把所有.conf文件备份到/root/conf_back

[root@node ~]# find /tmp -atime +3 -user wood -ok rm -r {} \;               #删除/tmp下属于wood的大于3天的文件

[root@node ~]# find / -perm -002 -exec chmod o-w {} \                         #修正能被other群组写入的文件

此类命令必须以 空格\; 结尾。{} 代表find找到的文件。

find以；作为定界符。

shell也是以；作为定界符。

所有需要使用反斜线 \ 防止shell解释它：

当一个字符前面加反斜线 \ 后，bash就会按这个字符的本意解释，因此在bash命令提示下键入 \; 会在bash解释过后才把；发送给find命令。

e.g:

[root@node ~]# find /var -user root -group mail

[root@node ~]# find / -not -user root -not -user student -not -user wood  -ls 2&gt;/dev/null

[root@node ~]# find /usr/bin/ -size +50k -ls 2&gt;/dev/null

[root@node ~]# find /etc/mail -exec file {} \; 2&gt;/dev/null

[root@node ~]# find /etc -type l         # -type l 符号链接

[root@node ~]# find /etc -type l -ls    # -ls 生成ls -l 列表

[root@node ~]# find /tmp -user root -cmin +120 -type f -ls   # -type -f 普通文件

[root@node ~]# find / -perm -u+s -ls      #查找启用了SetUID权限位的文件

[root@node ~]# find / -perm +4000 -ls  #查找启用了SetUID权限位的文件

e.g.:查找可疑文件

find /  \(-nouser -o -nogroup \)  #查找不包含在/etc/passwd /etc/group中的用户和组

find / -type f -perm -002 #查找其他用户有写入权限的文件

find / -type d -perm -2    #查找其他用户有写入权限的目录

e.g.：

使用find命令

1.在/var/lib目录下查找所有文件其所有者是games用户的文件$ find /var/lib -user games 2&gt; /dev/null

2.在/var目录下查找所有文件其所有者是root用户的文件。

3.查找所有文件其所有者不是root，bin和student用户并用长格式显示（如ls -l 的显示结果）。

4.查找/usr/bin目录下所有大小超过一百万byte的文件并用长格式显示（如ls -l 的显示结果）。

5.对/etc/mail目录下的所有文件使用file命令.

6.查找/tmp目录下属于student的所有普通文件，这些文件的修改时间为120分钟以前，查询结果用长格式显示（如ls -l 的显示结果）。

7\. 对于查到的上述文件，用-ok选项删除。

1.[root@localhost ~]# find /var/lib -user games 2&gt; /dev/null

2.[root@localhost ~]# find /var -user root -group mail 2&gt;/dev/mull

3.[root@localhost ~]# find / -not -user root -not -user bin -not -user student -ls 2&gt; /dev/null

3.[root@localhost ~]# find / ! -user root ! -user bin ! -user student -exec ls -ld {} \; 2&gt; /dev/null

4.[root@localhost ~]# find /usr/bin -size +1000000c -exec ls -l {} \;2&gt; /dev/null

5.[root@localhost ~]# find /etc/maill -exec file {} \; 2 &gt; /dev/null

6.[root@localhost ~]# find /tmp -user student -and -mmin +120 -and -type f -ls 2&gt; /dev/null

7.[root@localhost ~]# find /tmp -user student -and -mmin +120 -and -type f -ok rm {} \;