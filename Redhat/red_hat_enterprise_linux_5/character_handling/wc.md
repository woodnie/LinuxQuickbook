### wc {#wc}

wc

NAME

      wc - print the number of newlines, words, and bytes in files    ## 计算一个文件的行数／字符数／大小等

其他参说明：

     -c, --bytes

             print the byte counts

      -m, --chars

             print the character counts

      -l, --lines

             print the newline counts

      -L, --max-line-length

             print the length of the longest line

      -w, --words

             print the word counts

      --help display this help and exit

      --version

             output version information and exit

-c  计算字节数

-l   计算行数

-w 计算单词数

-m 计算字符总数

例：

[root@localhost ~]# wc -l passwd        ##   -l  参数  指定计算passwd文件有多少行