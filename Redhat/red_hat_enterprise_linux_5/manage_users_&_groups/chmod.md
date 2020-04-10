### chmod {#chmod}

chmod

[root@localhost ~]#chmod     掩码   文件/目录名    ##更改文件目录属性

[root@localhost ~]#chmod   u|g|o|a  =|+|-|     r|w|x|s|t

例：

[root@localhost ~]#chmod   755   dir

[root@localhost ~]#chmod   644   file

[root@localhost ~]#chmod   g+s   commen_dir  #给公共文件夹设置sgid权限，是公共组群用户都可访问