### ssh {#ssh}

OpenSSh

1.

[root@node ~]# ssh user@host

[root@node ~]# ssh user@host  command

e.g.:

[root@node ~]# ssh wood@node1

[root@node ~]# ssh wood@node1  df -h   # 通过 ssh 在远程主机上执行命令： df -h

[root@node ~]# ssh wood@node1  df -h   # 通过 ssh 在远程主机上执行命令： df -h

[root@node ~]# ssh wood@node1  df -h   # 通过 ssh 在远程主机上执行命令： df -h

[root@node ~]# echo &quot;hello world&quot; | ssh root@node1 cat &gt; /tmp/file  

[root@node ~]# ssh -L 8080:remote-server:80 user@ssh-server # 使用 [http://localhost:8080](http://localhost:8080) 来访问 [http://remote-server:80](http://remote-server:80)

[root@node ~]# ssh -fNgR 10022:172.16.13.115:22 ctbri@124.127.117.189

2.ssh 密钥管理

[root@node ~]# ssh-keygen   # 生成密钥（ private key,public key ）

[root@node ~]# ssh-keyscan

[root@node ~]# ssh-copy-id        user@host   # 把公钥复制到目标系统

[root@node ~]# ssh-copy-id   -i   user@host   # -i 指定公钥文件

或者手动复制到 ~/.ssh/authorized_keys 文件中，此文件只能设置属主读写权限

e.g.:

[root@host1 ~]# useradd wood

[root@host1 ~]# passwd wood

[root@node ~]# ssh wood@host1

[wood@node ~]$ ssh-keygen -t dsa   # 设置密码为空

[wood@node ~]$ ssh-copy-id -i .ssh/id_dsa.pub wood@host1

[wood@node ~]$ ssh 192.168.1.202  # 空密码即可直接登录

[wood@node ~]$ ssh-keygen -f .ssh/id_dsa -p  # 设置密码

[wood@node ~]$ ssh 192.168.1.202  # 需密码登录

3.ssh 代理：

GNOME 中代理自动提供

命令行中需运行：

[root@node ~]# ssh-agent bash  # 使用验证代理（ authentica agent ）；这样的话口令就只需输入一次

[root@node ~]# ssh-add               # 把密钥添加给代理

[root@node ~]# ssh-add -l            # 查看别保存的密钥列表