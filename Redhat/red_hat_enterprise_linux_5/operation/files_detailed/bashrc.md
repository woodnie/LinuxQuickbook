#### bashrc {#bashrc}

bashrc 文件

非登录生效

相关文件：

/etc/bashrc

~/.bashrc

用于：

设置本地变量

定义别名

登录shell 调用/etc/profile，然后调用/etc/profile.d，然后调用~/.bash_profile，然后调用~/.bashrc，然后调用/etc/bashrc

每个调用的脚本会依次撤销前一个调用脚本中的改变

[root@node ~]# cat .bashrc