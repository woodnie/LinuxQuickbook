### tee {#tee}

tee

NAME

      tee - read from standard input and write to standard output and files

[root@node1 ~]# ./sysconfig-backup.sh | tee backup$(date +%Y%m%d)   # 把shell script 的执行结果输出到屏幕的同时保存到文件