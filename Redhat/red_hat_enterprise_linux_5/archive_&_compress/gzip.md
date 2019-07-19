### gzip {#gzip}

gzip:

[root@localhost ~]# gzip           ## 压缩gzip

[root@localhost ~]# gunzip       ##解压缩gunzip

e.g.:

[root@localhost ~]# gzip  etc.gz  #gzip压缩tar打包后的文件

必要参数

　-a|--ascii ASCII模式

　-c|--stdout|--to-stdout        保留原始文件，把压缩后的文集输出到标准输出

　-d|--decompress|----uncompress 解开压缩文件

　-f|--force     强制压缩文件

　-l|--list      压缩文件的信息列表

　-n|--no-name   压缩文件时，不保存原来的文件名称及时间戳记

　-N|--name 　   压缩文件时，保存原来的文件名称及时间戳记

　-q|--quiet     不显示警告信息

　-r|--recursive 同时处理指定目录下的所有文件和子目录

　-t|--test      测试压缩文件是否正确性

　-v|--verbose   显示详细的处理过程

　-&lt;压缩效率&gt; 1-9 的数值，默认为6,数值越大压缩率越高

选择参数

　-L|--license   显示版本与版权信息

　-V|--version   显示版本信息

-h|--help       显示帮助信息

--fast 　       此参数的效果和指定&quot;-1&quot;参数相同

--best 　       此参数的效果和指定&quot;-9&quot;参数相同

范例                

范例1：压缩文件

[root@node ~]#  gzip * //压缩目录下的所有文件

范例2：列出详细的信息

[root@9linux a]# gzip -dv * //解压文件，并列出详细信息

范例3：显示压缩文件的信息

[root@9linux a]# gzip -l *