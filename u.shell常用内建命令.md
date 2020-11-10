## shell常用内建命令

 Shell 内建命令，是由 Bash 自身提供的命令，而不是文件系统中的某个可执行文件。

例如，用于进入或者切换目录的 cd 命令，虽然我们一直在使用它，但如果不加以注意很难意识到它与普通命令的性质是不一样的：该命令并不是某个外部文件，只要在 Shell 中你就一定可以运行这个命令。

可以使用 type 来确定一个命令是否是内建命令：

```bash
[root@localhost ~]# type cd
cd is a Shell builtin
[root@localhost ~]# type ip
ip is /usr/sbin/ip
```

由此可见，cd 是一个 Shell 内建命令，而 ip 是一个外部文件，它的位置是`/usr/sbin/ifconfig`。

系统变量`$PATH` 包含的目录中几乎聚集了系统中绝大多数的可执行命令，它们都是外部命令。

通常来说，内建命令会比外部命令执行得更快，执行外部命令时不但会触发磁盘 I/O，还需要 fork 出一个单独的进程来执行，执行完成后再退出。而执行内建命令相当于调用当前 Shell 进程的一个函数。

下表列出了 Bash Shell 中直接可用的内建命令。

| 命令      | 说明                                                  |
| --------- | ----------------------------------------------------- |
| :         | 扩展参数列表，执行重定向操作                          |
| .         | 读取并执行指定文件中的命令（在当前 shell 环境中）     |
| alias     | 为指定命令定义一个别名                                |
| bg        | 将作业以后台模式运行                                  |
| bind      | 将键盘序列绑定到一个 readline 函数或宏                |
| break     | 退出 for、while、select 或 until 循环                 |
| builtin   | 执行指定的 shell 内建命令                             |
| caller    | 返回活动子函数调用的上下文                            |
| cd        | 将当前目录切换为指定的目录                            |
| command   | 执行指定的命令，无需进行通常的 shell 查找             |
| compgen   | 为指定单词生成可能的补全匹配                          |
| complete  | 显示指定的单词是如何补全的                            |
| compopt   | 修改指定单词的补全选项                                |
| continue  | 继续执行 for、while、select 或 until 循环的下一次迭代 |
| declare   | 声明一个变量或变量类型。                              |
| dirs      | 显示当前存储目录的列表                                |
| disown    | 从进程作业表中刪除指定的作业                          |
| echo      | 将指定字符串输出到 STDOUT                             |
| enable    | 启用或禁用指定的内建shell命令                         |
| eval      | 将指定的参数拼接成一个命令，然后执行该命令            |
| exec      | 用指定命令替换 shell 进程                             |
| exit      | 强制 shell 以指定的退出状态码退出                     |
| export    | 设置子 shell 进程可用的变量                           |
| fc        | 从历史记录中选择命令列表                              |
| fg        | 将作业以前台模式运行                                  |
| getopts   | 分析指定的位置参数                                    |
| hash      | 查找并记住指定命令的全路径名                          |
| help      | 显示帮助文件                                          |
| history   | 显示命令历史记录                                      |
| jobs      | 列出活动作业                                          |
| kill      | 向指定的进程 ID(PID) 发送一个系统信号                 |
| let       | 计算一个数学表达式中的每个参数                        |
| local     | 在函数中创建一个作用域受限的变量                      |
| logout    | 退出登录 shell                                        |
| mapfile   | 从 STDIN 读取数据行，并将其加入索引数组               |
| popd      | 从目录栈中删除记录                                    |
| printf    | 使用格式化字符串显示文本                              |
| pushd     | 向目录栈添加一个目录                                  |
| pwd       | 显示当前工作目录的路径名                              |
| read      | 从 STDIN 读取一行数据并将其赋给一个变量               |
| readarray | 从 STDIN 读取数据行并将其放入索引数组                 |
| readonly  | 从 STDIN 读取一行数据并将其赋给一个不可修改的变量     |
| return    | 强制函数以某个值退出，这个值可以被调用脚本提取        |
| set       | 设置并显示环境变量的值和 shell 属性                   |
| shift     | 将位置参数依次向下降一个位置                          |
| shopt     | 打开/关闭控制 shell 可选行为的变量值                  |
| source    | 读取并执行指定文件中的命令（在当前 shell 环境中）     |
| suspend   | 暂停 Shell 的执行，直到收到一个 SIGCONT 信号          |
| test      | 基于指定条件返回退出状态码 0 或 1                     |
| times     | 显示累计的用户和系统时间                              |
| trap      | 如果收到了指定的系统信号，执行指定的命令              |
| type      | 显示指定的单词如果作为命令将会如何被解释              |
| typeset   | 声明一个变量或变量类型。                              |
| ulimit    | 为系统用户设置指定的资源的上限                        |
| umask     | 为新建的文件和目录设置默认权限                        |
| unalias   | 刪除指定的别名                                        |
| unset     | 刪除指定的环境变量或 shell 属性                       |
| wait      | 等待指定的进程完成，并返回退出状态码                  |


