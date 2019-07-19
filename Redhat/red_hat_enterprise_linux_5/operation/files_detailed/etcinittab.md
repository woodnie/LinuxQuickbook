#### /etc/inittab {#etc-inittab}

/etc/inittab 文件中每个登记项的结构都是一样的，共分为以冒号&quot;：&quot;分隔的4个字段。具体如下：

     identifier :  run_level  :  action  :  process

其中，各字段以及与其相关的说明如下：

identifier　　登记项标识符，最多为4个字符。用于惟一地标识/etc/inittab文件中的每一个登记项

run_level

系统运行级，即执行登记项的init级别。用于指定相应的登记项适用于哪一个运行级，即在哪一个运行级中被处理。如果该字段为空，那么相应的登记项将适用于所有的运行级。在该字段中，可以同时指定一个或多个运行级，其中各运行级分别以数字0.1.2.3.4.5.6或字母a、b、c 表示，且无需对其进行分隔。

action

动作关键字。用于指定init(M)命令或进程对相应进程（在&quot;process&quot;字段定义）所实施的动作。具体动作包括：

1、boot：只有在引导过程中，才执行该进程，但不等待该进程的结束；当该进程死亡时，也不重新启动该进程。

2、bootwait：只有在引导过程中，才执行该进程，并等待进程的结束：当该进程死亡时，也不重新启动该进程。实际上，只有在系统被引导后，并从单用户方式进入多用户方式时，

                   这些登记项才被处理；如果系统的默认运行级设置为2（即多用户方式），那么这些登记项在系统引导后将马上被处理。

3、initdefault：指定系统的默认运行级。系统启动时，init将首先查找该登记项。如果存在init将据此决定系统最初要进入的运行级。

                     具体来说，init将指定登记项&quot;run_level&quot;字段中的最大数字（即最高运行级）为当前系统的默认运行级；

                     如果该字段为空，那么将其解释为 &quot;0123456&quot;，并以&quot;6&quot;作为默认运行级。如果不存在该登记项，那么init将要求用户在系统启动时指定一个最初的运行级。

4、off：如果相应的进程正在运行，那么就发出一个警告信号，等待20秒后，再通过杀死信号强行终止该进程。如果相应的进程并不存在那么就忽略该登记项。

5、once：启动相应的进程，但不等待该进程结束便继续处理/etc/inittab文件中的下一个登记项；当该进程死亡时，init也不重新启动该进程。

              注意：在从一个运行级进入另一个运行级时，如果相应的进程仍然在运行，那么init就不重新启动该进程。

6、ondemand：与&quot;respawn&quot;的功能完全相同，但只用于运行级为a、b或c的登记项。

7、powerfail：  只在init接收到电源失败信号时执行相应的进程，但不等待该进程结束。

8、powerwait：只在init接收到电源失败信号时执行相应的进程，并在继续对/etc/inittab文件进行任何处理前等待该进程结束。

9、respawn：如果相应的进程还不存在，那么init就启动该进程，同时不等待该进程的结束就继续扫描/etc/inittab文件；当该进程死亡时，init将重新启动该进程。

                   如果相应的进程已经存在，那么init将忽略该登记项并继续扫描/etc/inittab文件。

10、sysinit：只有在启动或重新启动系统并首先进入单用户时，init才执行这些登记项。而在系统从运行级1－6进入单用户方式时，init 并不执行这些登记项。

                  &quot;action&quot;字段为&quot;sysinit&quot;的登记项在&quot;run_level&quot;字段不指定任何运行级。

11、wait：启动进程并等待其结束，然后再处理/etc/inittab文件中的下一个登记项。

run_level

process

所要执行的shell命令。任何合法的shell语法均适用于该字段。

付：启动过程：

1.BIOS初始化

2.引导装载程序

3.内核初始化

4.启动init,并进入预期的运行级别：

  4.1 /etc/rc.d/rc.sysinit

  4.2 /etc/rc.d/rc and /etc/rc.d/rc[012345].d/

  4.3 /etc/rc.d/rc.local

  4.4 在适当的情况下使用X显示管理器

++++++++++++++++++++

今天在学习Linux启动流程的时候，发现了/etc/inittab这个文件里面的内容和以前的版本相差很多，在RHEL6中，这个文件中只能设置运行的等级了，其它的相关设置全部移动到/etc/init/里面了。经查询才知道，红帽使用新的Upstart启动服务来替换以前的init。

我们可能通过man inittab来查看

