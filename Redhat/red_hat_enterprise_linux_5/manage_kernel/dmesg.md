### dmesg {#dmesg}

dmesg

功能说明：显示开机信息。

语　　法：dmesg [-cn][-s &lt;缓冲区大小&gt;]

补充说明：kernel会将开机信息存储在ring buffer中。您若是开机时来不及查看信息，可利用dmesg来查看。开机信息亦保存在/var/log目录中，名称为dmesg的文件里。

参　　数：

-c 　显示信息后，清除ring buffer中的内容。

-s&lt;缓冲区大小&gt; 　预设置为8196，刚好等于ring buffer的大小。

-n 　设置记录信息的层级。