接下来的重点讲解几个常用的 Shell 内置命令。

### 1.alias命令:给命令起别名

alisa 用来给命令创建一个别名。若直接输入该命令且不带任何参数，则列出当前 Shell 进程中使用了哪些别名。现在你应该能理解类似`ll`这样的命令为什么与`ls -l`的效果是一样的吧。

下面来看一下有哪些命令被默认创建了别名：

```bash
[root@localhost sh_scripts]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```

你看，为了让我们使用方便，Shell 会给某些命令默认创建别名。

#### 使用 alias 命令自定义别名

使用 alias 命令自定义别名的语法格式为：

```bash
alias new_name='command'
```

比如，一般的关机命令是`shutdown -h now`，写起来比较长，这时可以重新定义一个关机命令，以后就方便多了。

```bash
alias myShutdown='shutdown -h now'
alias yourShutdown='poweroff'
```

再如，通过 date 命令可以获得当前的 UNIX 时间戳，具体写法为`date +%s`，如果嫌弃它太长或者不容易记住，那可以给它定义一个别名。

```bash
alias timeStamp='date +%s'
```


使用`date +%s`计算脚本的运行时间，简化代码:

```shell
#!/bin/bash

alias timestamp='date +%s'
begin=`timestamp`  
sleep 20s
finish=$(timestamp)
difference=$((finish - begin))
echo "run time: ${difference}s"
```

运行脚本，20 秒后看到输出结果：
run time: 20s

#### 别名只是临时的

在代码中使用 alias 命令定义的别名**只能在当前 Shell 进程中使用**，在子进程和其它进程中都不能使用。当前 Shell 进程结束后，别名也随之消失。

要想让别名对所有的 Shell 进程都有效，就得把别名写入 Shell 配置文件。**Shell 进程每次启动时都会执行配置文件中的代码做一些初始化工作，将别名放在配置文件中，那么每次启动进程都会定义这个别名**。

#### 使用 unalias 命令删除别名

使用 unalias 内建命令可以删除当前 Shell 进程中的别名。unalias 有两种使用方法：

- 第一种用法是在命令后跟上某个命令的别名，用于删除指定的别名。
- 第二种用法是在命令后接`-a`参数，删除当前 Shell 进程中所有的别名。


同样，这两种方法都是在当前 Shell 进程中生效的。要想永久删除配置文件中定义的别名，只能进入该文件手动删除。

```bash
# 删除 ll 别名
[qzx@localhost ~]$ unalias ll
# 再次运行该命令时，报“找不到该命令”的错误，说明该别名被删除了
[qzxn@localhost ~]$ ll
-bash: ll: command not found
```

### 2.echo命令：输出字符串

echo 用来在终端输出字符串，并在最后默认加上换行符。请看下面的例子：

```shell
#!/bin/bash

name="百度"
url="www.baidu.com"
echo "读者，你好！"  #直接输出字符串
echo $url  #输出变量
echo "${name}的网址是：${url}"  #双引号包围的字符串中可以解析变量
echo '${name}的网址是：${url}'  #单引号包围的字符串中不能解析变量
```

运行结果：

```bash
读者，你好！
www.baidu.com
百度的网址是：www.baidu.com
${name}的网址是：${url}
```

#### 不换行

echo 命令输出结束后默认会换行，如果**不希望换行，可以加上`-n`参数**，如下所示：

```shell
#!/bin/bash

name="Tom"
age=20
height=175
weight=62
echo -n "${name} is ${age} years old, "
echo -n "${height}cm in height "
echo "and ${weight}kg in weight."
echo "Thank you!"
```

运行结果：

```bash
Tom is 20 years old, 175cm in height and 62kg in weight.
Thank you!
```

#### 输出转义字符

默认情况下，echo 不会解析以反斜杠`\`开头的转义字符。比如，`\n`表示换行，echo 默认会将它作为普通字符对待。请看下面的例子：

```bash
[root@localhost ~]# echo "hello \nworld"
hello \nworld
```

我们可以**添加`-e`参数来让 echo 命令解析转义字符**。例如：

```bash
[root@localhost ~]# echo -e "hello \nworld"
hello
world
```

#### \c 转义字符

有了`-e`参数，我们也可以使用转义字符`\c`来强制 echo 命令不换行了。请看下面的例子：

```shell
#!/bin/bash

