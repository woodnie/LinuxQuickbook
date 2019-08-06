### build iso {#build-iso}

[root@localhost ~]# dd    if=/dev/cdrom   of=/root/myiso.iso

[root@localhost ~]# cp -r /dev/cdrom   /root/myiso.iso

[root@localhost ~]# mkisofs -r -o myiso.iso /dev/cdrom