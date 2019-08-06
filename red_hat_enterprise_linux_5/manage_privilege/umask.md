### umask {#umask}

umask

1\. 文件权限

文件类型和权限被一个10个字符长的字符串代表

e.g.:

[root@node ~]# ll |cut -d -f1  

-rw-------

-rw-r--r--

drwx------

drwxr-xr-x

drwxrwxr-x

lrwxrwxrwx

2.文件掩码属性:

r   W   X   -

4   2    1   0

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

普通用户：

[wood@node ~]$ umask

0002

目录属性：775=0775=0777-0002      

文件属性：664=0664=0666-0002

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

root用户：

[root@node ~]# umask

0022

目录属性：755=0755=0777-0022

文件属性：644=0644=0666-0022

e.g.:

[root@node temp]# ll  #文件

-rw-r--r-- 1 root root 0 Oct 22 14:59 file    #默认权限644

-rw------- 1 root root 0 Oct 22 15:00 file1   #只有属主有读写权限600

-r-------- 1 root root 0 Oct 22 15:00 file2   #只有属主有读权限400

[root@node temp]# ll  #目录

drwxr-xr-x 2 root root 4096 Oct 22 15:05 dir   #默认权限755

drwx------ 2 root root 4096 Oct 22 15:05 dir1  #只有属主有读写权限700

dr-x------ 2 root root 4096 Oct 22 15:05 dir2  #只有属主有读权限500

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

3.特殊权限

#umask 0002 最高位用于扩展属性

suid:使用该用户命令的所属用户的权限来运行，而不是命令执行者的权限

sgid:使用命令的组群权限来运行

sticky bit:在带有粘附位的目录中，文件只能被属主和root用户删除，无论这个目录设置什么样儿的权限

suid           4  针对文件                    属主执行位替换成s               4775    #设置用户suid

guid           2  针对文件,目录            属组执行位替换成s               2775    #设置用户guid

sticky bit    1  针对目录                    其他用户执行位替换成t         1777    #设置其他用户sticky bit

e.g.:

[root@node ~]# chmod g+s  目录   #SGID用于合作目录，使具有相同群组的用户，对同一文件拥有相同权限

[root@node ~]# chmod o+t   目录   #有粘贴位设定的目录，只有文件的拥有者才可以删除该文件

[root@node ~]# chmod 2770 /project/file*

[root@node ~]# chmod 2770 /project

[root@node ~]# ll -d /project/

drwxrwsr-x 2 student student 4096 11-22 23:29 /project/

[root@node ~]# ll /project/  

-rwxrws--T 1 student1 student 9 11-22 23:29 file1

-rwxrws--T 1 student1 student 9 11-22 23:29 file2

如果在执行位有一个大写字母，SUID或SGID权限打开，但是执行权限关闭

如果在执行位有一个大写字母，SUID或SGID权限打开，并且执行权限打开

如果目录的&quot;其他&quot;用户执行权限是关闭的，那么粘贴位将显示为T

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

用户可在 .bashrc 中设置自己的umask

用户可在  /etc/skel/.bashrc 中设置相应的umask,为以后添加用户指定默认ugo权限