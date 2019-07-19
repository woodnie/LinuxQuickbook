#### acl {#acl}

访问控制列表(ACL)

[root@node ~]# mount -o acl /directory                           #挂载文件系统使其支持ACl

[root@node ~]# getfacl file | directory                          #查看ACl信息

[root@node ~]# setfacl    -m   u:username:rwx file | directory   #创建基于群组的ACl

[root@node ~]# setfacl    -m   g:groupname:rw file | directory   #创建基于用户的ACl

[root@node ~]# setfacl    -m d:u:username:rw directory           #创建默认的ACl

[root@node ~]# setfacl -R -m   u:username:rwx directory          #创建递归的ACl

[root@node ~]# setfacl -x      u:username  file | directory      #删除用户ACL

[root@node ~]# setfacl -x      g:groupname file | directory      #删除群组ACL

[root@node ~]# setfacl -b                  file | directory      #删除所有ACL

[root@node ~]# setfacl --set       u: : ,g: : ,o: :

--set选项 会把原有的ACL项都删除，用新的替代，需要注意的是一定要包含UGO的全部设置，不能象-m一样只是添加ACL就可以了。

权限备份：

cp和mv都支持ACL，只是cp命令需要加上-p 参数。

tar  --acls 备份ACL信息

如果希望备份和恢复带有ACL的文件和目录，那么可以先把ACL备份到一个文件里。以后用--restore选项来回复这个文件中保存的ACL信息：

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

ACL 简介

用户权限管理始终是 Unix 系统管理中最重要的环节。大家对 Linux/Unix 的 UGO 权限管理方式一定不陌生，还有最常用的 chmod 命令。为了实现一些比较复杂的权限管理，往往不得不创建很多的组，并加以详细的记录和区分（很多时候就是管理员的噩梦）。可以针对某一个用户对某一文件指定一个权限，恐怕管理员都期待的功能。比如对某一个特定的文件，用户A可以读取，用户B所在的组可以修改，惟独用户B不可以……。于是就有了IEEE POSIX 1003.1e这个ACL的标准。所谓ACL，就是Access Control List，一个文件/目录的访问控制列表，可以针对任意指定的用户/组分配RWX权限。现在主流的商业Unix系统都支持ACL。FreeBSD也提供了对ACL的支持。Linux在这个方面也不会落后，从2.6版内核开始支持ACL。

准备工作

支持ACL需要内核和文件系统的支持。现在2.6内核配合EXT2/EXT3, JFS, XFS, ReiserFS等文件系统都是可以支持ACL的。用自己工作用的物理分区体验ACL，总是不明智的行为。万一误操作导致分区的损坏，造成数据的丢失，损失就大了。作一个loop设备是个安全的替代方法。这样不需要一个单独的分区，也不需要很大的硬盘空间，大约有个几百KB就足够进行我们的体验了。OK，下面我使用Fedora Core 5和Ext3文件开始对Linux的ACL的体验。

首先创建一个512KB的空白文件：

[root@FC3-vm opt]#  dd if=/dev/zero of=/opt/testptn count=512

512+0 records in

512+0 records out

和一个loop设备联系在一起：

[root@FC3-vm opt]#  losetup /dev/loop0 /opt/testptn

创建一个EXT2的文件系统：

[root@FC3-vm opt]#  mke2fs /dev/loop0

mke2fs 1.35 (28-Feb-2004)

max_blocks 262144, rsv_groups = 32, rsv_gdb = 0

Filesystem label=

OS type: Linux

Block size=1024 (log=0)

Fragment size=1024 (log=0)

32 inodes, 256 blocks

12 blocks (4.69%) reserved for the super user

First data block=1

1 block group

8192 blocks per group, 8192 fragments per group

32 inodes per group

Writing inode tables: done

Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 30 mounts or

180 days, whichever comes first. Use tune2fs -c or -i to override.

挂载新建的文件系统（注意mount选项里的acl标志，我们靠它来通知内核我们需要在这个文件系统中使用ACL）：

[root@FC3-vm opt]#  mount -o rw,acl /dev/loop0 /mnt

[root@FC3-vm opt]#  cd /mnt

[root@FC3-vm mnt]#  ls

lost+found

现在我已经得到了一个小型的文件系统。而且是支持ACL的。并且即使彻底损坏也不会影响硬盘上其他有价值的数据。可以开始我们的ACL体验之旅了。

体验1 － ACL的基本操作：添加和修改

我首先新建一个文件作为实施ACL的对象：

[root@FC3-vm mnt]#  touch file1

[root@FC3-vm mnt]#  ls -l file1

-rw-r--r-- 1 root root     7 Dec 11 00:28 file1

然后看一下这个文件缺省的ACL，这时这个文件除了通常的UGO的权限之外，并没有ACL：

