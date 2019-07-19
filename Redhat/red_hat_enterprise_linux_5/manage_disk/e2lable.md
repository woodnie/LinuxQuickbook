### e2lable {#e2lable}

e2lable

NAME

      e2label - Change the label on an ext2/ext3 filesystem

      更改ext2/ext3 文件系统标签

SYNOPSIS

      e2label device [ new-label ]

e.g.:

[root@node ~]# e2label /dev/sdb1 datadisk   #在相应的文件系统中，把标签写入超级块

[root@node ~]# e2label /dev/sdb1                   #在相应的文件系统中，生成标签

[root@node ~]# mount LABEL=datadisk /data  #使用标签挂载

[root@node ~]# cat /etc/fstab   #添加到fstab中

......

LABEL=datadisk          /data                   ext3    defaults        0 0

......

[root@node ~]# mount -a         #重新挂载fstab