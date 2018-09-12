#### performance {#performance}

找到最耗CPU的java线程

ps命令

命令：

ps -mp pid -o THREAD,tid,time 或者 ps -Lfp pid

结果展示：

　　这个命令的作用，主要是可以获取到对应一个进程下的线程的一些信息。 比如你想分析一下一个java进程的一些运行瓶颈点，可以通过该命令找到所有当前Thread的占用CPU的时间，也就是这里的最后一列。

　　比如这里找到了一个TID : 30834 ，所占用的TIME时间最高。

　　通过 printf &quot;%x\n&quot; 30834 首先转化成16进制， 继续通过jstack命令dump出当前的jvm进程的堆栈信息。 通过Grep命令即可以查到对应16进制的线程id信息，很快就可以找到对应最耗CPU的代码快在哪。

　　简单的解释下，jstack下这一串线程信息内容：

800 nid=0x7d9b waiting on condition [0x0000000046f66000]&quot;DboServiceProcessor-4-thread-295&quot; daemon prio=10 tid=0x00002aab047a9800 nid= 0x7d9b waiting oncondition [0x0000000046f66000]&lt; /FONT&gt;

　　nid : 对应的 linux操作系统下的tid，就是前面转化的16进制数字

　　tid: 这个应该是jvm的jmm内存规范中的唯一地址定位，如果你详细分析jvm的一些内存数据时用得上，我自己还没到那种程度，所以先放下

top命令

命令：

top -Hp pid

结果显示：

　　和前面的效果一下，你可以实时的跟踪并获取指定进程中最耗cpu的线程。 再用前面的方法提取到对应的线程堆栈信息。