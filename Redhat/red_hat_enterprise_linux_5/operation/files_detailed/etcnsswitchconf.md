#### /etc/nsswitch.conf {#etc-nsswitch-conf}

NSS

[root@node6 ~]# cat /etc/nsswitch.conf

passwd:     files nis ldap   # 密码文件顺序

shadow:     files

group:      files

#hosts:     db files nisplus nis dns

hosts:      files dns  

bootparams: nisplus [NOTFOUND=return] files

ethers:     files

netmasks:   files

networks:   files

protocols:  files

rpc:        files

services:   files

netgroup:   files

publickey:  nisplus

automount:  files

aliases:    files nisplus