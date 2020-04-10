#### /boot/grub.conf {#boot-grub-conf}

grub

/boot/grub

/boot/grub.conf

[root@node ~]# grub-md5-crypy  # 创建grub密码

[root@node ~]# /sbin/grub-install boot-device   #重装grub

e.g.:

[root@node ~]# /sbin/grub-install  /dev/hda(主硬盘)

grub.conf

default=0 //默认从下面系统列表的第一个启动

timeout=5 //默认启动等待时间是5秒钟

splashimage=(hd0,0)/grub/splash.xpm.gz //系统选择菜单背景所在的位置

hiddenmenu //隐藏菜单

title Red Hat Enterprise Linux Server (2.6.18-194.el5) //在grub界面所显示的系统名称 （可以任意该动）

root (hd0,0) //启动时从IDE接口的第一块硬盘第一个分区启动

kernel /vmlinuz-2.6.11-1.1369_FC4 ro root=LABEL=/ rhgb quiet //启动的内核版本,可以在后面添加内核参数;ro ：vmlinuz只读

initrd /initrd-2.6.11-1.1369_FC4.img

++++++++++++++++++++++++++++++++++

菜单命令只能用于grub配置文件的全局配置部分，不能用在grub命令行交互界面，菜单命令在配置文件中应放在其它命令之前。常规命令可以应该于配置文件和grub命令行交互界面，命令行和菜单项命令可应该于GRUB配置文件的菜单项设置中，也可以用在GRUB命令交互界面。

一、GRUB启动命令菜单命令

菜单命令只能用于grub配置文件的全局配置部分，不能用在grub命令行交互界面，菜单命令在配置文件中应放在其它命令之前。

1、default//设置默认启动的菜单项

2、fallback//设置启动某菜单项失败后反回的菜单项

3、hiddenmenu//隐藏菜单界面

4、timeout//设置菜单自动启动的延时时间

5、title//开始一个菜单项

二、GRUB启动命令常规命令

常规命令可以应该于配置文件和grub命令行交互界面，可使用的常规命令有

1、bootp//通过bootp初始化网络设备

2、color//设置菜单界面的颜色

3、device//指定设备文件作为驱动器

4、dhcp//通过DHCP初始化网络设备

5、hide//隐藏某分区

6、ifconfig//手工配置网络设备

7、pager//改变内部页程序的状态

8、partnew//新建一个主分区

9、parttype//改变分区的类型

10、password为菜单界面设置口令

11、rarp//通过RARP初始化网络设置

12、serial//设置串口设备

13、setkey//设置键盘映射

14、splashimage//设置GRUB启动时的背景图片文件

15、termainal//选择终端类型

16、tftpserver//指定TFTP服务器

17、unhide//还原某隐藏分区

三、GRUB启动命令命令行和菜单项命令

命令行和菜单项命令可应该于GRUB配置文件的菜单项设置中，也可以用在GRUB命令交互界面。

1、bolcklist//显示某文件所在分区位置（blocklistnotation）

2、boot//启动操作系统

3、cat//显示文件内容

4、chainloader//把启动控制权软交给另外的启动引导器

5、cmp//比较两个文件

6、configfile//加载已存在的GRUB配置文件

7、debug//设置为debug模式

8、displayapm//显示APMBIOS信息

9、displaymem//显示内存配置

10、embed//嵌入Stage1.5文件

11、find//查找包括某文件的所有设备

12、fstest//测试文件系统

13、geometry//显示某驱动器的物理信息

14、halt//停止计算机运行（软件关机）

15、help//显示GRUB的命令帮助信息

16、impsprobe//查询对称多处理器（SMP）的信息

17、initrd//加载initrd文件

18、install//安装GRUB

19、ioprobe//查询某驱动器的输入输出（I/O）端口

20、kernel//引导操作系统内核

21、lock//锁定某GRUB导菜单项，使其输入密码后才可启动

22、makeactive//激活某主分区

23、map//虚拟映射某驱动器

24、md5crypt//使用MD5加密口令

25、module//加载模块

26、modulenounzip//加载模块不进行解压

27、pause//暂停并等待按键

28、quit//退出GRUB

29、reboot//重新启动计算机

30、read//读取内存中的内容

31、root//设置GRUB的root设备

32、rootnoverify//设备GRUB的root设备但不装载文件系统

33、savedefault//保存当前的启动菜单项为默认启动

34、setup//自动安装GRUB

35、testload//从文件系统中测试读取某文件

36、testvbe//测试VESABIOSEXTENSION

37、uppermem//强制设置主机上位内存的大小

38、vbeprobe//查询VESABIOSEXTENSION信息

++++++++++++++++++++++++++++++++++++

启动管理器GRUB参数配置方法大全

什么是启动管理器

启动管理器是存储在磁盘开始扇区中的一段程序，例如，硬盘的MBR(Master Boot Record)，在系统完成启动测试后，如果系统是从MBR启动，则BIOS(Basic Input/Output System)将控制传送给MBR。然后存储在MBR中的这段程序将运行。这段程序被称为启动管理器。它的任务就是将控制传送给操作系统，完成启动过程》有许多可用的启动管理器，包括GNU GRUB (Grand Unified Boot Loader),Bootmanager, LILO (LInux LOader), NTLDR (boot loader for Windows NT systems)，等等等.

