#### create lv snapshot {#create-lv-snapshot}

创建逻辑卷快照

快照是特殊的逻辑卷，它是在生成快照时存在的逻辑卷的准确拷贝

对于需要备份或者复制的现有数据集临时拷贝以及其它操作来说，快照是最合适的选择

快照只有在它们和原来的逻辑卷不同时才会消耗空间

      在生成快照时会分配给它一定的空间，但只有在原来的逻辑卷或者快照有所改变时才会使用这些空间

      当原来的逻辑卷中有所改变时，会将旧的数据复制到快照中

      快照中只含有原来的逻辑卷中更改的数据或者自生成快照后的快照中更改的数

[root@node ~]# lvcreate -L 256M -s -n testvg-snapshot /dev/testvg/testlv    #创建一个增量256M ，/dev/testvg/testlv 的快照/dev/testvg/testlv-snapshot

[root@node ~]# lvcreate -L 256M -p r    -s -n testvg-snapshot /dev/testvg/testlv #创建只读    快照

[root@node ~]# lvcreate -L 256M -p rw -s -n testvg-snapshot /dev/testvg/testlv #创建只读写快照

[root@node ~]# lvremove /dev/testvg/testlv-snapshot   #移除快照