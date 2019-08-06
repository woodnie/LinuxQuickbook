### passwd {#passwd}

passwd

-d 删除密码

-f 强制执行

-k 更新只能发送在过期之后

-l 停止账号使用

-S 显示密码信息

-u 启用已被停止的账户

-x 设置密码的有效期

-g 修改群组密码

-i 过期后停止用户账号

[root@node ~]# echo PASSWORD |passwd --stdin USERNAME

[root@node1 ~]# passwd -S root

root PS 2010-12-16 0 99999 7 -1 (Password set, MD5 crypt.)

[root@node1 ~]# passwd -S nobody

nobody LK 2010-12-16 0 99999 7 -1 (Alternate authentication scheme in use.)

TEMP:

SH2CN-SUSE11SP2TST:~ # id test1

uid=1002(test1) gid=100(users) groups=16(dialout),33(video),100(users)

SH2CN-SUSE11SP2TST:~ # id test2

uid=1003(test2) gid=100(users) groups=16(dialout),33(video),100(users)

SH2CN-SUSE11SP2TST:~ # cat /etc/shadow|grep test

test1:$2y$10$J1vauJonwde6io4KK1mUneJm.AKGCjAFmGR.Wh9h5ad6j8JlM/Cwi:16602:0:99999:7:::

test2:$2y$10$OmDV3fIYl0z8GCmEnUwtTOXcnl2aHLeR4HKAn1kF80p0tEuZVZPCy:16602:0:99999:7:::

SH2CN-SUSE11SP2TST:~ # 

SH2CN-SUSE11SP2TST:~ # cat /etc/shadow|grep test

test1:$2y$10$9b.W5F7Zai3O3qWN5UtFPuGlBENZoM7FDkDM.YJVKfJ492wrNofQ2:16602:0:99999:7:::

test2:$2y$10$SQKJzTJbcL4UhvIlFhGJf.0xkh8UkrjoNnJjWhlZomBH0iLCwZHXa:16602:0:99999:7:::

SH2CN-SUSE11SP2TST:~ # id test1

uid=1002(test1) gid=100(users) groups=16(dialout),33(video),100(users)

SH2CN-SUSE11SP2TST:~ # id test2

uid=1003(test2) gid=100(users) groups=16(dialout),33(video),100(users)

SH2CN-SUSE11SP2TST:~ #