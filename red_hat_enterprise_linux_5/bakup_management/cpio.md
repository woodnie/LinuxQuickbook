### cpio {#cpio}

cpio

這個指令挺有趣的，因為 cpio 可以備份任何東西，包括裝置設備檔案。不過 cpio 有個大問題， 那就是 cpio 不會主動的去找檔案來備份！啊！那怎辦？所以囉，一般來說， cpio 得要配合類似 find 等可以找到檔名的指令來告知 cpio 該被備份的資料在哪裡啊！ 有點小麻煩啦～因為牽涉到我們在第三篇才會談到的資料流重導向說～ 所以這裡你就先背一下語法，等到第三篇講完你就知道如何使用 cpio 囉！

[root@www ~]# cpio -ovcB  &gt; [file|device] &lt;==備份

[root@www ~]# cpio -ivcdu &lt; [file|device] &lt;==還原

[root@www ~]# cpio -ivct  &lt; [file|device] &lt;==察看

備份會使用到的選項與參數：

 -o ：將資料 copy 輸出到檔案或裝置上

 -B ：讓預設的 Blocks 可以增加至 5120 bytes ，預設是 512 bytes ！

　  　 這樣的好處是可以讓大檔案的儲存速度加快(請參考 i-nodes 的觀念)

還原會使用到的選項與參數：

 -i ：將資料自檔案或裝置 copy 出來系統當中

 -d ：自動建立目錄！使用 cpio 所備份的資料內容不見得會在同一層目錄中，因此我們

      必須要讓 cpio 在還原時可以建立新目錄，此時就得要 -d 選項的幫助！

 -u ：自動的將較新的檔案覆蓋較舊的檔案！

 -t ：需配合 -i 選項，可用在&quot;察看&quot;以 cpio 建立的檔案或裝置的內容

一些可共用的選項與參數：

 -v ：讓儲存的過程中檔案名稱可以在螢幕上顯示

 -c ：一種較新的 portable format 方式儲存

你應該會發現一件事情，就是上述的選項與指令中怎麼會沒有指定需要備份的資料呢？還有那個大於 (&gt;) 與小於 (&lt;) 符號是怎麼回事啊？因為 cpio 會將資料整個顯示到螢幕上，因此我們可以透過將這些螢幕的資料重新導向 (&gt;) 一個新的檔案！ 至於還原呢？就是將備份檔案讀進來 cpio (&lt;) 進行處理之意！我們來進行幾個案例你就知道啥是啥了！

範例：找出 /boot 底下的所有檔案，然後將他備份到 /tmp/boot.cpio 去！

[root@www ~]# find /boot -print

/boot

/boot/message

/boot/initrd-2.6.18-128.el5.img

....以下省略....

# 透過這個 find 我們可以找到 /boot 底下應該要存在的檔名！包括檔案與目錄

[root@www ~]# find /boot | cpio -ocvB &gt; /tmp/boot.cpio

[root@www ~]# ll -h /tmp/boot.cpio

-rw-r--r-- 1 root root 16M Dec 17 23:30 /tmp/boot.cpio

我們使用 find /boot 可以找出檔名，然後透過那條管線 (|, 亦即鍵盤上的 shift+\ 的組合)， 就能將檔名傳給 cpio 來進行處理！最終會得到 /tmp/boot.cpio 那個檔案喔！接下來讓我們來進行解壓縮看看。

範例：將剛剛的檔案給他在 /root/ 目錄下解開

[root@www ~]# cpio -idvc &lt; /tmp/boot.cpio

[root@www ~]# ll /root/boot

# 你可以自行比較一下 /root/boot 與 /boot 的內容是否一模一樣！

事實上 cpio 可以將系統的資料完整的備份到磁帶機上頭去喔！如果你有磁帶機的話！

備份：find / | cpio -ocvB &gt; /dev/st0

還原：cpio -idvc &lt; /dev/st0

這個 cpio 好像不怎麼好用呦！但是，他可是備份的時候的一項利器呢！因為他可以備份任何的檔案， 包括 /dev 底下的任何裝置檔案！所以他可是相當重要的呢！而由於 cpio 必需要配合其他的程式，例如 find 來建立檔名，所以 cpio 與管線命令及資料流重導向的相關性就相當的重要了！

其實系統裡面已經含有一個使用 cpio 建立的檔案喔！那就是 /boot/initrd-xxx 這個檔案啦！ 現在讓我們來將這個檔案解壓縮看看，看你能不能發現該檔案的內容為何？

# 1\. 我們先來看看該檔案是屬於什麼檔案格式，然後再加以處理：

[root@www ~]# file /boot/initrd-2.6.18-128.el5.img

/boot/initrd-2.6.18-128.el5.img: gzip compressed data, ...

# 唔！看起來似乎是使用 gzip 進行壓縮過～那如何處理呢？

# 2\. 透過更名，將該檔案增加副檔名，然後予以解壓縮看看：

[root@www ~]# mkdir initrd

[root@www ~]# cd initrd

[root@www initrd]# cp /boot/initrd-2.6.18-128.el5.img initrd.img.gz

[root@www initrd]# gzip -d initrd.img.gz

[root@www initrd]# ll

-rw------- 1 root root 5408768 Dec 17 23:53 initrd.img

[root@www initrd]# file initrd.img

initrd.img: ASCII cpio archive (SVR4 with no CRC)

# 嘿嘿！露出馬腳了吧！確實是 cpio 的文件檔喔！

# 3\. 開始使用 cpio 解開此檔案：

[root@www initrd]# cpio -iduvc &lt; initrd.img

sbin

init

sysroot

....以下省略....

# 瞧！這樣就將這個檔案解開囉！這樣瞭解乎？

--------------------------------------------------------------------------------

重點回顧