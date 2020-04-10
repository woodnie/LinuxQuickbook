### od {#od}

非純文字檔： od

我們上面提到的，都是在查閱純文字檔的內容。 那麼萬一我們想要查閱非文字檔，舉例來說，例如 /usr/bin/passwd 這個執行檔的內容時， 又該如何去讀出資訊呢？事實上，由於執行檔通常是 binary file ，使用上頭提到的指令來讀取他的內容時， 確實會產生類似亂碼的資料啊！那怎麼辦？沒關係，我們可以利用 od 這個指令來讀取喔！

[root@www ~]# od [-t TYPE] 檔案選項或參數：

-t ：後面可以接各種『類型 (TYPE)』的輸出，例如：

a ：利用預設的字元來輸出；

c ：使用 ASCII 字元來輸出

d[size] ：利用十進位(decimal)來輸出資料，每個整數佔用 size bytes ；

f[size] ：利用浮點數值(floating)來輸出資料，每個數佔用 size bytes ；

o[size] ：利用八進位(octal)來輸出資料，每個整數佔用 size bytes ；

x[size] ：利用十六進位(hexadecimal)來輸出資料，每個整數佔用 size bytes ；