[root@FC3-vm mnt]#  getfacl file1

# file: file1

# owner: root

# group: root

user::rw-

group::r--

other::r-

*注意：即使是不支持ACL的情况下，getfacl仍然能返回一个这样的结果。不过setfacl是不能工作的。

下面添加几个用户和组，一会我将使用ACL赋予他们不同的权限：

[root@FC3-vm mnt]#  groupadd testg1

[root@FC3-vm mnt]#  useradd testu1

[root@FC3-vm mnt]#  useradd testu2

[root@FC3-vm mnt]#  usermod -G testg1 testu1

现在我们看看testu1能做什么：

[root@FC3-vm mnt]# su testu1

[testu1@FC3-vm mnt]$ echo &quot;testu1&quot; &gt;&gt; file1

bash: file1: Permission denied

失败了。因为file1并不允许除了root以外的用户写。我们现在就通过修改file1的ACL赋予testu1足够的权限：

[root@FC3-vm mnt]# setfacl -m u:testu1:rw file1

[root@FC3-vm mnt]# su testu1

[testu1@FC3-vm mnt]$ echo &quot;testu1&quot; &gt;&gt; file1

[testu1@FC3-vm mnt]$ cat file1

testu1

修改成功了，用户testu1可以对file1做读写操作了。我们来看一下file1的ACL：

[testu1@FC3-vm mnt]$ getfacl file1

# file: file1

# owner: root

# group: root

user::rw-

user:testu1:rw-

group::r--

mask::rw-

other::r-

我们ls看一下：

[root@FC3-vm mnt]# ls -l file1

-rw-rw-r--+ 1 root root     7 Dec 11 00:28 file1

可以看到那个&quot;+&quot;了么？就在通常我们看到的权限位的旁边。这个说明file1设置了ACL， 接下来我们修改一下testu1的权限，同时给testg1这个组以读的权限：

[root@FC3-vm mnt]# setfacl -m u:testu1:rwx,g:testg1:r file1

[root@FC3-vm mnt]# getfacl file1

# file: file1

# owner: root

# group: root

user::rw-

user:testu1:rwx

group::r--

group:testg1:r--

mask::rwx

other::r-

可以看到设置后的权限，testu1已经有了执行的权限，而 testg1这个组也获得了读取文件内容的权限。也许有人已经注意到了两个问题：首先，file1的组权限从r--变成了rw-。其次，mask是什么？为什么也变化了呢？我们先从mask说起。如果说acl的优先级高于UGO，那么mask就是一个名副其实的最后一道防线。它决定了一个用户/组能够得到的最大的权限。这样我们在不破坏已有ACL的定义的基础上，可以临时提高或是降低安全级别：

[root@FC3-vm mnt]# setfacl -m mask::r file1

[root@FC3-vm mnt]# getfacl file1

# file: file1

# owner: root

# group: root

user::rw-

user:testu1:rwx                 #effective:r--

group::r--

group:testg1:r--

mask::r--

other::r--

[root@FC3-vm mnt]# ls -l file1

-rw-r--r--+ 1 root root 7 Dec 11 00:28 file1

在testu1对应的ACL项的后边出现了effective的字样，这是实际testu1得到的权限。Mask只对其他用户和组的权限有影响，对owner和other的权限是没有任何影响的。执行ls的结果也显示UGO的设置也有了对应的变化。因为在使用了ACL的情况下，group的权限显示的就是当前的mask。通常我们把mask设置成rwx，以不阻止任何的单个ACL项。

*需要注意的是，每次修改或添加某个用户或组的ACL项的时候，mask都会随之修改以使最新的修改能够真正生效。所以如果需要一个比较严格的mask的话，可能需要每次都重新设置一下mask。

体验2 － ACL的其他功能：删除和覆盖

我们来看一下其他的ACL操作。首先如何删除已有的ACL项呢？

[root@FC3-vm mnt]# setfacl -x g:testg1 file1

[root@FC3-vm mnt]# getfacl file1

# file: file1

# owner: root

# group: root

user::rw-

user:testu1:rwx

group::r--

mask::rwx

other::r--

我们看到testg1的权限已经被去掉了。如果需要去掉所有的ACL可以用-b选项。所有的ACL项都会被去掉。

[root@FC3-vm mnt]# setfacl -b file1

[root@FC3-vm mnt]# getfacl file1

# file: file1

# owner: root

# group: root

user::rw-

group::r--

other::r--

我们可以用--set 设置一些新的ACL项，并把原有的ACL项全部都覆盖掉。和-m不同，-m选项只是修改已有的配置或是新增加一些。--set选项会把原有的ACL项都删除，用新的替代，需要注意的是一定要包含UGO的设置，不能象-m一样只是添加ACL就可以了。比如下边这一段：

