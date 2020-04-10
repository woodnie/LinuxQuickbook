#### /etc/skel {#etc-skel}

/etc/skel 目录浅谈：

skel 是 skeleton 的缩写，每当你新建一个用户的时候 (通过 useradd 命令)，/etc/skel 目录下的文件，都会原封不动的复制到新建用户的家目录下～

[root@node ~]# ll -a /etc/skel/  #让我们看看 /etc/skel 目录下都有啥吧

.bash_logout

.bash_profile

.bashrc

............

熟悉的一系列 .bash_xxx 配置文件，现在明白为什么新建一个用户，用户的目录下自动就会有了这些文件了吧～

如果你是一个多用户系统的管理员，你可以在 skel 目录下写个 ReadMe.txt 之类的文件，写一些使用说明，这样每个新建的用户都会在自己的目录下看到这个说明文件了。

如果你希望新建用户可以直接 startx 就启动到 gnome 桌面环境，你可以在 skel 目录下建立一个 .xinitrc 文件，内容如下：

export LC_ALL=&quot;zh_CN.UTF-8&quot;

export XMODIFIERS=@im=SCIM

export GTK_IM_MODULE=&quot;scim&quot;

eval `dbus-launch --exit-with-session --sh-syntax`

exec gnome-session

因为 .xinitrc 是 X 启动需要读取的用户配置文件，这样每个用户 startx 之后就直接装载 gnome 了。

你甚至可以在 sekl 目录下再建立目录，总之 /etc/skel 下的所有文件都会拷贝的用户的家目录去。

你也许会想到，在 skel 目录下的 .bashrc 文件中加入一些方便的环境变量或者命令别名，这样每个新建用户都可以使用这些功能。不过，更好的选择是把这些设置放到全局的 /etc/profile 中，因为 skel 目录下的文件是拷贝过去的，如果你修改或者增加了新的文件，只有新建的用户才能受益。