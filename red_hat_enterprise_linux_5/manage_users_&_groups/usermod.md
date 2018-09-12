### usermod {#usermod}

usermod

[root@localhost ~]#usermod -L                                               ## 用户加锁

[root@localhost ~]#usermod -U                                               ##用户解锁

[root@localhost ~]#usermod -e 时间 用户名                         ##设置用户在YYYY-MM-DD时登录失效

[root@localhost ~]#usermod  -a -G  groupname     username     ##把用户追加到指定组中

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

选项：

-c commen      #更改注释字段，这里通常是用户的全称

-d home dir    #更改主目录-e expire date #设置账户的过期及禁用日期

-g group       #更改初始登录组

-G group,[...] #用逗号隔开的一组该用户的补充组。如果使用-a -G选项会将用户附加到指定的组

-l login name  #更改登录名

-s shell       #更改登录shell

-u uid         #更改登录ID

-p password    #更改密码字段的字符串，但必须是加密的密码

-L             #锁住密码，这会使账户不可用

-U             #密码解锁

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

e.g.:

usermod -G sales joshua

usermod -G sales alex

usermod -G hr dax

usermod -G hr bryan

usermod -G web zak

usermod -G web ed

usermod -G sales,hr,web manager

usermod -a -G web dax

uid=504(joshua) gid=504(joshua) groups=504(joshua),200(sales)

uid=505(alex) gid=505(alex) groups=505(alex),200(sales)

uid=506(dax) gid=506(dax) groups=506(dax),201(hr),202(web)

uid=507(bryan) gid=507(bryan) groups=507(bryan),201(hr)

uid=508(zak) gid=508(zak) groups=508(zak),202(web)

uid=509(ed) gid=509(ed) groups=509(ed),202(web)

uid=510(manager) gid=510(manager) groups=510(manager),200(sales),201(hr),202(web)