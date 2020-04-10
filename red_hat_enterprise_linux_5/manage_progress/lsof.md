### lsof {#lsof}

lsof

NAME

      lsof - list open files   # 列出被进程所打开的文件的信息

[root@node ~]# lsof  -i:端口号   #列出谁在使用某个端口

[root@node ~]# lsof  -i:22

COMMAND  PID USER   FD   TYPE DEVICE SIZE NODE NAME

sshd    2752 root    3u  IPv6   9852       TCP *:ssh (LISTEN)

sshd    2954 root    3u  IPv6  10587      TCP node1:ssh-&gt;192.168.0.100:50157 (ESTABLISHED)

[root@node ~]# lsof -p pid #列出该进程打开的文件

被打开的文件可以是

1.普通的文件

2.目录  

3.网络文件系统的文件

4.字符设备文件  

5.(函数)共享库  

6.管道，命名管道

7.符号链接

8.底层的socket字流。网络socket，unix域名socket