什么是GRUB?

grub 是一个多重启动管理器。grub是GRand Unified Bootloader的缩写，它可以在多个操作系统共存时选择引导哪个系统。它可以引导的操作系统包括：

Linux,FreeBSD,Solaris,NetBSD,BeOSi,OS/2,Windows95/98,Windows NT,Windows2000，WinXP。它可以载入操作系统的内核和初始化操作系统（如Linux,FreeBSD），或者把引导权交给操作系统（如 Windows 98）来完成引导。

GRUB的特点

特别适用于linux与其它操作系统共存情况。

支持大硬盘现在大多数Linux发行版本的lilo都有同样的一个问题：根分区(/boot分区)不能分在超过1024柱面的地方，一般是在8.4G左右的地方，否则lilo不能安装，或者安装后不能正确引导系统。而grub就不会出现这种情况，只要安装时你的大硬盘是在LBA模式下，grub就可以引导根分区在 8G以外的操作系统。

支持开机画面 　 grub支持在引导开机的同时显示一个开机画面。对于玩家来说，这样可以制作自己的个性化开机画面；对于PC厂商，这样可以在开机时显示电脑的一些信息和厂商的标志等。grub支持640x480,800x600,1024x768各种模式的开机画面，而且可以自动侦测选择最佳模式，与Windows那 320x400的开机画面不可同日而语。

两种执行模式 grub不但可以通过配置文件进行例行的引导，还可以在选择引导前动态改变引导时的参数，还可以动态加载各种设备。例如你在Linux下编译了一个新的核心，但不能确定它能不能工作，你就可以在引导时动态改变grub的参数，尝试装载这个新的核心进行使用。Grub的命令行有非常强大的功能，而且支持如 bash或doskey一样的历史功能，你可以用上下键来寻找以前的命令。

菜单式选择 　 grub使用一个菜单来选择不同的系统进行引导。你还可以自己配置各种参数，如延迟时间，默认操作系统等。

分区大小改变后不必重新配置 　　grub是通过文件系统直接把核心读取到内存，因此只要操作系统核心的路径没有改变，grub就可以引导系统。

除此之外，Grub还有许多非常强大的功能。例如支持多种外部设备，动态装载操作系统内核，甚至可以通过网络装载操作系统核心。Grub支持多种文件系统，支持多种可执行文件格式，支持自动解压，可以引导不支持多重引导的操作系统,支持网络启动等。

什么是MBR和第一扇区

你可以简单的理解为MBR是整个硬盘的物理第一位置,而第一扇区是硬盘的物理第二位置.

1 一个GRUB配置文件

　　基于本例的分区如下：

　　hda 15G

　　hda1 8G / RED HAT LINUX8.0

　　hda5 7G /home

　　hdc 20G

　　hdc1 6.4G WinXP

　　hdc5 6.4G

　　hdc6 6.4G

　　hdc7 6.4G

　　#fdisk -l

　　# Disk /dev/hdc: 255 heads, 63 sectors, 2434 cylinders

　　Units = cylinders of 16065 * 512 bytes

　　Device Boot Start End Blocks Id System

　　/dev/hdc1 * 1 894 7181023+ b Win95 FAT32

　　/dev/hdc2 895 2434 12370050 f Win95 Extd (LBA)

　　/dev/hdc5 895 1787 7172991 b Win95 FAT32

　　/dev/hdc6 1788 2434 5196996 b Win95 FAT32

　　Disk /dev/hda: 255 heads, 63 sectors, 1867 cylinders

　　Units = cylinders of 16065 * 512 bytes

　　Device Boot Start End Blocks Id System

　　/dev/hda1 * 1 1020 8193118+ 83 Linux

　　/dev/hda2 1021 1802 6281415 83 Linux

　　/dev/hda3 1803 1867 522112+ 82 Linux swap

　　grub.conf,这个文件位于;/boot/grub/grub.conf

　　# grub.conf generated by anaconda

　　#

　　# Note that you do not have to rerun grub after making changes to this file

　　# NOTICE: You do not have a /boot partition. This means that

　　# all kernel and initrd paths are relative to /, eg.

　　# root (hd0,0)

　　# kernel /boot/vmlinuz-version ro root=/dev/hda1

　　# initrd /boot/initrd-version.img

　　#boot=/dev/hda

　　default=0

　　timeout=3

　　splashimage=(hd0,0)/boot/grub/splash.xpm.gz

　　title Red Hat Linux (2.4.18-14)

　　root (hd0,0)

　　kernel /boot/vmlinuz-2.4.18-14 ro root=LABEL=/

　　initrd /boot/initrd-2.4.18-14.img

　　title Microsoft Windows XP

　　map (hd0) (hd1)

　　map (hd1) (hd0)

　　root (hd1,0)

　　chainloader (hd1,0)+1

　　makeactive

　　boot

　　2 解读grub.conf文件

　　我们将来看看grub.conf文件内语句,(注:...)内的东西是我们的解读内容.

　　# grub.conf generated by anaconda

　　#

　　# Note that you do not have to rerun grub after making changes to this file

　　# NOTICE: You do not have a /boot partition. This means that

