### ls {#ls}

ls

ls - list directory contents

常用选项：

-a     显示隐藏文件

-d     显示目录信息

-i      显示inode信息

-l      显示额外信息

-h     print sizes in human readable format (e.g., 1K 234M 2G)

-h     human-readable

-S    以文件大小排序

-t      以时间排序

-r      反向排序

-U    不排序，按照文件在目录内的位置显示出来

ls的其他可用的选项如下：

-a ,--all       不要隐藏文件名以&quot;.&quot;字符开头的文件

-A,--almost-all 不要显示&quot;.&quot;和&quot;..&quot;两个项目

-b,--escape     非图形字符以八进制escape列出

--block-size=SIZE    使用SIZE字节的block

-B,--ignore-backups  不要列出文件名以&quot;`&quot;字符结尾的文件(通常是备份文件)

-c 根据文件修改时间做排序,使用 -l显示ctime字节段

-C 以字段格式显示项目

--color[=WHEN]   设定是否以不同颜色区分不同文件类型,WHEN可以被设定为&quot;never&quot;,&quot;always&quot;,&quot;auto&quot;

-d --director    对于目录项目信息,列出目录项目的信息,而不包含在目录内的文件信息(目录内容)

-D,--dird        产生设计给Emas dird模式使用的输出

-f               不要排序,激活-aU,取消-lst.

-F,--classify    在文件项目上加标识,如:*,/,=,@

--vformat=WORD   利用关键词来完成ls的输出模式,这些关键词也可以利用其他选项来完成,可用的关键词有(括号内为替代选项):

                across(-x),commas(-m),horizontal(-x),long(-l),single-column(-l),vertical(-C).

--full-time      以完整的日期,时间格式列出时间与日期信息

-G.--no-group    禁止显示组群信息

-h,-human-readble             以用户看得懂的格式来列出文件的大小信息,例如:124K

-H,--si  以1000取代1024        作为统计单位,例如,1024bytes会被显示为1.024KB

--indicator-style=WORD        在项目名称上附加WORD种类的标识,可设定的值有:none(预设).classify(-F),file-type(-p)

-i,--inode                    打印每一个文件的索引编号

-I PATTERN,--ignore=PATTERN   不要列出符合PATTERN的项目

-k,--kilobytes                使用KB为单位,相当于--block-size=1024

-l                使用长清单模式,列出文件权限,大小,拥有权...等信息

-L,--dereference  打印符号连接指向项目

-m                使用逗号来分隔项目

-n,--numberic-uid-gid    以用户与组群编号取代它们的名称显示  

-N,--literal             打印列项目名称

-o                       使用长清单模式,但不具有组名称字段

-p,--file-type           在项目上附加标识(/,=,@.|)

-q,--hide-control-chars  对于非图形字符以&quot;?&quot;显示

--show-control-chars     显示非图形字符as-is(预设)

-Q,--quotion-name        在项目名称前后加上双引号

--quoting-style=WORD     使用WORD引述类型显示项目名称,可设定值有literal,shell,shell-always,c,escape

-r,--reverse       反向排序

-R,--recursive     递归显示下层子目录

-s,--size  以block 为单位显示每一个文件大小

-S  根据文件的大小排序

--sort=WORD  根据WORD规则进行排序,可设置的值有(括号内为关键词对应的选项):

            extension(-X),none(-U),size(-S),time(-t),version(-v),status(-c),atime(-u),acces(-u),use(-u)

--time=WORD  如果设定--sort=WORD,使用WORD时间字段取代修改时间作为排序的键值.可设定的值有:atime,access,use,ctime或 status

-t  根据修改时间排序

-T  COLS,--tabsize=COLS  假设&lt;tab&gt;字符间隔宽度为COLS,预设为8

-u  根据上次存取时间排序

-U  不要排序,根据项目在目录中的顺序来排序

-v  根据版本排序

-w  COLS,--width-=COLS  假设画面的宽度为COLS取代当前值

-x  按行列出项目,取代按栏列出

-X  按扩展名排序

-1  单行只显示一个文件

--help     显示说明

--version  显示版本信息

ls -l

-: 正规文档

d: 目录

l: 链接

b: 块设备

c: 字符设备

s: 資料接口檔(sockets)

p: 資料輸送檔(FIFO, pipe)