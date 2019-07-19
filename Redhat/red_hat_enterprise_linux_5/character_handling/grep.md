### grep {#grep}

grep

[root@localhost ~]#grep       关键字              # 过滤出包含该关键字的行

[root@localhost ~]#grep     关键字串       # 过滤出包含该关键字的行， 中包含一组（用空格等分隔开的）字符串

e.g.:

[root@node ~]# ifconfig | grep inet addr  ## 过滤 inet addr , 需使用

[root@localhost ~]#grep    ^ 关键字      # 过滤出以关键字起始的行

[root@localhost ~]#grep     关键字 $      # 过滤出以关键字结尾的行

[root@node ~]# grep bash$     /etc/passwd |cut -d: -f1   # 显示 passwd 中使用 bash shell 的账号

[root@node ~]# grep -v bash$ /etc/passwd |cut -d: -f1   # 显示 passwd 中不使用 bash shell 的账号

[root@localhost ~]#grep    -v   关键字                             ## 过滤出不包含该关键字的行

[root@localhost ~]#grep    -Anum         关键字              ## 过滤出包含该关键字上下多少行的行数，匹配后 num 行

[root@localhost ~]#grep    -Bnum         关键字              ## 过滤出包含该关键字上下多少行的行数，匹配钱 num 行

[root@node ~]# grep     -i   关键字       # 不区分关键字的大小写

[root@node ~]# grep     -c   关键字      # 计算包含该关键字的行数

[root@node ~]# grep     -n   关键字      # 显示包含该关键字在原文件中所在的行号

[root@node ~]# grep       关键字       / 目录名 /*      # 显示某个目录下包含该关键字的文件和相关内容

[root@node ~]# grep    -l             关键字       / 目录名 /*      # 列出某个目录下包含该关键字的文件

[root@node ~]# grep    -l  -R/-r   关键字      / 目录名 /*        # 列出某个目录下包含该关键字的文件 ,-R, -r, --recursive

e.g.:

[wood@node ~]$ grep -l mac /etc/sysconfig/* 2&gt;/dev/null   ## 显示 /etc/sysconfig/ 目录下，那个文件包含 mac 关键字

[root@node1 ~]# grep ^#[[:space:]]chkconfig:[[:space:]][[:digit:]]\+ /etc/init.d/*   # 默认在系统引导时运行

[root@node1 ~]# grep ^#[[:space:]]chkconfig:[[:space:]]-     # 默认在系统引导时不运行

e.g.:

1 、想列出 /usr/share/dict/words 中包含先有字母 t 然后有一个元音字母，之后是 sh 的单词，命令为：

2 、在 /usr/share/dict/words 文件中，创建可以符合 abominable ， abominate ， anomie 和 atomize 的正则表达式，但是不要选到别的单词。

3 、在 /usr/share/dict/words 文件中包含多少先有字母 t 然后有一个元音字母，之后是 sh 的单词，只输出数量。

4 、列出 /usr/share/dict/words 中刚好包含 16 个字母的单词：

5 、我们将要使用 /usr/share/doc 文件夹来完成我们的下几个任务。

      列出 /usr/share/doc/bash-3.1 文件夹中，所有包含单词 expansion 的文件

6 、显示出&quot; Linux &quot;在 /usr/share/doc/bash-3.1 文件夹的文件中出现的次数，但是不要显示没有这个单词的文件。

      提示：先列出所有的文件，然后想如何使输出符合要求：

7 、在 /usr/share/doc/ 列出所有包含 Havoc 的文件名：

1 、 [root@localhost ~]#grep &quot;^t[aiuoe]sh&quot; /usr/share/dict/words;

2 、 [root@localhost ~]#grep &quot;a[bnt]omi.*[ltiz]e$&quot; /usr/share/dict/words;

3 、 [root@localhost ~]#grep -c &quot;^t[aiuoe]sh&quot; /usr/share/dict/words;

4 、 [root@localhost ~]#grep &quot;^.[a-z]\{14\}.$&quot;  /usr/share/dicr/words;

6 、 [root@localhost ~]#grep - c linux /usr/share/doc/bash-3.1/* |grep - v &quot; :0 &quot;

7 、 [root@localhost ~]#grep - R - l Havoc /usr/share/doc/