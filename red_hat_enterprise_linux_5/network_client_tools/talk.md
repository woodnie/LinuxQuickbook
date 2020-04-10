### talk {#talk}

 write, mesg, wall

**Wite send message to single user, Example:**

root@www ~]# write 使用者账号 [用户所在终端接口]

[root@www ~]# who

root pts/1 2009-03-04 11:04 (192.168.1.100)

vbird1 pts/2 2009-03-04 13:15 (192.168.1.100) &lt;==有看到 vbird1 在在线

[root@www ~]# write vbird1 pts/2

Hello, there:

Please don&#039;t do anything wrong... &lt;==这两行是 root 写的信息！

# 结束时，请按下 [crtl]-d 来结束输入。此时在 vbird1 的画面中，会出现：

Message from root@www.vbird.tsai on pts/1 at 13:23 ...

Hello, there:

Please don&#039;t do anything wrong...

EOF

**MESG enable/disable message delivery.Example:**

[vbird1@www ~]$ mesg

[vbird1@www ~]$ mesg n

[vbird1@www ~]$ mesg y

**Wall send message to all users, Example:**

[root@www ~]# wall &quot;I will shutdown my linux server...&quot;