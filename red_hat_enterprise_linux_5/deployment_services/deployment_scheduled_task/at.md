#### at {#at}

at

NAME

      at, batch, atq, atrm - queue, examine or delete jobs for later execution # 从标准输入或者指定文件读取命令，这些命令将在一段时间之后执行

[root@node ~]#  at time                  #创建

[root@node ~]#  at -l                        #列举

[root@node ~]#  atq                         #列举

[root@node ~]#  at -d  作业号码     #删除

[root@node ~]#  atrm  作业号码     #删除

[root@node ~]#  at -c  作业号码     #详情