### mkfs {#mkfs}

mkfs

NAME

      mkfs - build a Linux file system

mkfs.ext2

mkfs.ext3

mkfs.msdos

mkfs.vfat

...

e.g.:

[root@node ~]# mkfs.ext3 -L opt -b 2048 -i 4096 /dev/sdc1 # 将文件系统标为opt ，使用2KB块单位，每4KB空间分配一个节点。