name="Tom"
age=20
height=175
weight=62
echo -e "${name} is ${age} years old, \c"
echo -e "${height}cm in height \c"
echo "and ${weight}kg in weight."
echo "Thank you!"
```

运行结果：

```bash
Tom is 20 years old, 175cm in height and 62kg in weight.
Thank you!
```

### 3.read命令：读取从键盘输入的数据

read 用来从标准输入中读取数据并赋值给变量。如果没有进行重定向，默认就是从键盘读取用户输入的数据；如果进行了重定向，那么可以从文件中读取数据。

read 命令的用法为：

```bash
read [-options] [variables]
```

`options`表示选项，如下表所示；`variables`表示用来存储数据的变量，可以有一个，也可以有多个。

`options`和`variables`都是可选的，如果没有提供变量名，那么读取的数据将存放到环境变量 REPLY 中。

| 选项         | 说明                                                         |
| :----------- | ------------------------------------------------------------ |
| -a array     | 把读取的数据赋值给数组 array，从下标 0 开始。                |
| -d delimiter | 用字符串 delimiter 指定读取结束的位置，而不是一个换行符（读取到的数据不包括 delimiter）。 |
| -e           | 在获取用户输入的时候，对功能键进行**编码转换**，不会直接显式功能键对应的字符。 |
| -n num       | 读取 num 个字符，而不是整行字符。                            |
| -p prompt    | 显示提示信息，提示内容为 prompt。                            |
| -r           | 原样读取（Raw mode），不把反斜杠字符解释为转义字符。         |
| -s           | 静默模式（Silent mode），不会在屏幕上显示输入的字符。当输入密码和其它确认信息的时候，这是很有必要的。 |
| -t seconds   | 设置超时时间，单位为秒。如果用户没有在指定时间内输入完成，那么 read 将会返回一个非 0 的退出状态，表示读取失败。 |
| -u fd        | 使用文件描述符 fd 作为输入源，而不是标准输入，类似于重定向。 |


【实例1】使用 read 命令给多个变量赋值。

```shell
#!/bin/bash

read -p "Enter some information > " name gender age
echo "名字：$name"
echo "性别：$gender"
echo "年龄：$age"
```

运行结果：

```bash
Enter some information > "Hongjian Cao" female 24↙
名字：Hongjian Cao
性别：female
年龄：24
```

注意，必须在一行内输入所有的值，不能换行，否则只能给第一个变量赋值，后续变量都会赋值失败。

本例还使用了`-p`选项，该选项会用一段文本来提示用户输入。

【示例2】只读取一个字符。

```shell
#!/bin/bash

read -n 1 -p "Enter a char > " char
printf "\n"  #换行
echo $char
```

运行结果：

```bash
Enter a char > 1
1
```

`-n 1`表示只读取一个字符。运行脚本后，只要用户输入一个字符，立即读取结束，不用等待用户按下回车键。

`printf "\n"`语句用来达到换行的效果，否则 echo 的输出结果会和用户输入的内容位于同一行，不容易区分。

【实例3】在指定时间内输入密码。

```shell
#!/bin/bash
if    
	read -t 20 -sp "Enter password in 20 seconds(once) > " pass1 && printf "\n" &&  #第一次输入密码    
	read -t 20 -sp "Enter password in 20 seconds(again)> " pass2 && printf "\n" &&  #第二次输入密码    
	[ $pass1 == $pass2 ]  #判断两次输入的密码是否相等
then    
	echo "Valid password"
else    
	echo "Invalid password"
fi
```

这段代码中，我们使用`&&`组合了多个命令，这些命令会依次执行，并且从整体上作为 if 语句的判断条件，只要其中一个命令执行失败（退出状态为非 0 值），整个判断条件就失败了，后续的命令也就没有必要执行了。

如果两次输入密码相同，运行结果为：

```bash
Enter password in 20 seconds(once) >
Enter password in 20 seconds(again)>
Valid password
```

如果两次输入密码不同，运行结果为：

```bash
Enter password in 20 seconds(once) >
Enter password in 20 seconds(again)>
Invalid password
```

如果第一次输入超时，运行结果为：

```bash
Enter password in 20 seconds(once) > Invalid password
```

如果第二次输入超时，运行结果为：

```bash
Enter password in 20 seconds(once) >
Enter password in 20 seconds(again)> Invalid password
```

### 4.exit命令：退出当前进程

exit 用来退出当前 Shell 进程，并返回一个退出状态；使用`$?`可以接收这个退出状态。

exit 命令可以接受一个整数值作为参数，代表退出状态。如果不指定，默认状态值是 0。

一般情况下，退出状态为 0 表示成功，退出状态为非 0 表示执行失败（出错）了。

exit 退出状态只能是一个介于 0~255 之间的整数，其中只有 0 表示成功，其它值都表示失败。

Shell 进程执行出错时，可以根据退出状态来判断具体出现了什么错误，比如打开一个文件时，我们可以指定 1 表示文件不存在，2 表示文件没有读取权限，3 表示文件类型不对。

编写下面的脚本，并命名为 test.sh：

```shell
#!/bin/bash

echo "befor exit"
exit 8
echo "after exit"
```

运行该脚本：

```bash
[qzx@localhost ~]$ bash ./test.sh
befor exit
```

可以看到，`"after exit"`并没有输出，这说明遇到 exit 命令后，test.sh 执行就结束了。

> 注意，exit 表示退出当前 Shell 进程，我们必须在新进程中运行 test.sh，否则当前 Shell 会话（终端窗口）会被关闭，我们就无法看到输出结果了。

我们可以紧接着使用`$?`来获取 test.sh 的退出状态：

```bash
[qzx@localhost ~]$ echo $?
8
```

