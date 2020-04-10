### archive &amp; compress {#archive-compress}

归档和压缩

tar:

[root@localhost ~]#tar   -cvf    name.tar             目标目录         ##tar打包命令，把 目标目录 下的文件打包，打包后的文件名为name.tar

[root@localhost ~]#tar   -xvf    name.tar      -C   目的目录         ##tar解压缩命令，把目标文件 name.tar    解压缩到 目的目录

[root@localhost ~]#tar -zcvf  name.gzip             目标目录         ##tar压缩gzip

[root@localhost ~]#tar -zxvf  name.gzip        -C 目的目录         ##tar解压缩gunzip

[root@localhost ~]# tar -jcvf   name.bzip2          目标目录         ##tar压缩bzip2

[root@localhost ~]# tar -jxvf   name.bzip2     -C 目的目录         ##tar解压缩bunzip2

参数：

c：压缩

x：解压缩

v：显示打包文件信息

f ：打包成一个文件

gzip:

[root@localhost ~]#gzip           ##tar压缩gzip

[root@localhost ~]#gunzip       ##tar解压缩gunzip

bzip2

[root@localhost ~]#bzip2         ##tar压缩bzip2

[root@localhost ~]#bunzip2     ##tar解压缩bunzip2

用rmt命令写入远程磁带设备

归档工具: dump/restore

备份及重建ext2/ext3文件系统

   不要与其它文件系统混用

   应该只能在卸载的文件系统或者只读的文件系统中使用dump

可以进行全部或者增量备份

例:

dump -0u -f /dev/nst0 /dev/hda2

restore -rf /dev/nst0

归档工具: rsync

高效率地在远程系统之间复制文件

使用安全的ssh连接作为传输方式

   rsync *.conf barney:/home/joe/configs/

对scp更快；复制相似文件间的不同