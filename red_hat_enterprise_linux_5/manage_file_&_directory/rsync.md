### rsync {#rsync}

rsync

NAME

      rsync - faster, flexible replacement for rcp

-e 命令           #指定一个外部的，和rsh兼容的程序（通常是ssh）来连接

-a                    #循环进入子目录，保持权限、所有权等

-r                     #循环进入子目录，但是不保持权限等

--partial           #继续下载被部分下载了的文件

--progress      #显示传输进度

-P                    #和 --partial --progress相同

rsync有六种不同的工作模式：

1.拷贝本地文件；当SRC和DES路径信息都不包含有单个冒号&quot;:&quot;分隔符时就启动这种工作模式。

2.使用一个远程shell程序（如rsh、ssh）来实现将本地机器的内容拷贝到远程机器。当DST路径地址包含单个冒号&quot;:&quot;分隔符时启动该模式。

3.使用一个远程shell程序（如rsh、ssh）来实现将远程机器的内容拷贝到本地机器。当SRC地址路径包含单个冒号&quot;:&quot;分隔符时启动该模式。

4.从远程rsync服务器中拷贝文件到本地机。当SRC路径信息包含&quot;::&quot;分隔符时启动该模式。

5.从本地机器拷贝文件到远程rsync服务器中。当DST路径信息包含&quot;::&quot;分隔符时启动该模式。

6.列远程机的文件列表。这类似于rsync传输，不过只要在命令中省略掉本地机信息即可。

rsync 命令实例

1 显示目录内容

命令:

a) rsync &lt;dst-dir&gt;

b) rsync -r &lt;dst-dir&gt;

c) rsync  jack@192.168.0.1::&lt;dst-dir&gt;

d) rsync  ssh_user@192.168.0.1:&lt;dst-dir&gt;

命令说明

a) 显示&lt;dst-dir&gt;目录内容(第一层)

b) 递归显示&lt;dst-dir&gt;目录内容

c) 显示远程主机&lt;dst-dir&gt;目录内容

  *注1：端口模式, 基于rsync用户的身份验证

  *注2：rsync server上的目录必须具有xx7的权限.

d) 查看远程主机&lt;dst-dir&gt;目录内容

  *注1：remote shell模式, 通过ssh连接的基于系统本地用户的身份验证

  *注2：这里只使用了一个冒号(:)，同时用户名是远程主机的ssh用户，密码也是ssh用户对应的密码。

  *注3：使用&quot;&lt;dst-dir&gt;&quot;，则列出&lt;dst-dir&gt;文件夹本身的信息。若要列出&lt;dst-dir&gt;文件夹内容，应使用&quot;&lt;dst-dir&gt;/&quot;。

2本地目录之间同步

命令

a) rsync -av  --progress &lt;src-dir&gt;/ &lt;dst-dir&gt;             *** 注意(/) ***

b) rsync -av  --progress &lt;src-dir&gt;  &lt;dst-dir&gt;

c) rsync -avu --progress --delete &lt;src-dir&gt;/  &lt;dst-dir&gt;

d) rsync -av  --progress --temp-dir=/tmp &lt;src-dir&gt;/ &lt;dst-dir&gt;

命令说明

a) 同步src-dir目录下所有文件到dst-dir目录下

b) 同步src-dir目录下所有文件到dst-dir/src-dir目录下

c) 对src-dir目录内容向dst-dir目录下进行差异更新，有增加/更新则添加替换，有减少则对其删减

d) 比a)多了--temp-dir=/tmp，即指定/tmp为临时交换区，这样可以避免因目标目录空间不够引起的无法同步文件的错误。

3 异地主机之间同步

命令

a) rsync -avz  --progress &lt;src-dir&gt; jack@192.168.0.1::&lt;dst-dir&gt;/

b) rsync -avz  --progress &lt;src-dir&gt; jack@192.168.0.1::&lt;dst-dir&gt;/ --password-file=/home/jack/rsync.jack

c) rsync -avuz  --progress --delete &lt;src-dir&gt; jack@192.168.0.1::&lt;dst-dir&gt;/ --password-file=/home/jack/rsync.jack

