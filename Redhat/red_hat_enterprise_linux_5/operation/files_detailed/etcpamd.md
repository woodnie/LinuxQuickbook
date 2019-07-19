#### /etc/pam.d/ {#etc-pam-d}

示例: /etc/pam.d/login 文件

auth       required     pam_securetty.so

auth       include      system_auth

account    required     pam_nologin.so

account    include      system_auth

password   include      system_auth

session    required     pam_selinux.so close

session    optional     pam_keyinit.so force revoke

session    include      system_auth

session    required     pam_loginuid.so

session    optional     pam_console.so

session    required     pam_selinux.so open

测试被分为四类:

auth 验证某用户的确是这个用户

account 批准某账户的使用

password 控制密码的修改

session 打开, 关闭, 并记录会话

每一类都会在需要时被调用，并为服务提供独立的结果

/etc/pam.d/ 文件：控制值

控制值决定每个测试将会如何影响组群的总体结果

required ：必须通过，若失败侧继续测试

requisite  和 required相似，不同之处是它在失败后停止测试

sufficient ：如果到此为止一直通过，现在就返回成功；如果失败，忽略测试，继续检查

optional ： 测试通过与否都无关紧要

include 返回在被调用的文件中配置的测试的总体控制值

system_auth 文件:

system-auth 被广泛使用

被include控制标记，而不是模块（如pam_stack.so) 调用

包含标准验证测试

被系统中许多程序共享

能够对标准系统验证进行简单、统一的管理

使用：/etc/pam.d/system-auth 进行验证

pam_unix.so模块：

用于基于传统的NSS验证的模块

auth 从NSS中获取散列密码，然后与输入的密码散列进行比较

account 检查密码是否过期

password 处理本地文件或NIS中的密码修改

session 将登录和注销事件记录到日志中

example:

跟踪失败登录：

#vi /etc/pam.d/system-auth

auth管理组添加一下内容：

auth        required      pam_tally.so no_magic_root

auth管理组添加一下内容：

account     required      pam_tally.so deny=2 no_magic_root

#faillog -u username     #查看用户失败的登录信息