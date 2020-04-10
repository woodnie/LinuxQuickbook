### fg {#fg}

fg

[root@localhost ~]# kill   -18    % 后台进程ID      ##把进程调入后台，继续运行

[root@localhost ~]# kill   -17    %后台进程ID      ##把进程调入后台，停止运行（即 ctrl+z）

[root@localhost ~]# kill            %后台进程ID      ##结束后台进程

[root@localhost ~]# fg            %后台进程ID        ##把进程调到前台foreground  ，恢复运行

[root@localhost ~]# bg           %后台进程ID        ##把进程调到后台background，恢复运行

[root@localhost ~]# command   &amp;        ##把进程调到后台background运行

[root@node ~]# tail -f /var/log/messages &amp;

[root@node ~]# tail -n0 -f /var/log/messages &amp;  # -n0选项可以使tail不会产生屏幕输出