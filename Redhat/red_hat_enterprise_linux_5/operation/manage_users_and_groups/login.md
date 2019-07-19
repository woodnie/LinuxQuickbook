#### login {#login}

1\. 控制台登陆

虚拟控制台之间的切换：

使用:Ctrl-Alt-F[1-6]

使用:Ctrl-Alt-F7访问图形化控制台

虚拟控制台的历史数据可以通过shift-PgUp shift-PgDn来滚动显示

登录到虚拟控制台使用startx 命令也可切换到图形环境

2.X登录

GNOME []: 默认的桌面环境     即GNU网络对象模型环境 (The GNU Network Object Model Environment)

KDE:                     备选的桌面环境     K桌面环境(Kool Desktop Environment)

Gnome图形化工具:

nautilus:Gnome图形化文件系统浏览器

3.非登录shell

1.[root@node ~]# su

2.图形化终端

3.执行的脚本

4.任何其他shell实例

4.登录shell

1.[root@node ~]# su -

2.任何登录时创建的shell(包括X登录)

登录shell 调用/etc/profile，然后调用/etc/profile.d，然后调用~/.bash_profile，然后调用~/.bashrc，然后调用/etc/bashrc

每个调用的脚本会依次撤销前一个调用脚本中的改变

[(/etc/profile：这是系统整体的配置，你最好不要修改这个文件；

~/.bash_profile 或 ~/.bash_login 或 ~/.profile：属于使用者个人配置，你要改自己的数据，就写入这里！)]

5.用户退出

调用~/.bash_logout

用于：

创建自动备份

清除临时文件

查看登录信息

查看当前登陆者的信息：

[root@server ~]# whoami

[root@server ~]# groups    ##groups - print the groups a user is in

[root@server ~]# id

查看当前所有登陆者的信息：

[root@server ~]# users       ##users - print the user names of users currently logged in to the current host

[root@server ~]#  who         ##who - show who is logged on

[root@server ~]#  w             ##w - Show who is logged on and what they are doing

[root@server ~]# finger       ##user information lookup program

查看历史登录信息：

[root@node ~]# last            #查看历史登录信息

[root@node ~]# last  user  #查看指定用户的历史登录信息

[root@node ~]# lastb          #查看历史失败的登录信息