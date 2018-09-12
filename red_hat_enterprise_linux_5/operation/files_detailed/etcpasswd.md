#### /etc/passwd {#etc-passwd}

/etc/passwd

[root@node ~]# cat /etc/passwd

root:x:0:0:root:/root:/bin/bash

1     :2:3:4:5     :6     :7

1\. 用户名

2.口令占位符(由于历史原因)

3.UID

4.主要群组GID

5.GECOS字段（典型包含用户的真实姓名）

6.主目录

7.登录shell

[root@node ~]# vipw 专用的passwd编辑工具