### useradd {#useradd}

useradd

[root@localhost ~]# useradd                    username                                         # 创建用户

[root@localhost ~]# useradd                    username            -p password        #创建用户，指定密码  

[root@localhost ~]# useradd    -u  uid     username                                          #创建用户的同时指定用户ID

[root@localhost ~]# useradd  -s/bin/false  -d/dev/null   username     #创建一个非登录用户，用户shell为/bin/false ，用户家目录为/dev/null

e.g.:

[root@localhost ~]# useradd -g clamav -s/bin/false -d/dev/null clamav