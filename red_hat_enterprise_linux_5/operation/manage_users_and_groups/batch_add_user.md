#### batch add user {#batch-add-user}

批量添加用户设置密码

1.使用userlist  &amp; chpasswd

[root@node ~]# neuusers       userlist           #userlist：用户列文件表格式类似/etc/passwd；但不会复制/etc/skel中的文件

[root@node ~]# chpasswd  &lt; userpasswd   #userpasswd：用户密码文件

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

2.使用for  &amp; mkpasswd

[root@node ~]# cat useradd_student.sh  #自动添加用户,用户名序列规律

     1 #!/bin/bash

     2 #This shell script add user student0-student10

     3 touch /tmp/userlog

     4 for x in $(seq 0 9);

     5 do

     6 echo adding user student$x

     7 (

     8 echo -ne &quot;student$x\t&quot;

     9 useradd -u 100$x student$x  &amp;&amp; mkpasswd student$x     #mkpasswd 生成随机密码

    10 ) &gt;&gt; /tmp/userlog

    11 done

    12 echo &quot;passwd saved is /tmp/userlog&quot;

    13 cat /tmp/userlog

3.使用for passwd --stdin(用户序列)

[root@node ~]# cat    #自动添加用户,从用户序列

#!/bin/bash

#

for NAME in joshua alex dax bryan zak ed manager;

do

       useradd $NAME

       #PASSWORD=$(openssl rand -base64 10)  #10个字符长度的随机字符串

       echo password | passwd --stdin $NAME

       echo &quot;name is: $NAME  passwdord is: password &quot;|mail -s &quot;Account Info&quot; root@node

done

4.使用for passwd --stdin(用户列表文件)

[root@node ~]# cat    #自动添加用户,从文件userlist

#!/bin/bash

#

for NAME in $(cat /root/userlist)

do

       useradd $NAME

       #PASSWORD=$(openssl rand -base64 10)

       echo $PASSWORD |passwd --stdin $NAME

       echo &quot;name is: $NAME  passwdord is: $PASSWORD&quot;|mail -s &quot;Account Info&quot; root@node

done