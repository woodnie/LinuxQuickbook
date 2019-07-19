### manage mememort {#manage-mememort}

1.vim /etc/sysctl.conf

添加：

vm.swappiness=

这个数值越低就是叫OS尽量使用物理内存，数值越高就是叫OS尽量使用SWAP，如果我的OS本来物理MEM就不多的话，那么这个vm.vm.swappiness比较高才对

2./proc/sys/vm/swappiness=0 #禁用了所有进程使用swap

3.swapoff/on