### bzip2 {#bzip2}

bzip2

[root@localhost ~]# bzip2         ##tar 压缩bzip2

[root@localhost ~]# bunzip2     ##tar解压缩bunzip2

e.g.:

[root@localhost ~]# bzip2 -v etc.tar  #bzip2压缩tar打包后的文件

必要参数

-c|--stdout 　压缩或者压缩的结果输出到标准输出

-d|--decompress 　执行解压缩。

-f|--force 　强制解压，覆盖同名文件

-k|--keep 　bzip2在压缩或解压缩后，不删除原文件。

-s|--small 　降低程序执行时内存的使用量。

-t|--test 　检查.bz2压缩文件的完整性。 但不解压

-v|--verbose 　 显示详细的信息。

-z|--compress 　强制执行压缩。

-L|--license 显示软件版本信息

-1|当压缩时将块的大小设置为100kb

-9|当压缩时将块的大小设置为900kb

-q 不显示警告信息

选择参数

--help 显示帮助信息

--version 显示版本信息

范例1：解压.bz2文件

[root@9linux ~]# bzip2 -v temp.bz2 //解压文件显示详细处理信息

范例2：压缩文件

[root@9linux ~]# bzip2 -c a.c b.c c.c

范例3：检查文件完整性

[root@9linux ~]# bzip2 -t temp.bz