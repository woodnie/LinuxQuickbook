## glibc {#glibc}

[root@web ~]# ls  -l /lib64/libc.so.6

lrwxrwxrwx. 1 root root 12 Aug 19 10:24 /lib64/libc.so.6 -&gt; libc-2.12.so

LD_PRELOAD

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/glibc-2.15/lib