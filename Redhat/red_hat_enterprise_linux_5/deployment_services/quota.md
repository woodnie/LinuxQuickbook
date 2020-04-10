### quota {#quota}

quota

在/home 分区上对用户和组设置磁盘配额：

准备：1.单独的/home目录分区

           2.测试账户test

1.设置分区挂载选项：usrquota,grpquota

[root@localhost ~]# vi /etc/fstab

LABEL=/home             /home                   ext3    defaults,usrquota,grpquota        1 2

[root@localhost ~]# mount -o remount /home     ##重新挂载/home分区

2.建一个用于保存用户/组的配额数据库文件

[root@localhost ~]# init1   #最好将系统引导至单用户模式，来确定正确的配额计算

[root@localhost ~]# quotacheck -cugm  /home

-c  创建新的磁盘配额

-u  用户相关，结合/etc/fstab 中 usrquota

-g  组相关，结合/etc/fstab 中 grpquota

-m 强迫在&quot;读、写&quot;模式下检查硬盘的配额

[root@localhost ~]# quotacheck -c  /home   ##可以只用-c参数

[root@localhost ~]# init 5 #离开 leve 1

[root@localhost ~]# mount -o remount /home

3.设置用户/组的配额:

3.1edquota

[root@localhost ~]#edquota -u  test

Disk quotas for user test (uid 502):

 Filesystem                   blocks       soft       hard     inodes     soft     hard

 /dev/hda3                        48       4096       5120          6       40       50

##blocks以使用的块儿数,系统会显示已使用的blocks,一般不要修改  

##inodes以使用的节点数,系统会显示已使用的inodes,一般不要修改

blocks:

##soft，hard 软硬限制，值为0即无限制，单位 KB

indones:

##soft，hard 软硬限制，值为0即无限制，单位 个   #空文件不会引起quota个数限制

3.2setquota

[root@localhost ~]# setquota test 4096 5120 40 50 /home  #直接在shell中设置配额

4.打开配额功能

[root@localhost ~]# quotaon  -a           #开启整个系统的配额

[root@localhost ~]# quotaon   /home   #开启指定分区的配额

5.设定soft quota和hard quota之间的时间（宽限时间）

[root@localhost ~]# edquota -t

6.复制配额

[root@localhost ~]# edquota -p user1   user2   ##把user1用户的配额设置复制给user2用户

7.查看配额状态

quota       #擦看用户磁盘用量和配额

repquota  #生成所有用户的磁盘用量报告

warnquota

e.g.:

[root@node ~]# quota 用户名

[root@node ~]# repquota 分区

8.关闭配额功能

[root@localhost ~]# quotaoff   /home

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

quota语法：

[root @test /root ]# quota [-guvs] [user,group]

参数说明：

-g  ：显示 group 群组

-u  ：显示 user

-v  ：显示 quota 的值

-s  ：选择 inod 或 硬盘空间来显示

范例：

[root @test /root ]# quota -guvs    &lt;==显示目前执行者（就是 root ）的 quota 值

[root @test /root ]# quota -uvs test &lt;==显示 test 这个使用者的 quota 值

quotacheck语法：

[root @test /root ]# quotacheck [-auvg] /yourpath

参数说明：

-a  ：扫瞄所有在 /etc/mtab 里头已经 mount 的具有 quota 支持的磁盘

-u  ：扫瞄使用者的档案与目录

-v  ：显示扫瞄过程

-g  ：扫瞄群组使用的档案与目录

-m　：强制进行 quotacheck  

范例：

范例一、要针对 /home 这个 partition 进行 quota 的规划：

[root@test root ]# quotacheck -uvg /home       &lt;==开始扫瞄 /home 这一个独立扇区的目录

quotacheck: Scanning /dev/hda3 [/home] done      &lt;==显示 /home 扇区为 /dev/hda3 ！

quotacheck: Checked 35 directories and 342 files &lt;==扫瞄完毕，有 35 目录与 342 档案。

[root@test root ]# ls -l /home          &lt;==查看一下 /home 这个目录底下，两个档案产生了！

-rw-------    1 root     root         7168 May  6 18:37 aquota.group

-rw-------    1 root     root         7168 May  6 18:37 aquota.user

关于 quotacheck 发生错误的解决方法：

# 有些时候，在新版的 Linux distribution 当中，进行 quotacheck 时，可能会出现

# quotacheck: Cannot get quotafile name for /dev/hda3

# quotacheck: Cannot get quotafile name for /dev/hda3

# 这可能是新版的 quota 在设计时的小问题，解决的方法有两个：

[root@test root]# quotacheck -uvgm  

# 加上 -m 的参数来强制进行，或者是：

[root@test root]# touch /home/aquota.user; touch /home/aquota.group

[root@test root]# quotacheck -uvg

# 既然 quotacheck 找不到 quotafile ，那么我就手动将 quotafile 建立起来即可！

# 然后再重新进行 quotacheck 一次即可！

# 注意喔！因为我的 /dev/hda3 对应到 /home ，所以当然就是在 /home 底下建立起 qoutafile 了！

edquota语法：

[root @test /root ]# edquota [-u user] [-g group] [-t]

[root @test /root ]# edquota -p user_demo -u user

参数说明：

-u  ：编辑 user 的 quota

-g  ：编辑 group 的 quota

-t  ：编辑宽限时间（就是超过 quota 值后，还能使用硬盘的宽限期限）

-p  ：copy 模板（以建立好的使用者或群组）到另一个使用者（或群组）

范例：

[root @test /root ]# edquota -u test        &lt;==设定 test 这个使用者的 quota 数值，会直接进入 vi 画面

Disk quotas for user test (uid 501):

 Filesystem                   blocks       soft       hard     inodes     soft     hard

 /dev/hda3                         8          0          0          5        0        0

修改一下成为：

Disk quotas for user test (uid 501):

 Filesystem                   blocks       soft       hard     inodes     soft     hard

 /dev/hda3                         8       50005000          5     50005000

[root @test /root ]# edquota -p test -u test2  &lt;==将 test 这个人的 quota 资料复制给 test2 这个人！

[root @test /root ]# edquota -t         &lt;==设定宽限时间，也就是超过 quota 值之后的修正时间啦！

Grace period before enforcing soft limits for users:

Time units may be: days, hours, minutes, or seconds

 Filesystem             Block grace period     Inode grace period

 /dev/hda3                  0minutes               0minutes

上面的 0minutes 可以改成 60minutes 即可！也就是 60 分钟之内必须要赶快整理硬盘的意思！

quotaon语法：

[root @test /root ]# quotaon [-a] [-uvg directory]

参数说明：

-a  ：全部的 quota 设定都启动（会自动去寻找 /etc/mtab 的设定）

-u  ：使用者的 quota 启动

-g  ：群组的 quota 设定启动

-s  ：显示讯息

范例：

[root @test /root ]# quotaon -a         &lt;==全部的 quota 限制都启动

[root @test /root ]# quotaon -uv /home  &lt;==只有激活 /home 底下的使用者 quota 限额，group 不激活！

quotaoff语法：

[root @test /root ]# quotaoff -a

参数说明：

-a  ：全部的 quota 设定都关闭（会自动去寻找 /etc/mtab 的设定）

范例：

[root @test /root ]# quotaoff -a         &lt;==全部的 quota 限制都关闭了！