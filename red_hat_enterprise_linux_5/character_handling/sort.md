### sort {#sort}

sort

NAME

      sort - sort lines of text files   ## 文本文件排序

-r 反向整理

-n 按照数字大小整理

-f  忽略（fold）大小写

-u 删除输出中的重复（unique）

-t  c  指定字符c作为字符定界符(cut -d)

-k X 整理-t 选项处理后的结果(cut -f)

[root@node ~]# sort -t : -k 3 -n  /etc/passwd   #以UID排序

[root@node ~]# sort -t : -k 4 -n  /etc/passwd   #以GID排序

[root@node ~]# cut -d: -f7 /etc/passwd | sort -u  #查看系统所使用的shell

Other options:

      -b, --ignore-leading-blanks

             ignore leading blanks

      -d, --dictionary-order

             consider only blanks and alphanumeric characters

      -f, --ignore-case

             fold lower case to upper case characters

      -g, --general-numeric-sort

             compare according to general numerical value

      -i, --ignore-nonprinting

             consider only printable characters

      -M, --month-sort

             compare (unknown) &lt; JAN &lt; ... &lt; DEC

      -n, --numeric-sort

             compare according to string numerical value

      -r, --reverse

             reverse the result of comparisons                             ##反向排序

      -c, --check

             check whether input is sorted; do not sort

      -k, --key=POS1[,POS2]

             start a key at POS1, end it at POS2 (origin 1)

      -m, --merge

             merge already sorted files; do not sort

      -o, --output=FILE

             write result to FILE instead of standard output

      -s, --stable

             stabilize sort by disabling last-resort comparison

      -S, --buffer-size=SIZE

             use SIZE for main memory buffer

      -t, --field-separator=SEP

             use SEP instead of non-blank to blank transition

      -T, --temporary-directory=DIR

             use DIR for temporaries, not $TMPDIR or /tmp; multiple options specify  multiple directories

      -u, --unique

             with  -c, check for strict ordering; without -c, output only the first of an equal run

      -z, --zero-terminated

             end lines with 0 byte, not newline

      --help display this help and exit

      --version

             output version information and exit

EXP:

1.拷贝/etc/passwd到你的主目录下:$ cd$ cp /etc/passwd .

2\. 在/etc/passwd里面有系统里的每一个帐户.使用wc,在passwd文件里计算有多少行。$ wc -l passwd在你的系统里有多少个帐户____________

3\. 找出本机中所有用户使用的各种shell并把其放置在一个文件内： $ cut -d: -f7 passwd &gt; shells

4\. 使用cat命令查看你新的shells文件的内容，为了使输出结果更为友好.用sort命令输出这些数据在一个新的文件里： $ sort shells &gt; sorted.shells

5\. 你的文件包含许多同样的内容.使用uniq命令可以计算出有多少个相同的行：$ uniq -c sorted.shells &gt; uniq.sorted.shells

   为什么在使用uniq之前要使用sort命令(uniq以空行为域分割符 ？)

6\. 按照数字由大到小的顺序列出在你的机器上使用的各种shell: $ sort -nr uniq.sorted.shellsi. /sbin/nologin6 /bin/bash1 /sbin/shutdown1 /sbin/halt1 /bin/sync