　　# all kernel and initrd paths are relative to /, eg.

　　# root (hd0,0)

　　# kernel /boot/vmlinuz-version ro root=/dev/hda1

　　# initrd /boot/initrd-version.img

　　#boot=/dev/hda (注:以上以符号井＂＃＂开头的行表示被注释掉，没有任何意义)

　　default=0 (注:默认的操作系统就是由default控制的。default后加一个数字n，表明是第 n＋1个。需要注意的是，GRUB中，计数是从0开始的，第一个硬盘是hd0，第一 个软驱是fd0，等等。所以，default 0 表示默认的操作系统在这儿是 Red Hat Linux (2.4.18-14)如果你修改成1就是WinXP了)

　　timeout=3 (注:timeout表示默认等待的时间，这儿是3秒钟。超过3秒，用户还没有作出选 择的话，系统将自动选择默认的操作系统;当然你可以改成任何你乐意的时间)

　　splashimage=(hd0,0)/boot/grub/splash.xpm.gz (注:指定开机画面文件splash.xpm.gz的位置，也可以splash /boot/logo/800x600x8.img)

　　title Red Hat Linux (2.4.18-14) (注:表示Red Hat Linux的菜单项)

　　root (hd0,0) (注:表示第一个硬盘第一个分区,这里的root和系 统内的root不是一码事!详细如下说明)

　　kernel /boot/vmlinuz-2.4.18-14 ro root=LABEL=/ (注:指定内核的位置,详细说明如下 文)

　　initrd /boot/initrd-2.4.18-14.img (注:初始化)

　　title Microsoft Windows XP (注:表示Microsoft Windows XP的菜单项)

　　map (hd0) (hd1) (注:map是命令,详细如下)

　　map (hd1) (hd0)

　　root (hd1,0) (注:这是指第二个硬盘(从硬盘)上第一个分区)

　　chainloader (hd1,0)+1 (注:链式装入器,装入一个扇区的数据然后把引导 权交给它。详细说明如下)

　　makeactive

　　boot

　　(注:在 Linux 中，当谈到 &quot;root&quot; 文件系统时，通常是指主 Linux 分区。但是，GRUB 有它自己的 root 分区定义。GRUB 的 root 分区是保存 Linux 内核的分区。这可能是您的正式 root 文件系统，也可能不是。我们讨论的是 GRUB，需要指定 GRUB 的 root 分区。进入 root 分区时，GRUB 将把这个分区安装成只读型，这样就可以从该分区中装入 Linux 内核。GRUB 的一个很&quot;酷&quot;的功能是它可以读取本机的 FAT、FFS、minix、ext2 和 ReiserFS 分区.到目前为止，您可能会感到一点疑惑，因为 GRUB 所使用的硬盘／分区命名约定与 Linux 使用的命名约定不同。在Linux 中，第一个硬盘的第五个分区称作 &quot;hda5&quot;。而 GRUB 把这个分区称作 &quot;(hd0,4)&quot;。GRUB 对硬盘和分区的编号都是从 0 开始计算。另外，硬盘和分区都用逗号分隔，整个表达式用括号括起。现在，可以发现如果要引导 Linux 硬盘 hda5，应输入 &quot;root (hd0,4)&quot;。

　　知道了内核在哪儿，还要具体指出哪个文件是内核文件，这就是kernel的工作。

　　kernel /boot/vmlinuz-2.4.18-14 ro root=LABEL=/说明/boot/vmlinuz-2.4.18-14就是要载入的内核。后面的都是传递给内核的参数。root=LABEL= /就是linux的硬盘分区表示法，ro是readonly的意思。initrd用来初始的linux image，并设置相应的参数。

　　命令map:当你有两块硬盘，一个无法从第二块硬盘启动的操作系统，例如Windowsxp，就可以使用map命令.你能够将hd0映射为hd1，将hd1映射为hd0。换句话说，你可以虚拟的交换两个硬盘而启动所需要的操作系统 。命令形式如下：

　　grub&gt; map (hd0) (hd1)

　　grub&gt; map (hd1) (hd0)

　　GRUB 使用了&quot;链式装入器&quot;(chainloader)。链式装入器从分区 (hd1,0) 的引导记录中装入winxp自己的引导装入器，然后引导它。这就是这种技术叫做链式装入的原因 -- 它创建了一个从引导装入器到另一个的链。这种链式装入技术可以用于引导任何版本的 DOS 或 Windows。

　　GRUB的配置文件要简单就这么简单，如果你要更个性化一点，试一试把&quot;color light-gray/blue &quot;加在default语句的下面，下一次启动GRUB时，看看有什么变化，再试一试&quot;color light-blue/red&quot;,惊喜吗？ 有趣吧! )

　　3 配置grub

　　grub启动时会在/boot/grub/中寻找一个名字为menu.lst的配置文件，如果找不到此文件则不进入菜单模式而直接进入命令行模式。

　　现在，我们来看一下如何在启动后进入各种操作系统，如何建立menu.conf文件。我们就从GRUB支持的启动过程开始。可以有两种方法来完成启动过程：

　　·A.通过调用内核本地启动

　　·B.连续启动或者将控制转给另一个引导器

　　A模式启动过程

