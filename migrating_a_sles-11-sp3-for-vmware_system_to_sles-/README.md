# Migrating a SLES-11-SP3-for-VMware system to SLES-11-SP3 {#migrating-a-sles-11-sp3-for-vmware-system-to-sles-11-sp3}

This document                                    **(7015096)                                   ** is provided subject to the                                        [_disclaimer_](https://www.suse.com/support/kb/doc.php?id=7015096)                                        at the end of this document.                                   

### Environment {#environment}

SUSE Linux Enterprise Server 11 Service Pack 1 (SLES 11 SP1)SUSE Linux Enterprise Server 11 Service Pack 2 (SLES 11 SP2)SUSE Linux Enterprise Server 11 Service Pack 3 (SLES 11 SP3)SUSE Linux Enterprise Server 11 for VMware Service Pack 1SUSE Linux Enterprise Server 11 for VMware Service Pack 2SUSE Linux Enterprise Server 11 for VMware Service Pack 3SUSE Manager 2.1 

### Situation {#situation}

A SLES-11-SP3-for-VMware system should be migrated into to a plain SLES-11-SP3 without reinstalling it. This TID explains the process. 

### Resolution {#resolution}

There are two possibilities: either use an interactive script (section A) or perform the migration manually (section B).

#### A) Script based migration {#a-script-based-migration}

Download the tar archive from [TID-7015096-SLES-for-VMware-migration.tar.gz](http://support.novell.com/Platform/Publishing/images/TID-7015096-SLES-for-VMware-migration.tar.gz)Save the file in the /tmp/ directory and extract the files via the following command:

# tar xvzf TID-7007761-SLES-for-VMware-migration.tgz

Run the interactive script &quot;Migrate_SLES-for-VMware_to_SLES.sh&quot; as root:

# cd /tmp/SLES-for-VMware-migration# ./Migrate_SLES-for-VMware_to_SLES.sh

#### B) Manual migration {#b-manual-migration}

Perform the migration manually, all steps have to be performed as root.**1) Replace the product files**Download the tar archive from [TID-7015096-SLES-for-VMware-migration.tar.gz](http://support.novell.com/Platform/Publishing/images/TID-7015096-SLES-for-VMware-migration.tar.gz)Save the file in the /tmp/ directory and extract the files via the following command:

# tar xvzf TID-7007761-SLES-for-VMware-migration.tgz

Replace the SLES-11-SP3-for-VMware product file with the SLES-11-SP3  product file. Backup the SLES-11-SP3-for-VMware product file first:

# mv /etc/products.d/SLES-for-VMware.prod /etc/products.d/SLES-for-VMware.prod.SLES-for-VMware

Remove the symlink

# rm /etc/products.d/baseproduct

Copy the downloaded SLES-11-SP3 product file to &quot;/etc/products.d/&quot;

# cp /tmp/SLES-for-VMware-migration/SUSE_SLES.prod /etc/products.d/

Create the new baseproduct symlink

# ln -s /etc/products.d/SUSE_SLES.prod /etc/products.d/baseproduct

**2) Re-register your product**Important: In case this system is connected to a SMT or SUSE Manager, please skip this point and follow to point 3!To be able to re-register your product and get subscribed to the corresponding channels, a valid email address and a valid activation code for SLES-11-SP3 and all other installed Add-On products needs to be entered. Optionally, you can enter the hostname of your system. At first, you need to remove all current registrations and channels:

# /usr/bin/suse_register --erase-local-regdata

Start registration process:

# yast2 inst_suse_register

**3) Exchange the release packages**Exchange SLES-11-SP3-for-VMware release packages by SLES-11-SP3 release packages:

# zypper in -f sles-release sles-release-DVD# zypper rm SLES-for-VMware-release SLES-for-VMware-release-DVD

For SUSE Manager clients, youll need to either manually change the channels (from VMWare to the &quot;regular&quot; ones) or copy the RPMs to the system and install them manually. For either case, youre encourage to re-run the correct bootstrap registration script (SLES in this case, not SLES for VMWare).For SMT clients, youre recommended to fetch the RPMs from the SMT server and install them manually.**4) Remove obsolete SLES-11-SP3-for-VMware specific packages**The following SLES-11-SP3-for VMware specific packages need to be removed (if installed):

- bootsplash-branding-SLES-for-VMware - desktop-data-SLES-for-VMware - gconf2-branding-SLES-for-VMware - gfxboot-branding-SLES-for-VMware - gtk2-branding-SLES-for-VMware - gtk2-theme-SLES-for-VMware - kdebase4-SLES-for-VMware - kdebase4-SLES-for-VMware-lang - kdebase4-workspace-branding-SLES-for-VMware - kdm-branding-SLES-for-VMware - MozillaFirefox-branding-SLES-for-VMware - release-notes-SLES-for-VMware - sblim-gather-novirt - sblim-gather-novirt-provider - yast2-branding-SLES-for-VMware - yast2-theme-SLES-for-VMware - yast2-trans-vmware-install - yast2-vmware-warning

