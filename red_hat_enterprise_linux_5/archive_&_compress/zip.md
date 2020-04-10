### zip {#zip}

zip

必要参数-A 自动解压文件-c  给压缩文件加注释-d  删除文件-F 修复损坏文件-k  兼容 DOS-m 压缩完毕后，删除园文件-q  运行时不显示信息处理信息-r  处理指定目录和指定目录下的使用子目录-v  显示信息的处理信息-y  保留符号链接

选择参数-b&lt;目录&gt;   指定压缩到的目录-i&lt;格式&gt;    匹配格式进行压缩-L             显示版权信息-t&lt;日期&gt;   指定压缩文件的日期-&lt;压缩率&gt; 指定压缩率

范例1：压缩文件

[root@9linux a]# zip -v cp.zip a.c b.c c.c e.c

adding: a.c (in=0) (out=0) (stored 0%)

adding: b.c (in=0) (out=0) (stored 0%)

adding: c.c (in=0) (out=0) (stored 0%)

adding: e.c (in=0) (out=0) (stored 0%)

total bytes=0, compressed=0 -&gt; 0% savings

范例2：压缩文件

[root@9linux a]# [root@9linux a]# zip -v cp2.zip *

范例3：压缩目录

[root@9linux a]# zip -r cp3.zip /root/

范例4：从压缩文件中删除文件

[root@9linux a]# zip -dv cp.zip a.c