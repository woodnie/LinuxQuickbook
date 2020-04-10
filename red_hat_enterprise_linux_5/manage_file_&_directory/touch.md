### touch {#touch}

touch

touch - change file timestamps

创建空文件或更新文件时间戳

選項與參數：

-a  ：僅修訂 access time；

-c  ：僅修改檔案的時間，若該檔案不存在則不建立新檔案；

-d  ：後面可以接欲修訂的日期而不用目前的日期，也可以使用 --date=

  &quot;日期或時間&quot;-m

：僅修改  mtime ； -t

：後面可以接欲修訂的時間而不用目前的時間，格式為  [[CC]YY]MMDDhhmm[.ss]

touch  -r

例：

[root@localhost temp]# touch {sep,oct}_{report,log}{1,2,3}

[root@localhost temp]# ll

total 48

-rw-r--r-- 1 root root 0 Oct  7 00:16 oct_log1

-rw-r--r-- 1 root root 0 Oct  7 00:16 oct_log2

-rw-r--r-- 1 root root 0 Oct  7 00:16 oct_log3

-rw-r--r-- 1 root root 0 Oct  7 00:16 oct_report1

-rw-r--r-- 1 root root 0 Oct  7 00:16 oct_report2

-rw-r--r-- 1 root root 0 Oct  7 00:16 oct_report3

-rw-r--r-- 1 root root 0 Oct  7 00:16 sep_log1

-rw-r--r-- 1 root root 0 Oct  7 00:16 sep_log2

-rw-r--r-- 1 root root 0 Oct  7 00:16 sep_log3

-rw-r--r-- 1 root root 0 Oct  7 00:16 sep_report1

-rw-r--r-- 1 root root 0 Oct  7 00:16 sep_report2

-rw-r--r-- 1 root root 0 Oct  7 00:16 sep_report3