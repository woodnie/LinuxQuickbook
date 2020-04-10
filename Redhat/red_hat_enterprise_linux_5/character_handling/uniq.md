### uniq {#uniq}

uniq

NAME

      uniq - report or omit repeated lines    ## 报告或者省略重复的行

      -c, --count

             prefix lines by the number of occurrences          ##以前缀的形式标注每一行重复的次数

      -d, --repeated

             only print duplicate lines                                        ##只显示有重复的行，即只显示重复次数大于1的

      -D, --all-repeated[=delimit-method] print all duplicate lines

             delimit-method={none(default),prepend,separate}  Delimiting  is  done   with  blank lines.

      -f, --skip-fields=N

             avoid comparing the first N fields

      -i, --ignore-case

             ignore differences in case when comparing

      -s, --skip-chars=N

             avoid comparing the first N characters

      -u, --unique

             only print unique lines

      -w, --check-chars=N

             compare no more than N characters in lines

      --help display this help and exit

      --version

             output version information and exit

-u     #删除重复行

-d     #只显示重复行，且只显示一次

-c     #标注没一行出现的次数

-fn/-sn  #避免比较每一行的最前面的n个字段或者n个字符

[root@node ~]# cut -d: -f7 /etc/passwd | sort -u        #sort -u删除重复行

[root@node ~]# cut -d: -f7 /etc/passwd | sort | uniq