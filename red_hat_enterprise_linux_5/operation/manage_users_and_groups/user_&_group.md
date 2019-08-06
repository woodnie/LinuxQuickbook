#### user &amp; group {#user-group}

用户和组管理

[root@node ~]# system-config-users  #图形化用户管理工具

[root@localhost ~]# useradd                   username                               ##创建用户

[root@localhost ~]# useradd    -u  uid    username                               ##创建用户的同时指定用户ID

[root@localhost ~]# groupadd                groupname                             ##创建组

[root@localhost ~]# groupadd  -g  gid   groupname                             ##创建组的同时指定组ID

[root@localhost ~]# userdel     -r             username

[root@localhost ~]# usermod   -G           groupname     username       ##把用户加入到指定组中

主要组群：/etc/paswd

次要组群：/etc/group

[wood@node ~]$ newgrp 组群名     #临时改变组群名

相关文件：

/etc/passwd

/etc/shadow

/etc/group

/etc/gshadow

文件umask:

r   W   X   -

4   2   1   0

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

普通用户：

[wood@node ~]$ umask

0002

目录属性：775=0775=0777-0002      

文件属性：664=0664=0666-0002

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

root用户：

[root@node ~]# umask

0022

目录属性：755=0755=0777-0022

文件属性：644=0644=0666-0022

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

特殊权限

#umask 0002 最高位用于扩展属性

suid:使用该用户命令的所属用户的权限来运行，而不是命令执行者的权限

sgid:使用命令的组群权限来运行

sticky bit:在带有粘附位的目录中，文件只能被属主和root用户删除，无论这个目录设置什么样儿的权限

suid            4  针对文件                   属主执行位替换成s             4775    #设置用户suid

guid           2  针对文件,目录            属组执行位替换成s             2775    #设置用户guid

sticky bit    1   针对目录                   其他用户执行位替换成t       1777    #设置其他用户sticky bit

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++