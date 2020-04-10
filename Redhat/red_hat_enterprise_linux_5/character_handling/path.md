### path {#path}

patch

NAME

      patch - apply a diff file to an original

[root@node test]# duff     -u   file1  file2 &gt;file.patch      # 生成diff补丁文件passwd.patch；-u生成同意格式

[root@node test]# patch  -b   file1  file.patch                #用补丁文件file.patch修改file1，使其与file2相同；-b 会自动创建.orig为扩展名的备份文件

[root@node test]# patch  -b   file1  file.orig                   #用patch命令组东生成的备份文件file.orig来撤销改变                  

[root@node test]# patch  -R   file1  file.patch                # -R同样是撤销改变

patch 会改变selinux的语境，在使用某些问价之前必须重设语境：

[root@node ~]# restore filename

[root@node test]# ll

-rw-r--r-- 1 root root   file1                    

-rw-r--r-- 1 root root   file2

-rw-r--r-- 1 root root   file.orig               #原始的file文件备份

-rw-r--r-- 1 root root   file.patch            #由diff生成的补丁文件

-rw-r--r-- 1 root root   file.rej