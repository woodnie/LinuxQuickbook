### locate {#locate}

locate

locate - find files by name

-i       # 区分大小写

-n X  #只列举前X个匹配项目

[root@node ~]# locate foo             #搜索名称或路径中带有foo的文件

[root@node ~]# locate -r \.foo$    #搜索以foo结尾的文件

updatedb：根據 /etc/updatedb.conf 的設定去搜尋系統硬碟內的檔名，並更新 /var/lib/mlocate 內的資料庫檔案；

locate：依據 /var/lib/mlocate 內的資料庫記載，找出使用者輸入的關鍵字檔名