#### /etc/securetty {#etc-securetty}

　　&quot;/etc/securetty&quot;文件允许你规定&quot;root&quot;用户可以从那个TTY设备登录。登录程序（通常是&quot;/bin/login&quot;）需要读取&quot;/etc/securetty&quot;文件。它的格式是：列出来的tty设备都是允许登录的，注释掉或是在这个文件中不存在的都是不允许root登录的。

　　注释掉（在这一行的开头加上＃号）所有你想不让root登录的tty设备。

　　编辑securetty文件（vi /etc/securetty）象下面一样，注释掉一些行：

console

vc/1

vc/2

vc/3

vc/4

vc/5

vc/6

vc/7

vc/8

vc/9

tty1

tty2

tty3

tty4

tty5

tty6

tty7

tty8

tty9

pts/1

pts/2

pts/3

pts/4

pts/5

pts/6

pts/7

pts/8

pts/9

   当进行以上设置以后，root就只能在tty1(ctrl+alt+F1)登录

   把这个文件改名、或注释掉里面的文件，不会影响SSH应用，因为SSH远程登录使用的是PTS，不是tty，这个文件的修改可以阻止telnet 通过root登录。

疑问：

禁用pts可以阻止telnet,但不能阻止sh

/etc/securetty 文件

&quot;/etc/securetty&quot;文件允许你规定&quot;root&quot;用户可以从那个TTY设备登录。登录程序（通常是&quot;/bin/login&quot;）需要读取&quot;/etc/securetty&quot;文件。它的格式是：列出来的tty设备都是允许登录的，注释掉或是在这个文件中不存在的都是不允许root登录的。

　　注释掉（在这一行的开头加上＃号）所有你想不让root登录的tty设备。

tty就是tty，是一个很宽泛的名词，它是Teletype的缩写

如果你指的是/dev/tty，那指当前终端  

pts是pesudo tty slave，是伪终端的slave端  

console好像是指当前的控制台（或者监视器），比如说你Ctrl+Alt+x，然后echo &quot;123&quot; &gt; /dev/console，123总会显示在你的monitor上。

vc是virtual console，也可以理解为虚拟的监视器，当你Ctrl+Alt+x，就会切换到vc x，在/dev下面没有直接对应的设备文件，不过你如果尝试 echo &quot;123&quot; &gt; /dev/vcs1, 你在monitor上也能看到，不过要切换到对应的vc.

vt指的是virtual terminal，虚拟终端，在我看来指的就是虚拟控制台