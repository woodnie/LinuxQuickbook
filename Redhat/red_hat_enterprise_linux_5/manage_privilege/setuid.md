### setuid {#setuid}

setuid

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

如果在执行位有一个小写字母，SUID或SGID权限打开，并且执行权限打开

如果目录的&quot;其他&quot;用户执行权限是关闭的，那么粘贴位将显示为T