　　1．配置跟设备或者告诉GRUB你的根文件系统。

　　2．告诉GRUB你的内核影像的位置，然后将参数传送给内核。

　　3．重新启动，试一下。

　　为了启动Linux，将内核以bzImage的文件名放在/boot/目录中，跟文件系统是

　　/dev/hda1，或者GRUB中的(hd0,0)。启动过程如下：

　　1.root (hd0,0) [This sets the root partition]

　　2.kernel /boot/bzImage root=/dev/hda1 [This sets the kernel]

　　B模式启动过程（这种模式假设当前的分区中安装了另一个启动管理器，例如LILO

　　或者NTLDR）：

　　1．设置根分区但不要安装它

　　2．激活这个分区

　　3．配置需要启动的分区的第一个扇区

　　4．重新启动，看一下效果。

　　我们在试试启动安装在/dev/hdc1或者(hd1，0)的windows。启动windows的过程如下:

　　1.rootnoverify (hd1,0)

　　2.makeactive

　　3.chainloader +1 [+1 sets the first sector of the current root

　　partition]

　　4.boot [transfers the control and quits GRUB]

　　menu.conf文件：它用于建立启动多操作系统时的菜单。建立menu.conf并不难。它使用简单的英语，就象你在这一节看到的那样。

　　所有的菜单项目都以没有逗号分隔的&quot;title TITLENAME&quot;开头。你可以随意设置

　　TITLENAME。

　　设置Linux启动菜单步骤如下：

　　1．设置标题

　　2．设置根分区

　　3．设置内核的相应参数

　　4．启动

　　一个菜单例子：

　　title Red Hat Linux (2.4.18-14)

　　root (hd0,0)

　　kernel /boot/vmlinuz-2.4.18-14 ro root=LABEL=/

　　initrd /boot/initrd-2.4.18-14.img

　　前面有#的行是一个注释。

　　建立启动Windows 或者 DOS的菜单：

　　title Windoze

　　rootnoverify (hd0,0)

　　makeactive

　　chainloader +1

　　boot

　　#----

　　又或者:

　　title Microsoft Windows XP

　　map (hd0) (hd1)

　　map (hd1) (hd0)

　　root (hd1,0)

　　chainloader (hd1,0)+1

　　makeactive

　　boot

　　----

　　注意:root和rootnoverify都是一样的，把rootnoverify改成root也行。不过经过实践来看。有时引导win时，系统安装好后，是rootnoverify (hdX.Y)这样形式的，这样会出现windows起不来，出现什么windows什么文件损坏的情况。这时，我们就要把在grub中，引导 windows的那段中的rootnoverify改为root

　　root英文的意思就是根的意思，在这里是让linux知道自己所处的位置，也就是我们所安装linux的／根分区所在的位置 。

　　4 GRUB的交互性

　　GRUB 最好的优点之一就是其强健的设计 -- 在不断使用它时请别忘了这点。如果更新内核或更改它在磁盘上的位置，不必重新安装 GRUB。事实上，如有必要，只要更新 menu.lst 文件即可，一切将保持正常。

　　只有少数情况下，才需要将 GRUB 引导装入器重新安装到引导记录。首先，如果更改 GRUB root 分区的分区类型（例如，从 ext2 改成 ReiserFS），则需要重新安装。或者，如果更新 /boot/grub 中的 stage1 和 stage2 文件，由于它们来自更新版本的 GRUB，很有可能要重新安装引导装入器。其它情况下，可以不必理睬！

　　GRUB的最大的特点就是交互性特别强。在开机时，按一下&quot;c&quot;，将进入GRUB 控制台。显示如下：

　　GRUB version 0.5.96.1 (640K lower / 3072K upper memory)

　　[ Minimal BASH-like line editing is supported. For the first word, TAB

　　lists possible command completions. Anywhere else TAB lists the possible

　　completions of a device/filename. ]

　　grub&gt;

　　欢迎使用 GRUB 控制台。现在，再研究命令：

　　将通过GRUB 控制台绕过lilo来启动RedHat linux，

