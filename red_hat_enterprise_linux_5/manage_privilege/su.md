### su {#su}

su

NAME

      su - run a shell with substitute user and group IDs

[root@node ~]# su  [ - ]  [user]

[root@node ~]# su  [ - ]  [user]  -c command      

带有- 选项，建立新的用户登录shell，使用新用户的shell即环境变量（就像是另一个用户登录一样）

不带- 选项，保留当前用户的shell和环境变量