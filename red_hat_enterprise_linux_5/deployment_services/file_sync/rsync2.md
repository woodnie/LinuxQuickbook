#### rsync2 {#rsync2}

配置rsync

NAME

      rsync - faster, flexible replacement for rcp

1.服务器安装rsync

2.服务器端配置

[root@node ~]# cat /etc/rsyncd.conf  

pid = nobody                                #uid和gid指明了运行身份

gid = nobody                                #uid和gid指明了运行身份

use chroot = no                            #chroot表示连接后自动切换目录

max connections = 4

pid file = /var/run/rsyncd.pid

lock file = /var/run/rsync.lock

log file = /var/log/rsyncd.log

[data]                      #模块名称

path = /data/          #模块的真实路径

ignore errors

read only = true     # read only = true 禁止客户机上传；read only=false 允许客户机上传

list = false

hosts allow = 192.168.1.0/24

hosts deny = 0.0.0.0/32

auth users = backup                  #访问时用于认证用户名，用户自定义；与useradd添加的用户不同

secrets file = /etc/rsyncd.pwd   #认证文件位置，用户自定义即可

[root@node ~]# cat /etc/rsyncd.pwd

backup:redhat       #明文保存密码，所以该文件只能设置root用户操作权限

3.服务器端启动rsync

[root@node ~]# rsync --daemon

如果要在启动时把服务起来，有几种不同的方法，比如：

a、加入inetd.conf

　　编辑/etc/services，加入rsync 873/tcp，指定rsync的服务端口是873

　　编加/etc/inetd.conf，加入rsync stream tcp nowait root /bin/rsync rsync --daemon

b、加入rc.local

　　在各种操作系统中，rc文件存放位置不尽相同，可以修改使系统启动时rsync --daemon加载进去。

c、如果采用的是rpm包安装的，可输入ntsysv然后把rsync服务选上，然后/etc/init.d/xinetd restart即可启动服务

4.客户端安装rsync

5.客户端启动rsync

6.客户端测试

[root@node2 ~]# rsync backup@192.168.1.201::data /var/ftp/

Password:

[root@node2 ~]# rsync --password-file=/etc/rsyncd.pwd backup@192.168.1.201::data /var/ftp/   #指定客户端密码保存位置，省去输入密码

rsync有六种不同的工作模式：

　　1.拷贝本地文档；当SRC和DES路径信息都不包含有单个冒号&quot;:&quot;分隔符时就启动这种工作模式。

　　2.使用一个远程shell程式（如rsh、ssh）来实现将本地机器的内容拷贝到远程机器。当DST路径地址包含单个冒号&quot;:&quot;分隔符时启动该模式。

　　3.使用一个远程shell程式（如rsh、ssh）来实现将远程机器的内容拷贝到本地机器。当SRC地址路径包含单个冒号&quot;:&quot;分隔符时启动该模式。

　　4.从远程rsync服务器中拷贝文档到本地机。当SRC路径信息包含&quot;::&quot;分隔符时启动该模式。

　　5.从本地机器拷贝文档到远程rsync服务器中。当DST路径信息包含&quot;::&quot;分隔符时启动该模式。

　　6.列远程机的文档列表。这类似于rsync传输，但是只要在命令中省略掉本地机信息即可。