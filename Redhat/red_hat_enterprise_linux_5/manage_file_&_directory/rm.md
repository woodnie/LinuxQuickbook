### rm {#rm}

rm

rm - remove files or directories

能否在摸个目录下使用删除操作，要看目录是否具有WX权限 。

-i：交互式

-r：递归式

-f：强制

[root@localhost ~]rm -f    filename      ##删除文件夹并且不提示

[root@localhost ~]rm -rf   dir_name    ##删除非空文件夹（r 递归，f 强制且无提示）

[root@localhost ~]rm -rf  - -文件名       ##删除以&quot;-&quot;开头的文件