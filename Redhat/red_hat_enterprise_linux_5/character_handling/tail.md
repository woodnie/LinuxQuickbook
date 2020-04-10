### tail {#tail}

tail

NAME

      tail - output the last part of files

[root@node ~]# tail                   /etc/passwd   # 默认显示文件前10行

[root@node ~]# tail  -n  num    /etc/passwd   #指定显示行数

[root@node ~]# tail  -f  /vcr/log/message      #显示文件后半部分（默认最后10行），并显示文件增长内容