DESCRIPTION

The /etc/inittab file was the configuration file used by the original System V init(8) daemon.

The Upstart init(8) daemon  does  not  use  this  file,  and instead  reads  its  configuration  from files in /etc/init.

See init(5) for more details.

UpStart: 基于事件的启动系统

Sysvinit:基于结果的启动系统

SysVinit守护进程（sysvinit软件包）是一个基于运行级别的系统，它使用运行级别（单用户、多用户以及其他更多级别）和链接（位于/etc /rc?.d目录中，分别链接到/etc/init.d中的init脚本）来启动和关闭系统服务。Upstart init守护进程（upstart软件包）则是基于事件的系统，它使用事件来启动和关闭系统服务。

几个很重要的定义

event: 事件， 是指系统状态的一种改变，这种改变会被通知给init进程。举例来说，boot loader会触发startup事件，系统进入runlevel 2的时候会触发runlevel 2事件，某个文件系统被挂载时会触发path-mounted事件， USB设备的插拔也都会产生相应的事件。这些时间会被通知给init进程，然后init进程来决定系统如何处理这些事件。

job: 作业，一项作业是init进程读入的一系列指令。你可以使用initctl start或initctl stop命令来开始或停止某项作业，这是调用作业的一种方式。另一种方式是当系统被告知发生什么事件event后，运行该事件所对应的作业。可用sudo initctl list命令列出所有系统作业的运行状态。

task: 任务， a job that performs its work and returns to a waiting state when it is done. 中文是一种完成指定工作后进去等待状态的作业。

service: 服务，a job that does not normally terminate by itself. 举个例子来说， gettys 是以服务来实现的。 init进程监视所有服务，并在服务失败的时候重启服务。

原有的System V init启动过程的缺点是，它基于包含了大量启动脚本的runlevel目录。而Upstart则是事件驱动型的，因此，它只包含按需启动的脚本，这将使启动过程变得更加迅速。经过良好调优并使用Upstart启动方式的Linux服务器的启动速度要明显快于原有的使用System V init的系统。

为了使Upstart更容易理解，它仍然使用了一个init进程。所以，你仍然可以看到/sbin/init，它是所有服务的根进程。

有一个好消息，那就是RHEL 6对启动过程的改变很少。你还是可以处理那些在目录/etc/init.d中的包含服务脚本的服务，所以runlevel的概念一直存在。因此，在使用yum增加一个服务后，照样可以像以前那样使用chkconfig命令激活它。此外，仍然可以用service命令来启动它。

所有先前由/etc/ inittab中处理的条目，现在都在目录/etc/init中以单个文件的形式存在（不要与目录/etc/init.d混淆，/etc/init.d中包含的是服务脚本）。下面列举几个要使用的脚本：

/etc/init/rcS.conf

通过启动大部分的基本服务来对系统进行初始化的设定

/etc/init/rc.conf

对启动各自的运行级别（runlevel）的设定

/etc/init/control-alt-delete.conf

定义当用户按&quot;control-alt-delete&quot;三个键时的系统行为

/etc/init/tty.conf

and

/etc/init/serial.conf

定义了系统处理终端登录的方式

除了这些通用的文件，在文件/etc/sysconfig/init中还有一些额外的配置。在这里，定义了一些参数来决定启动信息的格式。除了那些不很重要的设置，有三行我们需要注意：

AUTOSWAP=no

ACTIVE_CONSOLES=/dev/tty[1-6]

SINGLE=/sbin/sushell

其中，第一行的值你可以设定为Yes，这样可以让你的系统能够自动检测交换分区。使用此选项意味着你再也不必在/etc/fstab中挂载交换分区了。在ACTIVE_CONSOLES这一行决定了虚拟控制台的创建。在大多数情况下，tty[1-6]工作得很好，同时这个选项也允许您分配更多或者更少的虚拟控制台。但是要注意，不要使用tty [1-8]，因为tty7是专门为图形界面预留的。

最后很重要的一行是single= /sbin/sushell。这一行可以有两个参数：/sbin/sushell（系统默认的参数），它会在启动单用户模式时将你带入一个root的 shell，参数/sbin/sulogin会在单用户模式启动之前弹出一个登录提示，你必须输入root账户的密码才能继续下去。

RHEL 6通过将System V替换为Upstart加快了其启动速度。采用了这项新服务，红帽仍然可以向下兼容地保持以前的管理方式，这就意味着，作为管理员，你仍可以使用原来的方式来管理服务。