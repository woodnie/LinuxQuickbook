#### mount/tun2fs {#mount-tun2fs}

1、关闭日志mount -o remount -o ro tune2fs -O ^has_journal mount -o remount -o rw 2、查询日志功能是否开启[root@mail ~]# tune2fs -l /dev/hdb2 |grep has_journalFile ... 

        要指定日志方式，可以使用如下方式：

        1 向/etc/fstab的选项字段添加适当的字符串例如 data=journal 

|                                                         |                                         |
| --- | --- |

        2 在调用 mount 时直接指定 -o data=journal 命令行选项。

|                                                         |                                         |
| --- | --- |

**data=journal日志模式**

**data=ordered日志模式 （默认）**

**data=writeback日志模式 **