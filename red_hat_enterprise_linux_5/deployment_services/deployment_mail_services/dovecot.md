#### dovecot {#dovecot}

类型: 系统 V 管理的服务

软件包: dovecot

守护进程: /usr/sbin/dovecot

脚本: /etc/init.d/dovecot

端口: 110 (pop), 995 (pop3s), 143 (imap), 993 (imaps)

相关软件包: procmail, fetchmail, openssl

搭建dovecot,开启imaps和pop3s,并生成自己的证书

1 安装dovecot

[root@node6 ~]# yum install dovecot

2 生成cert

[root@node6 ~]# make /etc/pki/tls/certs/server.pem   # .pem包含密钥和自签证书

[root@node6 ~]# vi /etc/dovecot.conf

3.编辑配置文件

#(编译此文件，指定public key和private key的路径)

ssl_cert_file = /etc/pki/tls/certs/dovecot.pem

ssl_key_file = /etc/pki/tls/certs/dovecot.pem

#指定邮局协议

protocols = imap imaps pop3 pop3s

4.

[root@node6 ~]# /etc/init.d/dovecot restart

5.测试（用户必须包含在/etc/aliases）

[root@node6 ~]# echo &quot;test&quot; |mail -s test wood

[root@node6 ~]# mutt -f imaps://wood@node6.domain.org  #默认root拒绝登录，所以不要使用root测试

到此，你的dovecot的证书就有效了！！！