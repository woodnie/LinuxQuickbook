### watch {#watch}

watch

NAME

      watch - execute a program periodically, showing output fullscreen

-d 显示差异

-n&lt;时间&gt; 周期性执行命令的间隔

[root@node ~]# watch -n1-d ls -l #每隔1秒执行1次ls -l命令，并高亮显示差异；所要执行的命令最好用 引起来