### tune2fs {#tune2fs}

tune2fs

NAME

      tune2fs - adjust tunable filesystem parameters on ext2/ext3 filesystems

      调整ext2/ext3文件系统参数

e.g.:

[root@node ~]# tune2fs  -m  10   /dev/sda1  #修改保留块的比例

注：当保留块比例低于5%，系统会因随便而导致性能降低

[root@node ~]# tune2fs  -o   acl   /dev/sda1  #设定默认挂载选项

[root@node ~]# tune2fs  -i0  -c0   /dev/sda1  #禁用强制文件系统检查

[root@node ~]# tune2fs -l

[root@node ~]# dumpe2fs /dev/sda1  #浏览 /dev/sda1 文件系统参数