　　grub&gt; root (h

　　现在，按一次 Tab 键。如果系统中有多个硬盘，GRUB 将显示可能完成的列表，从 &quot;hd0&quot; 开始。如果只有一个硬盘，GRUB 将插入 &quot;hd0,&quot;。如果有多个硬盘，继续进行，在 (&quot;hd2&quot;) 中输入名称并在名称后紧跟着输入逗号，但不要按 Enter 键。部分完成的 root 命令看起来如下：

　　grub&gt; root (hd0,

　　现在，继续操作，再按一次 Tab 键。GRUB 将显示特定硬盘上所有分区的列表，以及它们的文件系统类型。在我的系统中，按 Tab 键时得到以下列表：

　　grub&gt; root (hd0, (tab，按tab一下键)

　　Possible partitions are:

　　Partition num: 0, Filesystem type is fat, partition type 0x6

　　Partition num: 2, Filesystem type is ext2fs, partition type 0x83

　　Partition num: 4, Filesystem type unknown, partition type 0x7

　　Partition num: 5, Filesystem type is ext2fs, partition type 0x83

　　Partition num: 6, Filesystem type is fat, partition type 0xb

　　Partition num: 7, Filesystem type is fat, partition type 0xb

　　Partition num: 8, Filesystem type is ext2fs, partition type 0x83

　　Partition num: 9, Filesystem type unknown, partition type 0x82

　　如您所见，GRUB 的交互式硬盘和分区名称实现功能非常有条理。这些，只需要好好理解 GRUB 新奇的硬盘和分区命名语法，然后就可以继续操作了

　　grub&gt; root (hd0,

　　现在已安装了 root 文件系统，到装入内核的时候了

　　grub&gt; kernel /boot/vmlinuz-2.4.2 root=/dev/hda5 ro

　　[Linux-bzImage, setup=0x1200, size=0xe1a30]

　　您已经安装了 root 文件系统并装入了内核。现在，可以引导了。只要输入 &quot;boot&quot;，Linux 引导过程就将开始。是不是很cool啊，GRUB的menu.lst更像一个linux下的脚本程序。

5 常见grub除错方法的思路

　　首先进去Linux的rescue模式！

　　用软盘或光盘启动，然后在启动的提示符输入:linux rescue

　　按照提示进入一个Shell状态，你可以到/mnt/下面看到一个sysimage这么目录，进去以后，就是你安装linux的/分区.

　　使用命令将根分区变为当前目录的根分区:chroot /mnt/sysimage

　　然后转到/sbin/这个目录中.

　　使用fdisk -l 显示当前分区情况，然后使用#grub-install /dev/hdx(x为你使用的是那块硬盘安装的，一般情况下是hda)

　　使用exit推出chroot，再使用exit退出linux rescue模式，系统将重新启动！取出光盘，应该可以看到grub安装好了.

　　在具体的环境中，编辑/boot/grub/grub.conf文件和menu.lst文件

　　简化：

　　1.安装盘启动

　　2.进入linux rescue模式

　　3.一系列键盘以及几项简单的配制，过后就［继续］了。。。这个过程，我不说了，比较简单。

　　4.然后会出现这样的字符

　　sh#

　　5\. sh#grub

　　会出现这样的字符：grub&gt;我们就可以在这样的字符后面，输入：grub&gt;root (hdX,Y)

　　grub&gt;setup (hd0)

　　如果成功会有一个successful......

　　这里的X，如果是一个盘，就是0，如果你所安装的linux的根分区在第二个硬盘上，那X就是1了；Y，就是装有linux系统所在的根分区。 setup (hd0)就是把GRUB写到硬盘的MBR上。

　　其他：

　　grub菜单项丢失，只有字符grub&gt;时的处理方法：

　　grub&gt;cat (hd0,0) /root/grub/grub.conf(为了看参数。)

　　grub&gt;root (hd0,1)

　　grub&gt;kernel (hd0,0) /boot/vmlinuz-2.4.18-11 ro root=/dev/hda2

　　grub&gt;initrd (hd0,0) /boot/initrd-2.4.18-11.img

　　grub&gt;boot

　　如果看不明白，可以参考后面的命令慢慢看，这里不做注释，促使大家学习，哈哈

　　98先装,用的是单独的硬盘,4.3G,那时候,LINUX8还没有到我手中

　　后来到了,在家中安装好了,选择GRUB,就会有DOS的一个菜单,我的是在主分区

　　到了公司,把LINUX挂在第一个盘的位置,那个盘挂在第四个盘的位置(这个无所谓)

　　然后GRUB配置如下

　　default=0

　　timeout=10

　　splashimage=(hd0,0)/grub/splash.xpm.gz

　　title Red Hat Linux (2.4.18-14)

　　root (hd0,0)

　　kernel /vmlinuz-2.4.18-14 ro root=LABEL=/

　　initrd /initrd-2.4.18-14.img

　　title DOS

　　rootnoverify (hd1,0)

　　makeactive

　　chainloader (hd1,0)+1

　　map (hd0) (hd1)

　　map (hd1) (hd0)

　　boot

　　下面是GRUB的可用命令列表:

　　#大部分命令我们不常用，而且我也没有每个都试验！

　　关于下面将要用到的三种模式的解释：

　　GRUB的用户界面有三种：命令行模式，菜单模式和菜单编辑模式

　　（a） 命令行模式：

　　进入命令行模式后GRUB会给出一个命令提示符`grub&gt;`，此时就可以键入命令，按回车执行。此模式下可执行的命令是在menu.lst中可执行的命令的一个子集。此模式下允许类似于Bash shell的命令行编辑功能：

　　或 光标右移一个字符

　　或 光标左移一个字符

　　到这一行的开头

　　或 到行尾

　　或 删除光标处的字符

　　或 删除光标左边的字符

　　删除光标右边的所有字符（包括光标处的字符）

　　删除光标左边的所有字符（包括光标处的字符）

　　恢复上次删除的字符串到光标位置

　　或 历史记录中的上一条命令

　　或 历史记录中的下一条命令

　　在命令行模式下键有补全命令的功能，如果你敲入了命令的前一部分，键入系统将列出所有可能以你给出的字符串开头的命令。如果你给出了命令，在命令参数的位置按下键，系统将给出这条命令的可能的参数列表，具体的可用命令集将在后面给出。

　　（b） 菜单模式

　　当存在文件/boot/grub/menu.lst文件时系统启动自动进入此模式。菜单模式下用户只需要用上下箭头来选择他所想启动的系统或者执行某个命令块，菜单的定义在menu.lst文件中，你也可以从菜单模式按键进入命令行模式，并且可以按键从命令行模式返回菜单模式。菜单模式下按键将进入菜单编辑模式。

　　（c） 菜单编辑模式

　　菜单编辑模式用来对菜单项进行编辑改变，其界面和菜单模式的界面十分类似，不同的是菜单中显示的是对应某个菜单项的命令列表。如果在编辑模式下按下,则取消所有当前对菜单的编辑并回到菜单模式下。在编辑模式下选中一个命令行，就可以对这条指令进行修改，修改完毕后按下，GRUB将提示你确认并完成修改。如果你想在当前命令列表中增加一条命令，按在当前命令的下面增加一条指令，按在当前命令前处增加一条指令。按删除一条指令。

　　仅用于菜单的命令（不包括菜单项内部的启动命令）

　　default num

　　设置菜单中的默认选项为num（默认为0,即第一个选项），超时将启动这个选项

　　fallback num

　　如果默认菜单项启动失败，将启动这个num后援选项。

　　password passwd new-config-file

　　关闭命令行模式和菜单编辑模式，要求输入口令，如果口令输入正确，将使用new-config－file

　　作为新的配置文件代替menu.lst，并继续引导。

　　timeout sec

　　设置超时，将在sec秒后自动启动默认选项。

　　title name ...

　　开始一个新的菜单项，并以title后的字串作为显示的菜单名。

　　在菜单（不包括菜单项内部的命令）和命令行方式下都可用的命令

　　bootp

　　以BOOTP协议初始化网络设备

　　color normal [highlight]

　　　　改变菜单的颜色，normal是用于指定菜单中非当前选项的行的颜色，highlight是用于指定当前菜单选项的颜色。如果不指定 highlight，GRUB将使用normal的反色来作为highlight颜色。指定颜色的格式是&quot;前景色/背景色&quot;，前景色和背景色的可选列表如下：

　　* black

　　* blue

　　* green

　　* cyan

　　* red

　　* magenta

　　* brown

　　* light-gray

　　下面的颜色只能用于背景色

　　* dark-gray

　　* light-blue

　　* light-green

　　* light-cyan

　　* light-red

　　* light-magenta

　　* yellow

　　* white

　　你可以在前景色前加上前缀&quot;blink-&quot;,产生闪烁效果，你可以在menu.lst中加上下面这个选项来改变颜色效果：

　　title OS-BS like

　　color magenta/blue black/magenta

　　device drive file

　　在GRUB命令行中，把BIOS中的一个驱动器drive映射到一个文件file。你可以用这条命令创建一个磁盘映象或者当GRUB不能真确地判断驱动器时进行纠正。如下

　　grub&gt; device (fd0) /floppy-image

　　grub&gt; device (hd0) /dev/sd0

　　这条命令只能在命令行方式下使用， 是个例外。

　　dhcp

　　用DHCP协议初始化网络设备。目前而言，这条指令其实就是bootp的别名，效果和bootp一样。

　　hide partition

　　这条指令仅仅对DOS和WINDOWS有用，当在一个硬盘上存在多个DOS/WIN的主分区时，有时需要这条指令隐藏其中的一个或几个分区，即在分区表中设置&quot;隐藏&quot;位。

　　rarp

　　用RARP协议初始化网络设备。

　　setkey to_key from_key

　　改变键盘的映射表，将from_key映射到to_key,注意这条指令并不是交换键映射，如果你要交换两个键的映射，需要用两次setkey指令,如下：

　　grub&gt; setkey capslock control

　　grub&gt; setkey control capslock

　　其中的键必须是字母，数字或者下面的一些代表某一键的字符串：

　　`escape, `exclam, `at, `numbersign, `dollar, `percent,

　　`caret, `ampersand, `asterisk, `parenleft, `parenright,

　　`minus, `underscore, `equal, `plus, `backspace, `tab,

　　`bracketleft, `braceleft, `bracketright, `braceright, `enter,

　　`control, `semicolon, `colon, `quote, `doublequote,

　　`backquote, `tilde, `shift, `backslash, `bar, `comma,

　　`less, `period, `greater, `slash, `question, `alt, `space,

　　`capslock, `FX (`X is a digit), and `delete.

　　下面给出了它们和键盘上的键的对应关系：

　　`exclam＝`!

　　`at＝`@

　　`numbersign＝`#

　　`dollar＝`$

　　`percent＝`%

　　`caret＝`^

　　`ampersand＝`&amp;

　　`asterisk＝`*

　　`parenleft＝`(

　　`parenright＝`)

　　`minus＝`-

　　`underscore＝`_

　　`equal＝`=

　　`plus＝`+

　　`bracketleft＝`[

　　`braceleft＝`{

　　`bracketright＝`]

　　`braceright＝`}

　　`semicolon＝`;

　　`colon＝`:

　　`quote＝`

　　`doublequote＝`&quot;

　　`backquote＝``

　　`tilde＝`~

　　`backslash＝`

　　`bar＝`|

　　`comma＝`,

　　`less＝`

　　`slash＝`/

　　`question＝`?

　　`space＝`

　　unhide partition

　　仅仅对DOS/WIN分区有效，清除分区表中的&quot;隐藏&quot;位。

　　仅用于命令行方式或者菜单项内部的命令

　　blocklist file

　　显示文件file在所占磁盘块的列表。

　　boot

　　仅在命令行模式下需要，当参数都设定完成后，用这条指令启动操作系统

　　cat file

　　显示文件file的内容，可以用来得到某个操作系统的根文件系统所在的分区，如下：

　　grub&gt; cat /etc/fstab

　　chainloader [`--force] file

　　把file装入内存进行chainload,除了能够通过文件系统得到文件外，这条指令也可以用磁盘块列表的方式读入磁盘中的数据块，如+1`指定从当前分区读出第一个扇区进行引导。如果指定了`--force`参数，则无论文件是否有合法的签名都强迫读入，当你在引导SCO UnixWare时需要用这个参数。

　　cmp file1 file2

　　比较文件的内容，如果文件大小不一致，则输出两个文件的大小，如下：

　　Differ in size: 0x1234 [foo], 0x4321 [bar]

　　如果两个文件的大小一致但是在某个位置上的字节不同，则打印出不同的字节和他们的位移：

　　Differ at the offset 777: 0xbe [foo], 0xef [bar]

　　如果两个文件完全一致，则什么都不输出。

　　configfile FILE

　　将FILE作为配置文件替代menu.lst。

　　embed stage1_5 device

　　如果device是一个磁盘设备的话，将Stage1_5装入紧靠MBR的扇区内。如果device是一个FFS文件系统分区的话，则将Stage1_5装入此分区的第一扇区。如果装入成功的话，输出写入的扇区数。

　　displaymem

　　显示出系统所有内存的地址空间分布图。

　　find filename

　　在所有的分区中寻找指定的文件filename，输出所有包含这个文件的分区名。参数filename应该给出绝对路径。

　　fstest

　　启动文件系统测试模式。打开这个模式后，每当有读设备请求时，输出向底层例程读请求的参数和所有读出的数据。输出格式如下：

　　先是由高层程序发出的分区内的读请求，输出：之后由底层程序发出的扇区读请求，输出：[磁盘绝对扇区偏移] 可以用install或者testload命令关闭文件系统测试模式。

　　geometry drive [cylinder head sector [total_sector]]

　　输出驱动器drive的信息。

　　help [pattern ...]

　　在线命令帮助，列出符合pattern的命令列表，如果不给出参数，则将显示所有的命令列表。

　　impsprobe

　　检测Intel多处理器，启动并配置找到的所有CPU。

　　initrd file ...

　　为Linux格式的启动映象装载初始化的ramdisk，并且在内存中的Linux setup area中设置适当的参数。

　　install stage1_file [`d] dest_dev stage2_file [addr] [`p] [config_file] [real_config_file]

　　这是用来完全安装GRUB启动块的命令，一般很少用到。

　　ioprobe drive

　　探测驱动器drive所使用的I/O口，这条命令将会列出所有dirve使用的I/O口。

　　kernel file ...

　　装载内核映象文件(如符合Multiboot的a.out,ELF,Linux zImage或bzImage,FreeBSD a.out，NetBSD

　　a.out等等）。文件名file后可跟内核启动时所需要的参数。如果使用了这条指令所有以前装载的模块都要重新装载。

　　makeactive

　　使当前的分区成为活跃分区，这条指令的对象只能是PC上的主分区，不能是扩展分区。

　　map to_drive from_drive

　　映射驱动器from_drive到to_drive。这条指令当你在chainload一些操作系统的时候可能是必须的，这些操作系统如果不是在第一个硬盘上可能不能正常启动，所以需要进行映射。如下：

　　grub&gt; map (hd0) (hd1)

　　grub&gt; map (hd1) (hd0)

　　这个就用来对付双硬盘最过瘾！！！哈哈

　　module file ...

　　对于符合Multiboot规范的操作系统可以用这条指令来装载模块文件file，file后可以跟这个module所需要的参数。注意，必须先装载内核，再装载模块，否则装载的模块无效。

　　modulenounzip file ...

　　同module命令几乎一样，唯一的区别是不对module文件进行自动解压。

　　pause message ...

　　输出字符串message，等待用户按任意键继续。你可以用(ASCII码007)使PC喇叭发声提醒用户注意。

　　quit

　　退出GRUB shell，GRUB shell类似于启动时的命令行模式，只是它是在用户启动系统后执行/sbin/grub才

　　进入，两者差别不大。

　　read addr

　　从内存的地址addr处读出32位的值并以十六进制显示出来。

　　root device [hdbias]

　　将当前根设备设为device，并且试图mount这个根设备得到分区大小。hdbias参数是用来告诉BSD内核在当前分区所在磁盘的前面还有多少个 BIOS磁盘编号。例如，系统有一个IDE硬盘和一个SCSI硬盘，而你的BSD安装在IDE硬盘上，此时，你就需要指定hdbias参数为1。

　　rootnoverify device [hdbias]

　　和root类似，但是不mount该设备。这个命令用在当GRUB不能识别某个硬盘文件系统，但是仍然必须指定根设备。

　　setup install_device [image_device]

　　安装GRUB引导在install_device上。这条指令实际上调用的是更加灵活但是复杂的install指令。如果

　　image_device也指定了的话，则将在image_device中寻找GRUB的文件映象，否则在当前根设备中查找。

　　testload file

　　这条指令是用来测试文件系统代码的，它以不同的方式读取文件file的内容，并将得到的结果进行比较，如果正确的话，输出的`i=X,filepos=Y`中的X,Y的值应该相等，否则就说明有错误。通常这条指令正确执行的话，之后我们就可以正确无误地装载内核。

　　uppermem kbytes

　　强迫GRBU认为高端内存只有kbytes千字节的内存，GRUB自动探测到的结果将变得无效。这条指令很少使用，可能只在一些古老的机器上才有必要。通常GRUB都能够正确地得到系统的内存数量。

GRUB加密

一、GRUB 明口令加密；

比如我没有设置密码之前/etc/grub是如下的样子：

default=1

timeout=10

splashimage=(hd0,7)/boot/grub/splash.xpm.gz

title Fedora Core (2.4.22-1.2061.nptl)

root (hd0,7)

kernel /boot/vmlinuz-2.4.22-1.2061.nptl ro root=LABEL=/

initrd /boot/initrd-2.4.22-1.2061.nptl.img

title WindowsXP

rootnoverify (hd0,0)

chainloader +1

加入以后就是下面这样的：

default=1

timeout=10

splashimage=(hd0,7)/boot/grub/splash.xpm.gz

password=123456

title Fedora Core (2.4.22-1.2061.nptl)

lock

root (hd0,7)

kernel /boot/vmlinuz-2.4.22-1.2061.nptl ro root=LABEL=/

initrd /boot/initrd-2.4.22-1.2061.nptl.img

title WindowsXP

rootnoverify (hd0,0)

chainloader +1

从上面的可以看出，GRUB的密码是123456，lock的意思就是把Redhat Fedora锁住了。如果启动时会提示错误。这时就应该按P键，然后输入密码就行了。我设置的是123456，当然应该输入123456了，输入别的密码肯定不能通过，这样是不是做到保密了呢？？

二、GRUB 的md5加密方法；

经jerboa兄指教，我又读了一下GRUB文档，的确感觉到用md5加密校验GRUB密码比较安全。为了也能让和我一样菜的弟兄，也能知道如何通过md5进行GRUB密码加密，我不得不把这个教程写出来。哈哈，高手就是免读了，此文为菜鸟弟兄所准备。

用md5加密校码GRUB密码，这样会更安全。

1、用grub-md5-crypt成生GRUB的md5密码；

通过grub-md5-crypt对GRUB的密码进行加密码运算，比如我们想设置grub的密码是123456，所以我们先要用md5进行对123456这个密码进行加密

[root@linux01 beinan]# /sbin/grub-md5-crypt

Password: 在这里输入123456

Retype password: 再输入一次123456

$1$7uDL20$eSB.XRPG2A2Fv8AeH34nZ0

$1$7uDL20$eSB.XRPG2A2Fv8AeH34nZ0 就是通过grub-md5-crypt进行加密码后产生的值。这个值我们要记下来，还是有点用。

2、更改 /etc/grub.conf

比如我原来的/etc/grub.conf文件的内容是下面的。

default=1

timeout=10

splashimage=(hd0,7)/boot/grub/splash.xpm.gz

title Fedora Core (2.4.22-1.2061.nptl)

root (hd0,7)

kernel /boot/vmlinuz-2.4.22-1.2061.nptl ro root=LABEL=/

initrd /boot/initrd-2.4.22-1.2061.nptl.img

title WindowsXP

rootnoverify (hd0,0)

chainloader +1

所以我要在/etc/grub.conf中加入 password --md5 $1$7uDL20$eSB.XRPG2A2Fv8AeH34nZ0 这行，以及lock，应该加到哪呢，请看下面的更改实例；

EXAMPLE1：

timeout=10

splashimage=(hd0,7)/boot/grub/splash.xpm.gz

password --md5 $1$7uDL20$eSB.XRPG2A2Fv8AeH34nZ0

title Fedora Core (2.4.22-1.2061.nptl)

lock

root (hd0,7)

kernel /boot/vmlinuz-2.4.22-1.2061.nptl ro root=LABEL=/

initrd /boot/initrd-2.4.22-1.2061.nptl.img

title WindowsXP

rootnoverify (hd0,0)

chainloader +1

EXAMPLE2：

timeout=10

splashimage=(hd0,7)/boot/grub/splash.xpm.gz

password --md5 $1$7uDL20$eSB.XRPG2A2Fv8AeH34nZ0    #全局密码，输入密码才能编辑启动菜单

title Fedora Core (2.4.22-1.2061.nptl)

password --md5 $1$7uDL20$eSB.XRPG2A2Fv8AeH34nZ0    #菜单密码，输入密码才能引导系统

root (hd0,7)

kernel /boot/vmlinuz-2.4.22-1.2061.nptl ro root=LABEL=/

initrd /boot/initrd-2.4.22-1.2061.nptl.img

title WindowsXP

rootnoverify (hd0,0)

chainloader +1