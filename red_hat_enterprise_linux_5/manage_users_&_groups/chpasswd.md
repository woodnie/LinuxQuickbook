### chpasswd {#chpasswd}

chpasswd

[root@node ~]# neuusers       userlist           # 用户列文件表格式类似/etc/passwd；但不会复制/etc/skel中的文件

[root@node ~]# chpasswd  &lt; userpasswd  #用户密码文件

e.g.:(批量添加用户)

[root@node ~]# cat userlist

student1:x:1001:1001:student1:/home/student1:/bin/bash

student2:x:1002:1002:student2:/home/student2:/bin/bash

student3:x:1003:1003:student3:/home/student3:/bin/bash

student4:x:1004:1004:student4:/home/student4:/bin/bash

student5:x:1005:1005:student5:/home/student5:/bin/bash

[root@node ~]# cat userpasswd

student1:student1

student2:student2

student3:student3

student4:student4

student5:student5

[root@node ~]# neuusers     userlist

[root@node ~]# chpasswd  &lt;userpasswd