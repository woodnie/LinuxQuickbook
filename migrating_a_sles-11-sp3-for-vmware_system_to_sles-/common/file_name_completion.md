### file name completion {#file-name-completion}

使用文件名完成

如果不需要在命令提示符处键入长的、令人费解的文件名，这是不是很棒呢？的确，您不需要这样做。相反，您可以配置最流行的 UNIX Shell 以使用文件名完成。该功能在各个 Shell 中的工作方式略有不同，因此我将向您展示如何在最流行的 Shell 中使用文件名完成。文件名完成使您可以更快地输入并避免错误。懒惰？也许吧。效率更高？当然！

我正在运行哪种 Shell？

如果您不知道目前使用的是哪一种 Shell，会怎么样？虽然这个诀窍不是另外 10 个好习惯的正式组成部分，但它仍然很有用。如清单 1 所示，您可以使用 echo $0 或 ps -p $$ 命令显示您正在使用的 Shell。对于我来说，运行的是 Bash Shell。

清单 1\. 确定您的 Shell

$ echo $0

-bash

$ ps -p $$

PID TTY           TIME CMD

6344 ttys000    0:00.02 -bash

C Shell

C Shell 支持最直接文件名完成功能。设置 filec 变量可启用该功能。（您可以使用命令 set filec。）在您开始键入文件名后，可以按 Esc 键，Shell 将完成文件名，或完成尽可能多的部分。例如，假设您拥有名为 file1、file2 和 file3 的文件。如果您键入 f，然后按 Esc 键，将填充 file，而您必须键入 1、2 或 3 来完成相应的文件名。

Bash

Bash Shell 也提供了文件名完成，但使用 Tab 键代替 Esc 键。您在 Bash Shell 中不需要设置任何选项即可启用文件名完成，该选项是缺省设置的。Bash 还实现了其他功能。键入文件名的一部分后，按 Tab 键，如果有多个文件满足您的请求，并且您需要添加文本以选择其中一个文件，那么您可以多按 Tab 键两次，以显示与您目前键入的内容相匹配的文件的列表。使用之前名为 file1、file2 和 file3 的文件示例，首先键入 f。当您按一次 Tab 键时，Bash 完成 file；再按一次 Tab 键时，将展开列表 file1 file2 file3。

Korn Shell

对于 Korn Shell 用户，文件名完成取决于 EDITOR 变量的值。如果 EDITOR 设置为 vi，那么您键入部分名称，然后按 Esc 键，后跟反斜杠 (\) 字符。如果 EDITOR 设置为 emacs，那么您键入部分名称，然后按两次 Esc 键以完成文件名。