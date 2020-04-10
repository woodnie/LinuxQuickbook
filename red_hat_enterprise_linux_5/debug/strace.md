### strace {#strace}

strace - trace system calls and signals

strace 经常被认为是程序员调试的工具，但不止如此。它可以记录进程进行系统调用的详情，因此它也是一个非常好的诊断工具，例如你可以使用它来找出某个程序正在打开某个配置文件。

Strace 也有一个缺陷，但它在跟踪某个进程时会让该进程的性能变得非常差，因此请谨慎使用。