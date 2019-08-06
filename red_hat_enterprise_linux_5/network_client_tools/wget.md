### wget {#wget}

**wget**

[root@node1 ~]# wget~URL~~~

**必要参数** -P   指定一个下载目录 -c   断点续传，继续下载已下载了一部分的文件 -v   显示详细的处理信息 -x   强制建立目录 -b   后台执行 -r   递归处理 -q   不显示信息 -nc 不覆盖已存在的文件 -nd 不创建目录 --spider 不下任何数据 --limit-rate 设置下载的速率 **选择参数** -t&lt; 数值 &gt; 设置重试次数 -o&lt; 文件 &gt; 把显示的信息输出到指定文件 -w&lt; 时间 &gt; 设置下载不同文件之间等待的时间 -Q&lt; 数值 &gt; 限制大小 -h 帮助信息  

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

wget 是一个从网络上自动下载文件的自由工具。它支持 HTTP ， HTTPS 和 FTP 协议，可以使用 HTTP 代理 . 所谓的自动下载是指， wget 可以在用户退出系统的之后在后台执行。这意味这你可以登录系统，启动一个 wget 下载任务，然后退出系统， wget 将在后台执行直到任务完成，相对于其它大部分浏览器在下载大量数据时需要用户一直的参与，这省去了极大的麻烦。 ~~~~~~wget 可以跟踪 HTML 页面上的链接依次下载来创建远程服务器的本地版本，完全重建原始站点的目录结构。这又常被称作 &quot; 递归下载 &quot; 。在递归下载的时候， wget 遵循 Robot Exclusion 标准 (/robots.txt). wget 可以在下载的同时，将链接转换成指向本地文件，以方便离线浏览。 ~~~~~~wget 非常稳定 , 它在带宽很窄的情况下和不稳定网络中有很强的适应性 . 如果是由于网络的原因下载失败， wget 会不断的尝试，直到整个文件下载完毕。如果是服务 器打断下载过程，它会再次联到服务器上从停止的地方继续下载。这对从那些限定了链接时间的服务器上下载大文件非常有用。 wget 的常见用法： wget 不但功能强大，而且使用起来比较简单， 基本的语法是： wget [ 参数列表 ] &quot;URL&quot; 用 &quot;&quot; 引起来可以避免因 URL 中有特殊字符造成的下载出错。 下面就结合具体的例子来说明一下 wget 的用法。 ~~~~1 、下载整个 http 或者 ftp 站点。 ~~~~wget   [http://place.your.url/here](http://place.your.url/here) ~~~~ 这个命令可以将 [http://place.your.url/here](http://place.your.url/here) 首页下载下来。使用 -x 会强制建立服务器上一模一样的目录，如果使用 -nd 参数，那么服务器上下载的所有内容都会加到本地当前目录。  

~~~~wget -r     [http://place.your.url/here](http://place.your.url/here) ~~~~ 这个命令会按照递归的方法，下载服务器上所有的目录和文件，实质就是下载整个网站。这个命令一定要小心使用，因为在下载的时候，被下载网站指向的所有地址 同样会被下载，因此，如果这个网站引用了其他网站，那么被引用的网站也会被下载下来！基于这个原因，这个参数不常用。可以用 -l number 参数来指定下载的层次。例如只下载两层，那么使用 -l 2 。    

~~~~ 要是您想制作镜像站点，那么可以使用－ m 参数，例如： wget -m     [http://place.your.url/here](http://place.your.url/here) ~~~~ 这时 wget 会自动判断合适的参数来制作镜像站点。此时， wget 会登录到服务器上，读入 robots.txt 并按 robots.txt 的规定来执行。

~~~~2 、断点续传。 ~~~~ 当文件特别大或者网络特别慢的时候，往往一个文件还没有下载完，连接就已经被切断，此时就需要断点续传。 wget 的断点续传是自动的，只需要使用 -c 参数，例如： ~~~~wget -c     [http://the.url.of/incomplete/file](http://the.url.of/incomplete/file) ~~~~ 使用断点续传要求服务器支持断点续传。 -t 参数表示重试次数，例如需要重试 100 次，那么就写 -t 100 ，如果设成 -t 0 ，那么表示无穷次重试，直到连接成功。 -T 参数表示超时等待时间，例如 -T 120 ，表示等待 120 秒连接不上就算超时。    

~~~~3 、批量下载。 ~~~~ 如果有多个文件需要下载，那么可以生成一个文件，把每个文件的 URL 写一行，例如生成文件 download.txt ，然后用命令： wget -i download.txt 这样就会把 download.txt 里面列出的每个 URL 都下载下来。（如果列的是文件就下载文件，如果列的是网站，那么下载首页）

~~~~4 、选择性的下载。 ~~~~ 可以指定让 wget 只下载一类文件，或者不下载什么文件。例如： ~~~~wget -m --reject=gif &lt;   /FONT&gt; [http://target.web.site/subdirectory](http://target.web.site/subdirectory) ~~~~ 表示下载 [http://target.web.site/subdirectory](http://target.web.site/subdirectory) ，但是忽略 gif 文件。 --accept=LIST 可以接受的文件类型， --reject=LIST 拒绝接受的文件类型。 &lt; /FONT&gt;

~~~~5 、密码和认证。 ~~~~wget 只能处理利用用户名 / 密码方式限制访问的网站，可以利用两个参数： ~~~~--http-user=USER 设置 HTTP 用户 ~~~~--http-passwd=PASS 设置 HTTP 密码 ~~~~ 对于需要证书做认证的网站，就只能利用其他下载工具了，例如 curl 。

~~~~6 、利用代理服务器进行下载。 ~~~~ 如果用户的网络需要经过代理服务器，那么可以让 wget 通过代理服务器进行文件的下载。此时需要在当前用户的目录下创建一个 .wgetrc 文件。文件中可以设置代理服务器： ~~~~http-proxy = 111.111.111.111:8080~~~~ftp-proxy = 111.111.111.111:8080~~~~ 分别表示 http 的代理服务器和 ftp 的代理服务器。如果代理服务器需要密码则使用： ~~~~--proxy-user=USER 设置代理用户 ~~~~--proxy-passwd=PASS 设置代理密码 ~~~~ 这两个参数。 ~~~~ 使用参数 --proxy=on/off 使用或者关闭代理。 ~~~~wget 还有很多有用的功能，需要用户去挖掘。

+++++++++++++++++++++++++++++++++++++++++++++++++ Usage: wget [OPTION]... [URL]... 用 wget 做站点镜像 :wget -r -p -np -khttp://dsec.pku.edu.cn/~us.. (     [http://dsec.pku.edu.cn/~us](http://dsec.pku.edu.cn/~us) ..)-r 表示递归下载 , 会下载所有的链接 , 不过要注意的是 , 不要单独使用这个参数 , 因为如果你要下载的网站也有别的网站的链接 ,wget 也会把别的网站的东西下载下来 , 所以要加上 -np 这个参数 , 表示不下载别的站点的链接 . -k 表示将下载的网页里的链接修改为本地链接 .-p 获得所有显示网页所需的元素 , 比如图片什么的 .# 或者 wget -mhttp://www.tldp.org/LDP/ab.. (   [http://www.tldp.org/LDP/ab](http://www.tldp.org/LDP/ab) ..)

~

在不稳定的网络上下载一个部分下载的文件，以及在空闲时段下载 wget -t 0 -w 31 -chttp://dsec.pku.edu.cn/BBC.. (       [http://dsec.pku.edu.cn/BBC](http://dsec.pku.edu.cn/BBC) ..) -o down.log &amp;# 或者从 filelist 读入要下载的文件列表 wget -t 0 -w 31 -c -Bftp://dsec.pku.edu.cn/linu.. (       [ftp://dsec.pku.edu.cn/linu](ftp://dsec.pku.edu.cn/linu) ..) -i filelist.txt -o down.log &amp; 上面的代码还可以用来在网络比较空闲的时段进行下载。我的用法是 : 在 mozilla 中将不方便当时下载的 URL 链接拷贝到内存中然后粘贴到文件 filelist.txt 中， 在晚上要出去系统前执行上面代码的第二条。

~

使用代理下载 wget -Y on -p -khttps://sourceforge.net/pr.. (     [https://sourceforge.net/pr](https://sourceforge.net/pr) ..)

代理可以在环境变量或 wgetrc 文件中设定 # 在环境变量中设定代理 export PROXY=http://211.90.168.94:8080/# 在 ~/.wgetrc 中设定代理 http_proxy =http://proxy.yoyodyne.com:.. ( [http://proxy.yoyodyne.com](http://proxy.yoyodyne.com) :..)ftp_proxy =http://proxy.yoyodyne.com:.. ( [http://proxy.yoyodyne.com](http://proxy.yoyodyne.com) :..)  

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

wget 各种选项分类列表 * 启动 ~~-V, --version~~~~~~~~~~~ 显示 wget 的版本后退出 ~~-h,~~~~~--help~~~~~~~~~~~~~~ 打印语法帮助 ~~-b,~~~~~--background~~~~~~~~ 启动后转入后台执行 ~~-e,~~~~~--execute=COMMAND~~~ 执行 `.wgetrc 格式的命令， wgetrc 格式参见 /etc/wgetrc 或 ~/.wgetrc* 记录和输入文件 ~~-o,~~~~~--output-file=FILE~~~~~ 把记录写到 FILE 文件中 ~~-a,~~~~~--append-output=FILE~~~ 把记录追加到 FILE 文件中 ~~-d, --debug~~~~~~~~~~~~~~~~ 打印调试输出 ~~-q,~~~~~--quiet~~~~~~~~~~~~~~~~ 安静模式 ( 没有输出 )~~-v,~~~~~--verbose~~~~~~~~~~~~~~ 冗长模式 ( 这是缺省设置 )~~-nv,~~~--non-verbose~~~~~~~~~~ 关掉冗长模式，但不是安静模式 ~~-i,~~~~~~--input-file=FILE~~~~~~ 下载在 FILE 文件中出现的 URLs~~-F,~~~~~--force-html~~~~~~~~~~~ 把输入文件当作 HTML 格式文件对待 ~~-B,~~~~~--base=URL~~~~~~~~~~~~~ 将 URL 作为在 -F -i 参数指定的文件中出现的相对链接的前缀 ~~~~~~~~~~~~--sslcertfile=FILE~~~~~ 可选客户端证书 ~~~~~~~~~~~~--sslcertkey=KEYFILE~~~ 可选客户端证书的 KEYFILE~~~~~~~~~~~~--egd-file=FILE~~~~~~~~ 指定 EGD socket 的文件名 * 下载 ~~~~~~~~~~~~--bind-address=ADDRESS~~~ 指定本地使用地址 ( 主机名或 IP ，当本地有多个 IP 或名字时使用 )~~-t,~~~~~--tries=NUMBER~~~~~~~~~~~ 设定最大尝试链接次数 (0 表示无限制 ).~~-O~~~~~--output-document=FILE~~~ 把文档写到 FILE 文件中 ~~-nc,~~--no-clobber~~~~~~~~~~~~~ 不要覆盖存在的文件或使用 .# 前缀 ~~-c,~~~~--continue~~~~~~~~~~~~~~~ 接着下载没下载完的文件 ~~~~~~~~~~~--progress=TYPE~~~~~~~~~~ 设定进程条标记 ~~-N, --timestamping~~~~~~~~~~~ 不要重新下载文件除非比本地文件新 ~~-S,~~~--server-response~~~~~~~~ 打印服务器的回应 ~~~~~~~~~--spider~~~~~~~~~~~~~~~~~ 不下载任何东西 ~~-T, --timeout=SECONDS~~~~~~~~ 设定响应超时的秒数 ~~-w,~~--wait=SECONDS~~~~~~~~~~~ 两次尝试之间间隔 SECONDS 秒 ~~~~~~--waitretry=SECONDS~~~~~~ 在重新链接之间等待 1...SECONDS 秒 ~~~~~~--random-wait~~~~~~~~~~~~ 在下载之间等待 0...2*WAIT 秒 ~~-Y,~~~--proxy=on/off~~~~~~~~~~~ 打开或关闭代理 ~~-Q,~~--quota=NUMBER~~~~~~~~~~~ 设置下载的容量限制 ~~~~~~~~~~--limit-rate=RATE~~~~~~~~ 限定下载输率 * 目录 ~~-nd~~--no-directories~~~~~~~~~~~~ 不创建目录 ~~-x,~~~~--force-directories~~~~~~~~~ 强制创建目录 ~~-nH, --no-host-directories~~~~~~~ 不创建主机目录 ~~-P,~~--directory-prefix=PREFIX~~~ 将文件保存到目录 PREFIX/...~~~~~~--cut-dirs=NUMBER~~~~~~~~~~~ 忽略 NUMBER 层远程目录 * HTTP 选项 ~~~~~~~~~--http-user=USER~~~~~~ 设定 HTTP 用户名为 USER.~~~~~~~~--http-passwd=PASS~~~~ 设定 http 密码为 PASS.~~-C,~~--cache=on/off~~~~~~~~ 允许 / 不允许服务器端的数据缓存 ( 一般情况下允许 ).~~-E,~~--html-extension~~~~~~ 将所有 text/html 文档以 .html 扩展名保存 ~~~~~~~~--ignore-length~~~~~~~ 忽略 `Content-Length 头域 ~~~~~~~~~--header=STRING~~~~~~~ 在 headers 中插入字符串 STRING~~~~~~~~--proxy-user=USER~~~~~ 设定代理的用户名为 USER~~~~~~~~--proxy-passwd=PASS~~~ 设定代理的密码为 PASS~~~~~--referer=URL~~~~~~~~~ 在 HTTP 请求中包含 `Referer: URL 头 ~~-s,~~--save-headers~~~~~~~~ 保存 HTTP 头到文件 ~~-U,~~--user-agent=AGENT~~~~ 设定代理的名称为 AGENT 而不是 Wget/VERSION.~~~~~~~--no-http-keep-alive~~ 关闭 HTTP 活动链接 ( 永远链接 ).~~~~~~~--cookies=off~~~~~~~~~ 不使用 cookies.~~~~~~~--load-cookies=FILE~~~ 在开始会话前从文件 FILE 中加载 cookie~~~~~~~--save-cookies=FILE~~~ 在会话结束后将 cookies 保存到 FILE 文件中 * FTP 选项 ~~-nr, --dont-remove-listing~~~ 不移走 `.listing 文件 ~~-g,~~--glob=on/off~~~~~~~~~~~ 打开或关闭文件名的 globbing 机制 ~~~~~~~--passive-ftp~~~~~~~~~~~ 使用被动传输模式 ( 缺省值 ).~~~~~~~--active-ftp~~~~~~~~~~~~ 使用主动传输模式 ~~~~~~~--retr-symlinks~~~~~~~~~ 在递归的时候，将链接指向文件 ( 而不是目录 )* 递归下载 ~~-r,~~--recursive~~~~~~~~~~ 递归下载－－慎用 !~~-l,~~--level=NUMBER~~~~~~~ 最大递归深度 (inf 或 0 代表无穷 ).~~~~~~~--delete-after~~~~~~~ 在现在完毕后局部删除文件 ~~-k,~~--convert-links~~~~~~ 转换非相对链接为相对链接 ~~-K,~~--backup-converted~~~ 在转换文件 X 之前，将之备份为 X.orig~~-m,~~--mirror~~~~~~~~~~~~~ 等价于 -r -N -l inf -nr.~~-p,~~--page-requisites~~~~ 下载显示 HTML 文件的所有图片 * 递归下载中的包含和不包含 (accept/reject) ~~-A,~~--accept=LIST~~~~~~~~~~~~~~~~ 分号分隔的被接受扩展名的列表 ~~-R,~~--reject=LIST~~~~~~~~~~~~~~~~ 分号分隔的不被接受的扩展名的列表 ~~-D,~~--domains=LIST~~~~~~~~~~~~~~~ 分号分隔的被接受域的列表 ~~~~~~~~~~--exclude-domains=LIST~~~~~~~ 分号分隔的不被接受的域的列表 ~~~~~~~~~~--follow-ftp~~~~~~~~~~~~~~~~~ 跟踪 HTML 文档中的 FTP 链接 ~~~~~~~~~~--follow-tags=LIST~~~~~~~~~~~ 分号分隔的被跟踪的 HTML 标签的列表 ~~-G,~~--ignore-tags=LIST~~~~~~~~~~~ 分号分隔的被忽略的 HTML 标签的列表 ~~-H,~~~--span-hosts~~~~~~~~~~~~~~~~~ 当递归时转到外部主机 ~~-L,~~~--relative~~~~~~~~~~~~~~~~~~~ 仅仅跟踪相对链接 ~~-I,~~~~--include-directories=LIST~~~ 允许目录的列表 ~~-X,~~--exclude-directories=LIST~~~ 不被包含目录的列表 ~~-np, --no-parent~~~~~~~~~~~~~~~~~~ 不要追溯到父目录  

~

wget -S --spider url 不下载只显示过程