d) rsync -avz  --progress jack@192.168.0.1::&lt;dst-dir&gt;/&lt;src-dir&gt; &lt;dst-dir&gt;

命令说明

a) 同步本地&lt;src-dir&gt;目录的内容到远程主机192.168.0.1的&lt;dst-dir&gt;目录下，jack是rsync数据库用户(参见3\. /etc/rsync.secrets)

b) 通过自动读取用户密码而实现非交互登录文件同步

c) 较b)多了-u和--delete

d) 同步远程主机内容到本地目录

选项说明

-v, --verbose 详细模式输出

-q, --quiet 精简输出模式

-c, --checksum 打开校验开关，强制对文件传输进行校验

-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD

-r, --recursive 对子目录以递归模式处理

-R, --relative 使用相对路径信息

rsync foo/bar/foo.c remote:/tmp/

则在/tmp目录下创建foo.c文件，而如果使用-R参数：

rsync -R foo/bar/foo.c remote:/tmp/

则会创建文件/tmp/foo/bar/foo.c，也就是会保持完全路径信息。

-b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix选项来指定不同的备份文件前缀。

--backup-dir 将备份文件(如~filename)存放在在目录下。

-suffix=SUFFIX 定义备份文件前缀

-u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件。(不覆盖更新的文件)

-l, --links 保留软链结

-L, --copy-links 想对待常规文件一样处理软链结

--copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结

--safe-links 忽略指向SRC路径目录树以外的链结

-H, --hard-links 保留硬链结

-p, --perms 保持文件权限

-o, --owner 保持文件属主信息

-g, --group 保持文件属组信息

-D, --devices 保持设备文件信息

-t, --times 保持文件时间信息

-S, --sparse 对稀疏文件进行特殊处理以节省DST的空间

-n, --dry-run现实哪些文件将被传输

-W, --whole-file 拷贝文件，不进行增量检测

-x, --one-file-system 不要跨越文件系统边界

-B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节

-e, --rsh=COMMAND 指定替代rsh的shell程序

rsync -e ssh user@host:/dir/file

[root@node2 ~]# rsync -e ssh backup@192.168.1.201:/data/* /var/ftp/    #使用ssh代替rsh,同步server上的数据到本地/var/ftp/pub

--rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息

-C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件

--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件

--delete 删除那些DST中SRC没有的文件

--delete-excluded 同样删除接收端那些被该选项指定排除的文件

--delete-after 传输结束以后再删除

--ignore-errors 及时出现IO错误也进行删除

--max-delete=NUM 最多删除NUM个文件

--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输

--force 强制删除目录，即使不为空

--numeric-ids 不将数字的用户和组ID匹配为用户名和组名

--timeout=TIME IP超时时间，单位为秒

-I, --ignore-times 不跳过那些有同样的时间和长度的文件

--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间

--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0

-T --temp-dir=DIR 在DIR中创建临时文件

--compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份

-P 等同于 --partial

--progress 显示备份过程

-z, --compress 对备份的文件在传输时进行压缩处理

--exclude=PATTERN 指定排除不需要传输的文件模式

--include=PATTERN 指定不排除而需要传输的文件模式

--exclude-from=FILE 排除FILE中指定模式的文件

--include-from=FILE 不排除FILE指定模式匹配的文件

--version 打印版本信息

--address 绑定到特定的地址

--config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件

--port=PORT 指定其他的rsync服务端口

--blocking-io 对远程shell使用阻塞IO

-stats 给出某些文件的传输状态

--progress 在传输时现实传输过程

--log-format=formAT 指定日志文件格式

--password-file=FILE 从FILE中得到密码

--password-file   引用192.168.0.1上rsync用户jack口令的本地文件，创建方法如下

                 shell&gt; echo &quot;jackpwd&quot; &gt;&gt; /home/jack/rsync.jack

                 shell&gt; chown jack:wheel /home/jack/rsync.jack

                 shell&gt; chmod 600 /home/jack/rsync.jack

--bwlimit=KBPS 限制I/O带宽，KBytes per second

-h, --help 显示帮助信息