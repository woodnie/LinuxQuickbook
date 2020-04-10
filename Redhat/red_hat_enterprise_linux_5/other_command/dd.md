### dd {#dd}

dd

功能说明：读取，转换并输出数据。

语　　法：

补充说明：dd可从标准输入或文件读取数据，依指定的格式来转换数据，再输出到文件，设备或标准输出。

参　　数：

if=&lt;文件&gt;      从文件读取

of=&lt;文件&gt;      输出到文件

count=&lt;区块数&gt; 仅读取指定的区块数

bs=&lt;字节数&gt;    将ibs( 输入)与obs(输出)设成指定的字节数

cbs=&lt;字节数&gt;   转换时，每次只转换指定的字节数

conv=&lt;关键字&gt;  指定文件转换的方式

ibs=&lt;字节数&gt;   每次读取的字节数

obs=&lt;字节数&gt;   每次输出的字节数

seek=&lt;区块数&gt;  一开始输出时，跳过指定的区块数

skip=&lt;区块数&gt;  一开始读取时，跳过指定的区块数

--help        帮助

--version     显示版本信息

e.g.:

[root@localhost ~]# dd if=/dev/zero of=bigfile bs=1MB count=3

[root@localhost ~]# dd if=/dev/sda of=mbrfile bs=512 count=1         ##备份MBR