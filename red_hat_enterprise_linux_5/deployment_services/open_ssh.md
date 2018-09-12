### open ssh {#open-ssh}

类型: 系统 V （System V）管理的服务

软件包: openssh, openssh-clients, openssh-server

守护进程: /usr/sbin/sshd

脚本: /etc/init.d/sshd

端口: 22

配置文件: /etc/ssh/*, $HOME/.ssh/

相关软件包: openssl, openssh-askpass, openssh-askpass-gnome, tcp_wrappers

OpenSSH 服务器配置

SSHD 配置文件

/etc/ssh/sshd_config

需考虑的选项

Protocol

ListenAddress

PermitRootLogin

Banner

OpenSSH 客户机

1.安全 shell 会话

ssh hostname

ssh user@hostname

ssh hostname remote-command

2.安全进行进程文件和目录复制

scp file user@host:remote-dir

scp -r user@host:remote-dir localdir

3.由sshd提供的安全FTP

sftp host

sftp -C user@host

ssh-add - 收集钥匙密码短语

ssh-agent - 管理钥匙密码短语

应用程序: RPM

文件完整性的两种实施方式

被安装的文件

MD5 单向散列

rpm --verify package_name (or -V)

发行的软件包文件

GPG 公钥签名

rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat*

rpm --checksig package_file_name (or -K)

EXAMPLE:

使用无密码短语的SHH钥匙

host:stationx.domain.org     user:userx       passwd:userx

host:stationy.domain.org     user:usery       passwd:usery

1.生成ssh密钥对：公钥和空的私钥

[stationx@userx ~]$ ssh-keygen -t dsa

Generating public/private dsa key pair.

Enter file in which to save the key (/home/userx/.ssh/id_dsa):    #Enter  使用默认私钥保存位置

Enter passphrase (empty for no passphrase):                           #Enter  使用空的私钥密码短语

Enter same passphrase again:

Your identification has been saved in /home/userx/.ssh/id_dsa.

Your public key has been saved in /home/userx/.ssh/id_dsa.pub.

The key fingerprint is:

0f:09:8e:d1:c0:17:d1:d1:9e:91:54:2e:a0:fe:6a:a0 userx@stationx.domain.org

2.公钥穿给usery@stationy

[stationx@userx ~]$ scp  ~/.ssh/id_dsa.pub usery@stationy.domain.org:/home/usery/.ssh/authorized_keys

3.userx@stationx测试

[stationx@userx ~]$ ssh usery@stationy.domain.org

Last login: Sun Apr 10 16:33:26 2011 from stationy.domain.org   # usery @ stationy 不用输入密码以userx登陆stationx

最好进行一下权限设置：

[stationy@usery ~]$ chmod  700  ~.ssh

[stationy@usery ~]$ chmod  600  ~.ssh/authorized_keys

使用无密码短语的SHH钥匙

host:stationx.domain.org     user:userx       passwd:userx

host:stationy.domain.org     user:usery       passwd:usery

1.生成ssh密钥对：公钥和空的私钥

[stationx@userx ~]$ ssh-keygen -t dsa

Generating public/private dsa key pair.

Enter file in which to save the key (/home/userx/.ssh/id_dsa):    #Enter  使用默认私钥保存位置

Enter passphrase (empty for no passphrase):                           #Enter  使用私钥密码短语

Enter same passphrase again:

Your identification has been saved in /home/userx/.ssh/id_dsa.

Your public key has been saved in /home/userx/.ssh/id_dsa.pub.

The key fingerprint is:

0f:09:8e:d1:c0:17:d1:d1:9e:91:54:2e:a0:fe:6a:a0 userx@stationx.domain.org

2.公钥穿给usery@stationy

[stationx@userx ~]$ scp  ~/.ssh/id_dsa.pub usery@stationy.domain.org:/home/usery/.ssh/authorized_keys

3.userx@stationx测试

[stationx@userx ~]$ ssh usery@stationy.domain.org

Enter passphrase for key /home/userx/.ssh/id_dsa:                   #要求注入私钥密码短语

Last login: Sun Apr 10 16:33:26 2011 from stationy.domain.org  

4.

[stationx@userx ~]$ ssh-agent           #图形化

[stationx@userx ~]$ ssh-agent bash   #文本

[stationx@userx ~]$ ssh usery@stationy.domain.org

使用SSH通道

alice  node5

bob   node6  

1.

[alice@node5 ~]$ ssh bob@192.168.1.60 -L 12345:192.168.1.60:80       #192.168.1.60:80 的数据重定向到本地 12345 端口

[alice@node5 ~]$ links [http://127.0.0.1:12345](http://127.0.0.1:12345)                                       #使用本地12345端口访问远程80端口数据

[alice@node5 ~]$ links [http://localhost:12345](http://localhost:12345)

2.

[alice@node5 ~]$ ssh bob@192.168.1.60 -L 192.168.1.50:12345:192.168.1.60:80  #192.168.1.60:80 的数据重定向到本地192.168.1.50:12345 端口

[alice@node5 ~]$ links [http://192.168.1.50:12345](http://192.168.1.50:12345)                                                #使用本地12345端口访问远程80端口数据

3.

[alice@node5 ~]$ ssh -Nf bob@192.168.1.60 -L 192.168.1.50:12345:192.168.1.60:80    #后台运行ssh

[alice@node5 ~]$ links [http://192.168.1.50:12345](http://192.168.1.50:12345)                                                       #使用本地12345端口访问远程80端口数据

生成证书文件

#openssl genrsa -out server.key 1024            //创建一个rsa私钥,文件名为server.key

#openssl rsa -in server.key -out server.key    //清楚密码

#openssl req -new -key server.key -out server.csr                                           //用 server.key 生成证书签署请求 CSR

#openssl x509 -days 365 -req -in server.csr -signkey server.key -out server.crt  //生成证书CRT文件server.crt