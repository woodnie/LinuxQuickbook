### pgrep {#pgrep}

pgrep

NAME

      pgrep, pkill - look up or signal processes based on name and other attributes

进程名模糊匹配，搜索进程PID

[root@node ~]# ps axo pid,comm|grep cups

[root@node ~]# pgrep  cups

[root@node ~]# pgrep  -U root

[root@node ~]# pgrep  -G wood