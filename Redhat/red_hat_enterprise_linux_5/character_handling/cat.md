### cat {#cat}

cat

NAME

      cat - concatenate files and print on the standard output

-A, --show-all  

             equivalent to -vET

-b, --number-nonblank

             number nonblank output line

-s, --squeeze-blank

             never more than one single blank line

cat -A # 显示所有字符，包括控制字符和非打印字符

cat -b #给每行（非空行）输出编号

cat -s #把多行空行，一行输出

選項與參數：

-A  ：相當於 -vET 的整合選項，可列出一些特殊字符而不是空白而已；

-b  ：列出行號，僅針對非空白行做行號顯示，空白行不標行號！

-E  ：將結尾的斷行字元 $ 顯示出來；

-n  ：列印出行號，連同空白行也會有行號，與 -b 的選項不同；

-T  ：將 [tab] 按鍵以 ^I 顯示出來；

-v  ：列出一些看不出來的特殊字符