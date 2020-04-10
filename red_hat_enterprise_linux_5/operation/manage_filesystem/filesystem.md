#### filesystem {#filesystem}

linux 文件系统

磁盘启动器被划分成分区（partition）

分区被格式化成文件系统（filesystem）

分区被LVM,RAID整合，格式化成文件系统

superblock:記錄此 filesystem 的整體資訊，包括inode/block的總量、使用量、剩餘量， 以及檔案系統的格式與相關資訊等；

inode(index node): 記錄檔案的權限與相關屬性，一個檔案佔用一個inode，同時記錄此檔案的資料所在的 block 號碼；

block(data block ): 實際記錄檔案的內容，若檔案太大時，會佔用多個 block 。

       如果我的檔案系統高達數百GB時， 那麼將所有的 inode 與 block 通通放置在一起將是很不智的決定，因為 inode 與 block 的數量太龐大，不容易管理。

       為此之故，因此 Ext2 檔案系統在格式化的時候基本上是區分為多個區塊群組 (block group) 的，每個區塊群組都有獨立的 inode/block/superblock 系統。

Inodes

inode 表（inode table）包含ext2或ext3文件系统中的所有文件的列表

inode（index node，索引节点）是该表中的项目，所包含的关于文件信息（元数据，metadata）有：

文件类型、权限、 UID, GID

连接计数 (指向这个文件的路径名数量)

文件的大小和各类时间戳

到文件在磁盘上的数据块的指针

其它关于文件的数据

inode和目录

计算机使用inode号码（inode number）来引用文件

人使用文件名（file name）来引用文件

目录（directory）是文件名和inode号码之间的映射表

文件名被保存在目录中，而不是inode表中，因此才可以使多个文件名指向同一个inode号码

一个目录的inde表

名字：通过父目录来关联inode

inode元数据：属性和磁盘上块的指针

内容：

       目录：name/indoe列表（被显示）

       文件：任何数据

cp 和 inodes

cp命令:

1.分配一个未用的inode号码，在inode表中添加一个新项目

2.在目录中创建一个dentry，关联文件名和inode号码

3.把数据复制到新文件中

mv 和 inodes

如果mv命令的目标和源文件所在的文件系统相同，mv命令就会：

1.使用新文件名新建目录项目

2.删除带有原有文件名的原有目录项目

对inode表无影响（除了时间戳以外），对数据在磁盘上的位置

也无影响：不会移动任何数据！

如果目标是不同的文件系统，mv命令的行为就是复制和删除

rm 和 inodes

rm命令:

1.减少连接数量

2.把数据块放在可用空间列表中

3.删除目录项目

数据实际上没有被删除，但是当数据块被另一个文件使用时，原

来的数据就会被覆盖

[root@node ~]# ln     /etc/passwd passwd_h

[root@node ~]# ln -s /etc/passwd passwd_s

[root@node ~]# ll -i

559385 -rw-r--r-- 2 root root 2309 11-15 21:24 passwd_h

623577 lrwxrwxrwx 1 root root   11 11-16 00:44 passwd_s -&gt; /etc/passwd

[root@node ~]# ll -i /etc/passwd

559385 -rw-r--r-- 2 root root 2309 11-15 21:24 /etc/passwd

硬连接

1.不能跨越驱动器或分区，因为硬链接和源文件使用同一个inode表

2.不能创建到目录的硬链接

硬连接添加了一个额外的访问点（路径名）来指代某个文件

文件系统上的一个常规的物理文件

每个目录都引用相同的inode号码

创建硬链接增加连接数量

rm命令减少连接数量

只要至少有一个连接存在，文件就存在

当连接数量减少到零时，文件就会别删除                                                                            

语法:

ln 文件名[连接名]

符号连接 (软连接)

1.符号链接拥有自己inode表，是与源文件不同的独立文件

2.符号链接是一种特殊的文件，其文件类型用 l 表示

3.符号链接本身的权限无关紧要，只与源文件相关

4.符号链接的内容是源文件的路径名

语法:

ln -s 文件名 连接名

基本文件类型：

- #常规文件

d #目录

l #符号链接

b #块特殊文件

c #字符特殊文件

p #被命名的管道

s #套接字

note：

1.文件(包括目录)

名称可以长达255个字符。

除了/以外，所有字符都是有效的

但 ： &gt;   &lt;   ?   *   &quot;   , 引号   空格  制表符   非打印符  等符号也应避免使用。

使用引号来存取文件名包含的特殊字符。

2.绝对路径/相对路径

[root@node ~]# cd      #转换到用户主目录

[root@node ~]# cd ..   #转换到当前目录的上级目录

[root@node ~]# cd -    #转换到上一个工作的目录

3.当前系统支持的文件系统类型：

/lib/modules/2.6.18-194.el5/kernel/fs/