Remove those packages with:

# zypper rm [SLES-11-SP3-for-VMware specific packages]

**5) Install missing SLES-11-SP3 packages**The following SLES-11-SP3 packages need to be installed:

- bootsplash-branding-SLES - desktop-data-SLED - desktop-data-SLES-extra-gnome - gfxboot-branding-SLES - MozillaFirefox-branding-SLED - release-notes-sles - yast2-branding-SLES - yast2-theme-SLE

In case youre using the GNOME desktop, following additional packages need also to be installed:

- desktop-data-SLES-extra-gnome - gconf2-branding-SLES - gtk2-branding-SLED - gtk2-theme-SLED

In case youre using the KDE desktop, following additional packages need also to be installed:

- kdebase4-SLED - kdebase4-SLED-lang - kdebase4-workspace-branding-SLED - kdm-branding-SLED

If the packages sblim-gather-novirt and sblim-gather-novirt-provider were installed on SLES-11-SP3-for-VMware, one needs to install the following corresponding SLES-11-SP3 packages:

- sblim-gather - sblim-gather-provider

Please install the needed SLES-11-SP3 packages by executing:

# zypper in [SLES-11-SP3 packages]

**6) Apply the SLES-11-SP3 bootsplash branding**Call mkinitrd to apply the SLES-11-SP3 bootsplash branding:

# mkinitrd

**7) Adjust /boot/grub/menu.lst**Alter the titles of the standard and failsafe sections to display&quot;SUSE Linux Enterprise Server 11 SP3&quot;instead of&quot;SUSE Linux Enterprise Server 11 SP3 for VMware&quot;:

# sed &quot;s/SUSE Linux Enterprise Server 11 SP3 for VMware/SUSE Linux Enterprise Server 11 SP3/&quot; -i /boot/grub/menu.lst

**8) Reboot system**Reboot the system to apply all changes.**9) Additional step for systems registered in a SUSE Manager**Please delete /etc/sysconfig/rhn/osad-auth.conf, restart SUSE Manager daemons and run a rhn_check

# rm /etc/sysconfig/rhn/osad-auth.conf# rcosad restart# rcrhnsd restart# rhn_check

Otherwise your system might still not be recognized as a SLES System and you wont be able to (among others) perform SP Migrations (to SP4).

### Additional Information {#additional-information}

Note 1: As SLES-for-VMware 11 SP2 is out of general support, migrate those system first to SP3 using the known supported update procedures and then use the above procedure to make it a regular SLES.  

### Disclaimer {#disclaimer}

This Support Knowledgebase provides a valuable tool for                                    NetIQ/Novell/SUSE customers and parties interested in our products and solutions to                                    acquire information, ideas and learn from one another. Materials are provided for                                    informational, personal or non-commercial use within your organization and are                                    presented &quot;AS IS&quot; WITHOUT WARRANTY OF ANY KIND.                               

*   **Document ID:**7015096
*   **Creation Date:**22-MAY-14
*   **Modified Date:**28-OCT-15
    *   **SUSE**SUSE Linux Enterprise Server

Did this document solve your problem? Provide Feedback

### Rescue {#rescue}

输入root登陆

Rescue login#

查看硬盘分区

Rescue#

 fdisk -l 

   Device Boot      Start        End      Blocks  Id  System 

/dev/hde1              1        1024    1048560  82  Linux swap 

/dev/hde2            1025      11264    10485760  83  Linux 

一般地，hde2或者sda2是根分区

挂载分区

Rescue#

 mount /dev/hde2 /mnt 

Rescue# 

chroot /mnt 

### 3D {#3d}

　在SUSE Linux系统中，比较突出的特点就是具有3D效果的页面显示，而且这些3D效果展示完全可以通过快捷键来实现，这里我们只列举部分快捷键操作方式。如下：

　　**程序切换：**　　**Alt Tab：**在当前工作台中切换窗口　　**Ctrl Alt Tab：**在所有工作台中切换窗口

　　**立方体旋转：**　　**Ctrl Alt左/右方向键：**立体地切换桌面　　**Ctrl Shift Alt 左/右方向键：**把活动窗口移到左/右工作台　　**Ctrl Alt鼠标左键并拖曳：**手动旋转立方体

### passwd recover {#passwd-recover}

**suse恢复root密码:**1.重新启动机器，在出现grub引导界面后，在启动linux的选项里加上init=/bin/bash，通过给内核传递init=/bin/bash参数使得OS在运行login程序之前运行bash，出现命令行。2.稍等片刻出现(none)#:命令行。3.这时输入mount -n / -o remount，rw 表示将根文件系统重新mount为可读写，有了读写权限后就可以通过passwd命令修改密码了。4.这时输入passwd命令就可以重置密码了5.修改完成后记得用mount -n / -o remount，ro将根文件系统置为原来的状态。