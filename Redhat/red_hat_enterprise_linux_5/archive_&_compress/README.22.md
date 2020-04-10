## Archive &amp; Compress {#archive-compress}

归档和压缩

tar:

[root@localhost ~]#tar   -cvf   /路径/文件名.tar            目标目录        #tar打包命令，把 目标目录 下的文件打包，打包后的文件名为

[root@localhost ~]#tar   -xvf   /路径/文件名.tar    -C    目的目录        #tar解包命令，把目标文件 name.tar    解压缩到 目的目录

[root@localhost ~]#tar -zcvf   /路径/文件名.gz            目标目录         #tar压缩gzip

[root@localhost ~]#tar -zxvf   /路径/文件名.gz    -C    目的目录         #tar解压缩gunzip

[root@localhost ~]# tar -jcvf   /路径/文件名.bz2           目标目录         #tar压缩bzip2

[root@localhost ~]# tar -jxvf   /路径/文件名.bz2   -C    目的目录         #tar解压缩bunzip2

行动参数 (需要一个):

-c 创建归档

-t 列举归档

-x 从归档中抽取文件

可选参数:

- z：使用gzip压缩

- j：使用bzip2来压缩

- v：详细输出

- f：指定文件或设备

--acl：      归档时保存ACL权限

--selinux：归档时保存selinux权限

--xattrs (--acl --selinux)：同时保存acl和selinux权限

详细参数:

-A 新增压缩文件到已存在的压缩

-B 设置区块大小

-c 建立新的压缩文件

-d 记录文件的差别

-r 添加文件到已经压缩的文件

-u 添加改变了和现有的文件到已经存在的压缩文件

-x 从压缩的文件中提取文件

-t 显示压缩文件的内容

-z 支持gzip解压文件

-j 支持bzip2解压文件

-Z 支持compress解压文件

-v 显示操作过程

-l 文件系统边界设置

-k 保留原有文件不覆盖

-m 保留文件不被覆盖

-W 确认压缩文件的正确性

选择参数

-b 设置区块数目

-C 切换到指定目录

-f 指定压缩文件

--help 显示帮助信息

--version 显示版本信息

zip:

[root@localhost ~]# zip

[root@localhost ~]# unzip

e.g.:

[root@localhost ~]# zip etc.zip /etc  #zip压缩/etc目录

[root@localhost ~]# unzip etc.zip     #unzip解压缩etc.zip

gzip:

[root@localhost ~]# gzip           ##压缩gzip

[root@localhost ~]# gunzip       ##解压缩gunzip

e.g.:

[root@localhost ~]# gzip -v etc.tar  #gzip压缩tar打包后的文件

bzip2

[root@localhost ~]# bzip2         ##tar压缩bzip2

[root@localhost ~]# bunzip2     ##tar解压缩bunzip2

e.g.:

[root@localhost ~]# bzip2 -v etc.tar  #bzip2压缩tar打包后的文件

【常见解压/压缩命令】

tar

解包：tar xvf FileName.tar

打包：tar cvf FileName.tar DirName

（注：tar是打包，不是压缩！）

----------------------------

.gz

解压1：gunzip FileName.gz

解压2：gzip -d FileName.gz

压缩：gzip FileName

.tar.gz 和 .tgz

解压：tar zxvf FileName.tar.gz

压缩：tar zcvf FileName.tar.gz DirName

----------------------------

.bz2

解压1：bzip2 -d FileName.bz2

解压2：bunzip2 FileName.bz2

压缩： bzip2 -z FileName

.tar.bz2

解压：tar jxvf FileName.tar.bz2

压缩：tar jcvf FileName.tar.bz2 DirName

----------------------------

.bz

解压1：bzip2 -d FileName.bz

解压2：bunzip2 FileName.bz

压缩：未知

.tar.bz

解压：tar jxvf FileName.tar.bz

压缩：未知

----------------------------

.Z

解压：uncompress FileName.Z

压缩：compress FileName

.tar.Z

解压：tar Zxvf FileName.tar.Z

压缩：tar Zcvf FileName.tar.Z DirName

----------------------------

.zip

解压：unzip FileName.zip

压缩：zip FileName.zip DirName

----------------------------

.rar

解压：rar x FileName.rar

压缩：rar a FileName.rar DirName