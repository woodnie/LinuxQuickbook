### mkswap {#mkswap}

mkswap

NAME

      mkswap - set up a Linux swap area

功能说明：设置交换区(swap area)。

语　　法：mkswap [-cf][-v0][-v1][设备名称或文件][交换区大小]

补充说明：mkswap可将磁盘分区或文件设为Linux的交换区。

参　　数：

-c   建立交换区前，先检查是否有损坏的区块。

-f   在SPARC电脑上建立交换区时，要加上此参数。

-v0   建立旧式交换区，此为预设值。

-v1   建立新式交换区。

[交换区大小]   指定交换区的大小，单位为1024字节。