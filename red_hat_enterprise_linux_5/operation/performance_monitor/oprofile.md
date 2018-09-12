#### oprofile {#oprofile}

yum install oprofile

在运行 oprofile 之前，必须用opcontrol配置监视环境。在 opcontrol 命令被执行时，设置选项就会被保存到 /root/.oprofile/daemonrc 文件中。

命令格式

opcontrol [--vmlinux] [--start] [--stop] [--dump] [--shutdown] [--save=filename]

参数解释

--vmlinux 用来配置是否监视内核。要监视内核，以根用户身份执行以下命令

[root@node ~]# opcontrol --vmlinux=/usr/src/linux-2.6.13/vmlinux

要配置oprofile 不监视内核，以根用户身份执行以下命令

[root@node ~]# opcontrol --no-vmlinux

这个命令还会载入 oprofile 内核模块（如果还没有被载入），并创建 /dev/oprofile/ 目录（如果不存在）。

[root@node ~]# opcontrol --start 开始监视

[root@node ~]# opcontrol --stop 停止监视

[root@node ~]# opcontrol --shutdown 停止建档器

[root@node ~]# opcontrol --save 保存数据

[root@node ~]# opcontrol --定期收集样品