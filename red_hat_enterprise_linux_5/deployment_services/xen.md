### xen {#xen}

配置xen

虚拟化：

全虚拟化

半虚拟化

1.install

install xen

install kernal-xen

install virt-manager

2.安装xen包后，需重启系统（注意更改grub.conf中为支持xen的系统），然后查看内核是否支持xen

[root@localhost ~]#uname -r   ##查看内核是否支持xen

2.6.18-53.el5xen                          ##有xen后缀即为支持

3.chkconfig xend on

4.创建虚拟机

[root@localhost ~]#virt-install            #tui创建虚拟机

[root@localhost ~]#virt-manage        #gui创建虚拟机