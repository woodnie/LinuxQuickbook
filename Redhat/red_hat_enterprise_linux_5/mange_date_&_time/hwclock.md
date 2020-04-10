### hwclock {#hwclock}

hwclock(hardware clock)

功能说明：显示和设定硬件时钟。

语　　法：hwclock [--adjust][--debug][--directisa][--hctosys][--show][--systohc][--test]

[--utc][--version][--set --date=&lt;日期和时间&gt;]

补充说明：在Linux中有硬件时钟和系统时钟等两种时钟。硬件时钟是指主机板上的时钟设备，也就是通常可在BIOS画面设定的时钟。系统时钟则是指kernel中的时钟。当Linux启动时，系统时钟会去读取硬件时钟的设定，之后系统时钟即独立运作。所有Linux相关指令和函数都是读取系统时钟的设定。

参　　数：

 --adjust 　    #hwclock每次更改硬件时钟时,都会记录在/etc/adjtime文件中.使用--adjust参数,可使hwclock根据先前的记录来估算硬件时钟的偏差，并用来校正目前的硬件时钟

 --debug 　     #显示hwclock执行时周详的信息。

 --directisa 　 #hwclock预设从/dev/rtc设备来存取硬件时钟。若无法存取时，可用此参数直接以I/O指令来存取硬件时钟。

 --hctosys 　   #将系统时钟调整为和目前的硬件时钟一致。

 --set --date=  #&lt;日期和时间&gt; 　设定硬件时钟。

 --show 　      #显示硬件时钟的时间和日期。

 --systohc 　   #将硬件时钟调整为和目前的系统时钟一致。

 --test 　      #仅测试程式，而不会实际更改硬件时钟。

 --utc 　       #若要使用格林威治时间，请加入此参数，hwclock会执行转换的工作。

 --version 　   #显示版本信息。