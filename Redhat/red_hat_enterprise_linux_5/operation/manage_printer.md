### manage printer {#manage-printer}

管理打印机

CUPS(ommon Unix Printing System,UNIX通用打印系统)

使用互联网打印协议 (IPP)

允许远程浏览打印队列

配置文件:

etc/cups/cupsd.conf      #cupsd.conf - server configuration file for cups   (cups守护进程cupsd的配置文件)

/etc/cups/printers.conf   #printers.conf - printer configuration file for cups (cups的打印机配置文件,可有配置工具自动生成)

配置工具：

system-config-printer #图形化管理工具管理本地和远程打印机

lpadmin命令行工具

Web 工具 [http://localhost:631](http://localhost:631)

1.打印

[root@node ~]# lpr                    #份数     file       #使用    默认打印机，并指定打印份数

[root@node ~]# lpr   -P printer    #份数     file       #使用非默认打印机，并指定打印份数

2.查看

[root@node ~]# lpq              #查看    默认的打印机打印队列

[root@node ~]# lpq  -P        #查看非默认的打印机打印队列

3.删除

[root@node ~]# lprm                         作业号码         #删除   默认的打印机打印队列

[root@node ~]# lprm   -P  printer     作业号码         #删除非默认的打印机打印队列

4.管理

[root@node ~]# lpadmin  #管理打印机队列

[root@node ~]# lpstat      #显示本地队列信息

其他：

lpstat -a 列举配置了的打印机

evincec查看PDF文档

enscrip和a2ps把文本转换成PostScript

ps2pdf 把PostScript转换成PDF

mpage在一张纸上打印多页