[root@FC3-vm mnt]# setfacl --set u::rw,u:testu1:rw,g::r,o::- file1

[root@FC3-vm mnt]# getfacl file1

# file: file1

# owner: root

# group: root

user::rw-

user:testu1:rw-

group::r--

mask::rw-

other::---

o::-是另一个需要注意的地方。其实完整的写法是other::---，正如u::rw的完整写法是user::rw-。通常我们可以把&quot;-&quot;省略，但是当权限位只包含&quot;-&quot;时，必须至少保留一个。如果写成了o::，就会出现错误。

如果希望对目录下的所有子目录都设置同样的ACL，可以使用-R参数：

[root@FC3-vm mnt]# setfacl --set u::rw,u:testu1:rw,g::r,o::- dir1

如果希望能从一个文件来读入ACL，并修改当前的文件的ACL，可以用-M参数：

[root@FC3-vm mnt]# cat test.acl

user:testu1:rw-

user:testu2:rw-

group:testg1:r--

group:testg2:r--

mask::rw-

other::---

体验3 － 目录的默认ACL

如果我们希望在一个目录中新建的文件和目录都使用同一个预定的ACL，那么我们可以使用默认(Default) ACL。在对一个目录设置了默认的ACL以后，每个在目录中创建的文件都会自动继承目录的默认ACL作为自己的ACL。用setfacl的-d选项就可以做到这一点：

[root@FC3-vm mnt]# setfacl -d --set g:testg1:rwx dir1

[root@FC3-vm mnt]# getfacl dir1

# file: dir1

# owner: root

# group: root

user::rwx

group::r-x

other::r-x

default:user::rwx

default:group::r-x

default:group:testg1:rwx

default:mask::rwx

default:other::r-x

可以看到默认ACL已经被设置了。建立一个文件试试：

[root@FC3-vm mnt]# touch dir1/file1

[root@FC3-vm mnt]# getfacl dir1/file1

# file: dir1/file1

# owner: root

# group: root

user::rw-

group::r-x                      #effective:r--

group:testg1:rwx                #effective:rw-

mask::rw-

other::r--

file1自动继承了dir1对testg1设置的ACL。只是由于mask的存在使得testg1只能获得rw-权限。

体验4 － 备份和恢复ACL

主要的文件操作命令cp和mv都支持ACL，只是cp命令需要加上-p 参数。但是tar等常见的备份工具是不会保留目录和文件的ACL信息的。 如果希望备份和恢复带有ACL的文件和目录，那么可以先把ACL备份到一个文件里。以后用--restore选项来回复这个文件中保存的ACL信息：

[root@FC3-vm mnt]# getfacl -R dir1 &gt; dir1.acl

[root@FC3-vm mnt]# ls -l dir1.acl

total 16

-rw-r--r--  1 root root   310 Dec 12 21:10 dir1.acl

我们用-b选项删除所有的ACL数据，来模拟从备份中回复的文件和目录：

[root@FC3-vm mnt]# setfacl -R -b dir1

[root@FC3-vm mnt]# getfacl -R dir1

# file: dir1

# owner: root

# group: root

user::rwx

group::r-x

other::r-x

# file: dir1/file1

# owner: root

# group: root

user::rw-

group::r--

other::r--

现在我们从dir1.acl中恢复被删除的ACL信息：

[root@FC3-vm mnt]# setfacl --restore dir1.acl

[root@FC3-vm mnt]# getfacl -R dir1

# file: dir1

# owner: root

# group: root

user::rwx

group::r-x

other::r-x

default:user::rwx

default:group::r-x

default:group:testg1:rwx

default:mask::rwx

default:other::r-x

# file: dir1/file1

# owner: root

# group: root

user::rw-

group::r-x                      #effective:r--

group:testg1:rwx                #effective:rw-

mask::rw-

other::r--

结语

ACL 的引入使得大规模的复杂权限管理可以很容易的在 Linux 上实现。对于 /home 这样存放大量用户文件的分区，可以做到更有效的管理。但是我们也看到在备份工具等方面的欠缺，好在 FC2 中已经开始包含了 star 这样的支持 ACL 的备份工具，虽然还是 alpha 版。

在单个文件的 ACL 条目的数量上，不同的文件系统有不同的限制。Ext2 和 Ext3 只能支持每个文件 25 个 ACL 条目。ReiserFS 和 JFS 可以支持超过 8,000 个条目。这个方面 Ext* 文件系统还需要加强。

无论多么复杂的系统中，文件系统的权限管理都是最基础的内容。而 Linux 对 ACL的支持，无疑是一把管理海量用户系统的利器，对 Linux 在大规模的企业级应用中更方便的发挥更大的作用添了一把火。