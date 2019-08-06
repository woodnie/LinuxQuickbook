### mail {#mail}

mail

NAME

    mail - send and receive mail

SYNOPSIS

    [-s subject] [-c cc-addr] [-b bcc-addr] to-addr...

**Example,Send an email:**

[root@www ~]# mail vbird1 -s &quot;nice to meet you&quot;

Hello, D.M. Tsai

Nice to meet you in the network.

You are so nice. byebye!

. &lt;==这里很重要喔，结束时，最后一行输入小数点 . 即可！

Cc: &lt;==这里是所谓的『副本』，不需要寄给其他人，所以直接 [Enter]

[root@www ~]# &lt;==出现提示字符，表示输入完毕了！

**[root@www ~]# mail**

| 令 | 意义 |
| --- | --- |
| h | 列出信件标头；如果要查阅 40 封信件左右的信件标头，可以输入『 h 40 』 |
| d | 删除后续接的信件号码，删除单封是『 d10 』，删除 20~40 封则为『 d20-40 』。 不过，这个动作要生效的话，必须要配合 q 这个命令才行(参考底下说明)！ |
| s | 将信件储存成文件。例如我要将第 5 封信件的内容存成 ~/mail.file:『s 5 ~/mail.file』 |
| x | 或者输入 exit 都可以。这个是『不作任何动作离开 mail 程序』的意思。 不论你刚刚删除了什么信件，或者读过什么，使用 exit 都会直接离开 mail，所以刚刚进行的删除与阅读工作都会无效。 如果您只是查阅一下邮件而已的话，一般来说，建议使用这个离开啦！除非你真的要删除某些信件。 |
| q | 相对于 exit 是不动作离开， q 则会进行两项动作： 1\. 将刚刚删除的信件移出 mailbox 之外； 2\. 将刚刚有阅读过的信件存入 ~/mbox ，且移出 mailbox 之外。鸟哥通常不很喜欢使用 q 离开， 因为，很容易忘记读过什么咚咚～导致信件给他移出 mailbox 说～ |