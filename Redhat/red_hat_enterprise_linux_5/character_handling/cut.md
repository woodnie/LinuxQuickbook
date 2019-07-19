### cut {#cut}

cut

NAME

      cut - remove sections from each line of files   ## 把某项内容从某个文件的每一行中筛选出来

      -b, --bytes=LIST

             select only these bytes

      -c, --characters=LIST

             select only these characters

      -d, --delimiter=DELIM

             use DELIM instead of TAB for field delimiter

      -f, --fields=LIST

             select  only  these  fields;  also print any line that contains no delimiter

             character, unless the -s option is specified

      -n     with -b: dont split multibyte characters

      --complement

             complement the set of selected bytes, characters or fields.

      -s, --only-delimited

             do not print lines not containing delimiters

      --output-delimiter=STRING

             use STRING as the output delimiter the default is to use the input delimiter  

-d 指定域分隔符

-f  指定要显示第几个域

-c 按照字符切割

例：

[root@localhost ~]#cut -d: -f7 passwd &gt; shells    ##找出本机中所有用户使用的各种shell并把其放置在一个文件名为shells的文件

                                                                                  ## -d：指定域分割付为冒号     -f7指定在第7个域进行相关操作

[root@node ~]# ifconfig | grep inet addr | cut -d: -f2 |cut -d -f1