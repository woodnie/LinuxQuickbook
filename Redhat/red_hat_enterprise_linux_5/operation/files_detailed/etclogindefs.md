#### /etc/login.defs {#etc-login-defs}

[root@node ~]# vi /etc/login.defs

PASS_MAX_DAYS   99999       # 密码更改最大间隔天数

PASS_MIN_DAYS   0               #密码更改最小间隔天数

PASS_MIN_LEN    5                 #密码最小长度

PASS_WARN_AGE   7             #密码到期前，提出警告的天数

#chage 命令会更改相应值

MD5_CRYPT_ENAB yes  #用户口令使用MD5加密

/etc/login.defs 文件是当创建用户时的一些规划，比如：

创建用户时，是否需要家目录；

UID和GID的范围；

用户的期限等等；

这个文件是可以通过root来定义的；

比如Fedora 的 /etc/logins.defs 文件内容；

# *REQUIRED*

#   Directory where mailboxes reside, _or_ name of file, relative to the

#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.

#   QMAIL_DIR is for Qmail

#

#QMAIL_DIR      Maildir

MAIL_DIR        /var/spool/mail  注：创建用户时，要在目录/var/spool/mail中创建一个用户mail文件；

#MAIL_FILE      .mail

# Password aging controls:

#

#       PASS_MAX_DAYS   Maximum number of days a password may be used.

#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.

#       PASS_MIN_LEN    Minimum acceptable password length.

#       PASS_WARN_AGE   Number of days warning given before a password expires.

#

PASS_MAX_DAYS   99999   注：用户的密码不过期最多的天数；

PASS_MIN_DAYS   0       注：密码修改之间最小的天数；

PASS_MIN_LEN    5       注：密码最小长度；

PASS_WARN_AGE   7       注：

#

# Min/max values for automatic uid selection in useradd

#

UID_MIN                   500   注：最小UID为500 ，也就是说添加用户时，UID 是从500开始的；

UID_MAX                 60000   注：最大UID为60000；

#

# Min/max values for automatic gid selection in groupadd

#

GID_MIN                   500   注：GID 是从500开始；

GID_MAX                 60000

#

# If defined, this command is run when removing a user.

# It should remove any at/cron/print jobs etc. owned by

# the user to be removed (passed as the first argument).

#

#USERDEL_CMD    /usr/sbin/userdel_local

#

# If useradd should create home directories for users by default

# On RH systems, we do. This option is ORed with the -m flag on

# useradd command line.

#

CREATE_HOME     yes   注：是否创用户家目录，要求创建；