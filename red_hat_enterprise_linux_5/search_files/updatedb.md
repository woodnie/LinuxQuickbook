### updatedb {#updatedb}

updatedb

NAME

      udpatedb - update a database for mlocate #

DESCRIPTION

      updatedb  creates  or updates a database used by locate(1).  If the database already exists, its data is reused

      to avoid rereading directories that have not changed.

[root@node ~]# updatedb    # 更新locate数据库

[root@node ~]# cat /etc/updatedb.conf  #配置文件