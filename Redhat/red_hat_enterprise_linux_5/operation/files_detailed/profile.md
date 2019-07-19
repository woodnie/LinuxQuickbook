#### profile {#profile}

profile 文件

仅登录生效

相关文件：

/etc/profile

/root/.profile

/home/.../.profile

用于：

设置环境变量

运行命令

登录shell 调用/etc/profile，然后调用/etc/profile.d，然后调用~/.bash_profile，然后调用~/.bashrc，然后调用/etc/bashrc

每个调用的脚本会依次撤销前一个调用脚本中的改变

shell启动文件

    登录系统时，自动执行的命令。

1.系统启动文件

       /etc/profile

       系统启动文件对所有用户有效，一般用于用户的全局配置

2.用户启动文件

        /root/.profile

        /home/.../.profile

        用户启动文件针对每个用户定制启动文件，一般用于用户特定的配置