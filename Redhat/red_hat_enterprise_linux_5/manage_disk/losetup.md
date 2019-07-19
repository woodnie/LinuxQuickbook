### losetup {#losetup}

losetup

[root@station10 ~]# for i in $(seq 0 6); do dd if=/dev/zero of=./vdisk$i bs=1M count=50 ;done

[root@station10 ~]# for i in $(seq 0 6); do losetup /dev/loop$i  vdisk$i

/dev/loop[0 6] 就可像本地磁盘一样使用