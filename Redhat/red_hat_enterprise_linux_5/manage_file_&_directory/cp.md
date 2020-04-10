### cp {#cp}

cp

cp - copy files and directories

-i  : ( 交互，interactive) 在覆盖文件前询问

-r  : (递归，recursive)   递归地复制整个目录树

-p : (保留，preserve)    保留权限，所有者，时间戳

-a : (归档，archive)       递归地复制文件和目录(和 -r 相似)，同时保留权限(和 -p 相似)

-v  :(冗长，verbose）   explain what is being done)

-a 等同于&quot;-dpR&quot;

-b 诺删除或者覆盖目标文件将对文件进行备份，备份的文件以备份的字符结尾

-d 复制符号链接

-f 强制复制

-i 交互模式，覆盖目标文件之前要进行询问

-l 建立硬连链接，非复制

-p 源目录目录或文件的属性保留

-P 源文件或文件的路径保留

-r 处理指定目录以及目录的子目录下的所有文件

-R 通&quot;-r&quot;选项相同

-s 不进行复制，而是建立符号链接

-u 只在源文件更新时进行复制

-v 运行时显示详细的处理信息

-x 只在源文件和目标文件文件系统类型相同时才复制

最少需要2个参数，源目录和目地目录

当参数超过两个：除了最后一个参数以外的所有参数都会被解释成源文件

                               最后一个参数被解释成目标目录

[root@node ~]#cp  / 源目录  /  *    / 目标目录  /    -r   ## 把目录1下所有内容拷贝到目录2

[root@node ~]# cp -av /var/log/    /lv1/var/

[root@node ~]# cp -av /var/log/*  /lv1/var/

##########################

cp命令强制覆盖方式实现

方法一：

输入alias命令，看到系统内部使用的是cp的别名。

#alias

alias cp=cp -i

输入unalias cp命令，解除别名。

#unaslias cp   （这只是临时取消cp的别名，不是永久的）

#cp a test/a        呵呵，这下正常了吧。

方法二：

输入\cp命令，作用也是取消cp的别名。

#\cp a test\a    呵呵，这么用也一样好使。

方法三：

输入yes|cp a test\a，使用管道自动输入yes。

#yes | cp a test\a   看到了吧，自动打出一堆yes，替你输入了。