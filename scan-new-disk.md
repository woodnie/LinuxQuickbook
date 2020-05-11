### **Scanning FC-LUN’s in Redhat Linux**

1.First, find out how many disks are visible in “fdisk -l” .

```
# fdisk -l 2
>
/dev
ull | egrep '^Disk' | egrep -v 'dm-' | wc -l
```

2.Find out how many host bus adapter configured in the Linux box.you can use “systool -fc\_host -v” to verify available FC in the system.

```
# ls /sys/class/fc_host
host0  host1
```

In this case,you need to scan host0 & host1 HBA.

3.If the system virtual memory is too low ,then do not proceed further.If you have enough free virtual memory,then you can proceed with below command to scan new LUNS.

```
# echo "1" > /sys/class/fc_host/host0/issue_lip
# echo "- - -" > /sys/class/scsi_host/host0/scan
# echo "1" > /sys/class/fc_host/host1/issue_lip
# echo "- - -" > /sys/class/scsi_host/host1/scan
```

**Note:**You need to monitor the “issue\_lip” in /var/log/messages to determine when the scan will complete.This operation is an asynchronous operation.

You can also use rescan-scsi-bus.sh script to detect new LUNS.

```
# yum install sg3_utils
# ./rescan-scsi-bus.sh
```

1. Verify if the new LUN is visible or not by counting the available disks.

```
# fdisk -l 2 >/dev/null | egrep '^Disk' | egrep -v 'dm-' | wc -l
```

If any new LUNS added , then you can see more count is more then before scanning the LUNS.

### **Scanning SCSI DISKS in Redhat Linux**

1. Finding the existing disk from fdisk.

```
[root@mylinz1 ~]# fdisk -l |egrep '^Disk' |egrep -v 'dm-'
Disk /dev/sda: 21.5 GB, 21474836480 bytes
```

2.Find out how many SCSI controller configured.

```
[root@mylinz1 ~]# ls /sys/class/scsi_host/host
host0 host1 host2
```

In this case, you need to scan host0,host1 & host2.

1. Scan the SCSI disks using below command.

```
[root@mylinz1 ~]# echo "- - -" > /sys/class/scsi_host/host0/scan
[root@mylinz1 ~]# echo "- - -" > /sys/class/scsi_host/host1/scan
[root@mylinz1 ~]# echo "- - -" > /sys/class/scsi_host/host2/scan
```

1. Verify if the new disks are visible or not.

```
[root@mylinz1 ~]# fdisk -l |egrep '^Disk' |egrep -v 'dm-'
Disk /dev/sda: 21.5 GB, 21474836480 bytes
Disk /dev/sdb: 1073 MB, 1073741824 bytes
Disk /dev/sdc: 1073 MB, 1073741824 bytes
```

From Redhat Linux 5.4 onwards, Red hat introduced “/usr/bin/rescan-scsi-bus.sh” script to scan all the SCSI bus and update the SCSI layer to reflect new devices.

But most of the time, the script will not be able to scan new disks and you need to go with echo command.

### 3\) If you’ve increased the disk size {#3-if-you-ve-increased-the-disk-size}

Once you’ve changed the disk’s size in VMware, boot up your VM again if you had to shut it down to increase the disk size in vSphere. If you’ve rebooted the server, you won’t have to rescan your SCSI devices as that happens on boot. If you did not reboot your server, rescan your SCSI devices as such.

First, check the name\(s\) of your scsi devices.

```
$ 
ls /sys/class/scsi_device/

0:0:0:0 1:0:0:0  2:0:0:0
```

Then rescan the scsi bus. Below you can replace the ‘0:0:0:0’ with the actual scsi bus name found with the previous command. Each colon is prefixed with a slash, which is what makes it look weird.

```
~$ 
echo 1 
>
 /sys/class/scsi_device/0\:0\:0\:0/device/rescan
```

That will rescan the current scsi bus and the disk size that has changed will show up.



[https://www.suse.com/support/kb/doc/?id=000018798](https://www.suse.com/support/kb/doc/?id=000018798)

btrfs:~ \# echo 1 &gt; /sys/block/sda/device/rescan

