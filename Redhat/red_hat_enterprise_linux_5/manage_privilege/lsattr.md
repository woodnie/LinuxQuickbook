### lsattr {#lsattr}

lsattr ( 显示文件目录隐藏属性)

[root@www ~]# lsattr [-adR] 文件或目录

参数说明：

-a ：将隐藏文件的属性也显示出来

-R ：连同子目录的数据一并显示出来

[root@www tmp]# chattr +aij attrtest

[root@www tmp]# lsattr attrtest

----ia---j--- attrtest