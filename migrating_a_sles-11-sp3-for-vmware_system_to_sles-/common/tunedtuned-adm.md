### tuned/tuned-adm {#tuned-tuned-adm}

使用 tuned/tuned-adm 工具动态调优系统

2014年08月7日 | 标签: performance

RHEL/CentOS 在 6.3 版本以后引入了一套新的系统调优工具 tuned/tuned-adm，其中 tuned 是服务端程序，用来监控和收集系统各个组件的数据，并依据数据提供的信息动态调整系统设置，达到动态优化系统的目的；tuned-adm 是客户端程序，用来和 tuned 打交道，用命令行的方式管理和配置 tuned，tuned-adm 提供了一些预先配置的优化方案可供直接使用，比如：笔记本、虚拟机、存储服务器等。

如果你正在使用笔记本（电池电源），想优化系统、节约电源又不想知道太多这方面的细节，就可以用 tuned/tuned-adm 这套工具并应用 laptop-battery-powersave 方案来调整和优化系统。当然不同的系统和应用场景有不同的优化方案，tuned-adm 预先配置的优化策略不是总能满足要求，这时候就需要定制，tuned-adm 允许用户自己创建和定制新的调优方案。

系统的性能优化是个很大的话题，如果对这方面感兴趣可以参考 Linux 性能监测系列文章：

介绍，CPU，Memory, IO, Network, Tools.

安装和启动 tuned:

# yum update

# yum install tuned

# service tuned start

# chkconfig tuned on

# service ktune start

# chkconfig ktune on

查看当前优化方案：

# tuned-adm active

Current active profile: default

Service tuned: enabled, running

Service ktune: enabled, running

查看预先配置好的优化方案：

# tuned-adm list

Available profiles:

- laptop-battery-powersave

- virtual-guest

- desktop-powersave

- sap

- server-powersave

- virtual-host

- throughput-performance

- enterprise-storage

- laptop-ac-powersave

- latency-performance

- spindown-disk

- default

Current active profile: default

如果服务器是虚拟机母机的话，可以选用 virtual-host 方案优化。如果报错 &quot;kernel.sched_migration_cost&quot; is an unknown key 可以通过编辑 sysctl.ktune 这个文件解决。

# tuned-adm profile virtual-host

Reverting to saved sysctl settings:                        [  OK  ]

Calling /etc/ktune.d/tunedadm.sh stop:                   [  OK  ]

Reverting to cfq elevator: sda sdb sdc sdd sde sdf sdg     [  OK  ]

Stopping tuned:                                            [  OK  ]

Switching to profile virtual-host

Applying deadline elevator: sda sdb sdc sdd sde sdf sdg    [  OK  ]

Applying ktune sysctl settings:

/etc/ktune.d/tunedadm.conf:                                [FAILED]

 error: &quot;kernel.sched_migration_cost&quot; is an unknown key

Calling /etc/ktune.d/tunedadm.sh start:                  [  OK  ]

Applying sysctl settings from /etc/sysctl.conf

Starting tuned:                                            [  OK  ]

# vi /etc/tune-profiles/virtual-host/sysctl.ktune

...

#kernel.sched_migration_cost = 5000000

...

# tuned-adm profile virtual-host

如果是企业存储服务器的话，可以用 enterprise-storage 方案：

# tuned-adm profile enterprise-storage

Stopping tuned:                                            [  OK  ]

Switching to profile enterprise-storage

Applying deadline elevator: dm-0 sda sdb sdc sdd           [  OK  ]

Applying ktune sysctl settings:

/etc/ktune.d/tunedadm.conf:                                [  OK  ]

Calling /etc/ktune.d/tunedadm.sh start:                  [  OK  ]

Applying sysctl settings from /etc/sysctl.conf

Starting tuned:                                            [  OK  ]

上面预定的方案不是总能满足要求，如果有自己的需求可以定制自己的方案。自己定制很容易，切换到优化方案的配置目录，拷贝一个例子，然后编辑里面的相关参数就可以了，使用 tuned-adm list 命令会看到刚创建的新方案 my-virtual-host：

# cd /etc/tune-profiles/

# cp -r virtual-host my-virtual-host

# vi my-virtual-host/*

# tuned-adm list

Available profiles:

- laptop-battery-powersave

- virtual-guest

- desktop-powersave

- sap

- server-powersave

- virtual-host

- throughput-performance

- enterprise-storage

- laptop-ac-powersave

- latency-performance

- spindown-disk

- default

- my-virtual-host

Current active profile: virtual-host