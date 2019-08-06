### tload {#tload}

tload

NAME

      tload - graphic representation of system load average

功能说明：显示系统负载状况。

语　　法：tload [-V][-d &lt;间隔秒数&gt;][-s &lt;刻度大小&gt;][终端机编号]

补充说明：tload指令使用ASCII字符简单地以文字模式显示系统负载状态。假设不给予终端机编号，则会在执行tload指令的终端机显示负载情形。

参　　数：

　-d&lt;间隔秒数&gt; 　设置tload检测系统负载的间隔时间，单位以秒计算。

　-s&lt;刻度大小&gt; 　设置图表的垂直刻度大小，单位以列计算。

　-V 　显示版本信息。