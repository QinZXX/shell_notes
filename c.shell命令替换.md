## shell命令替换：将命令的输出结果赋值给变量

Shell 命令替换是指将命令的输出结果赋值给某个变量。比如，在某个目录中输入 `ls` 命令可查看当前目录中所有的文件，但如何将输出内容存入某个变量中呢？这就需要使用命令替换了，这也是 Shell 编程中使用非常频繁的功能。

Shell 中有两种方式可以完成命令替换，一种是反引号`` ``，一种是`$()`，使用方法如下：

```bash
variable=`commands`
variable=$(commands)
```

其中，`variable` 是变量名，`commands` 是要执行的命令。`commands` 可以只有一个命令，也可以有多个命令，多个命令之间以分号`;`分隔。

例如，`date` 命令用来获得当前的系统时间，使用命令替换可以将它的结果赋值给一个变量。

```bash
#!/bin/bash

begin_time=`date`    #开始时间，使用``替换
sleep 20s            #休眠20秒
finish_time=$(date)  #结束时间，使用$()替换

echo "Begin time: $begin_time"
echo "Finish time: $finish_time"
```

运行脚本，20 秒后可以看到输出结果：
Begin time: Fri Nov  6 22:13:03 CST 2020
Finish time: Fri Nov  6 22:13:23 CST 2020



使用 data 命令的`%s`格式控制符可以得到当前的 UNIX 时间戳，这样就可以直接计算脚本的运行时间了。UNIX 时间戳是指从 1970 年 1 月 1 日 00:00:00 到目前为止的秒数。

```bash
#!/bin/bash

begin_time=`date +%s`    #开始时间，使用``替换
sleep 20s                #休眠20秒
finish_time=$(date +%s)  #结束时间，使用$()替换
run_time=$((finish_time - begin_time))  #时间差

echo "begin time: $begin_time"
echo "finish time: $finish_time"
echo "run time: ${run_time}s"
```

运行脚本，20 秒后可以看到输出结果：
begin time: 1604672285
finish time: 1604672305
run time: 20s

第 6 行代码中的`(( ))`是 Shell 数学计算命令。和 C++、C#、Java 等编程语言不同，在 Shell 中进行数据计算不那么方便，必须使用专门的数学计算命令，`(( ))`就是其中之一。



注意，**如果被替换的命令的输出内容包括多行（也即有换行符），或者含有多个连续的空白符，那么在输出变量时应该将变量用双引号包围，否则系统会使用默认的空白符来填充，这会导致换行无效，以及连续的空白符被压缩成一个**。

看下面的代码：

```bash
#!/bin/bash

LSL=`ls -l`
echo $LSL  #不使用双引号包围
echo "--------------------------"  #输出分隔符
echo "$LSL"  #使用引号包围
```

运行结果：

```bash
total 28 -rwxr-xr-x. 1 root root 143 Nov 5 07:25 1_demo.sh -rw-r--r--. 1 root root 27 Nov 5 07:51 2_demo -rw-r--r--. 1 root root 211 Nov 6 02:29 3demo -rw-r--r--. 1 root root 40 Nov 6 02:58 4demo -rwxrwxrwx. 1 root root 329 Nov 6 03:24 5demo -rw-r--r--. 1 root root 34 Nov 6 06:40 6demo -rw-r--r--. 1 root root 147 Nov 6 22:20 7demo
--------------------------
total 28
-rwxr-xr-x. 1 root root 143 Nov  5 07:25 1_demo.sh
-rw-r--r--. 1 root root  27 Nov  5 07:51 2_demo
-rw-r--r--. 1 root root 211 Nov  6 02:29 3demo
-rw-r--r--. 1 root root  40 Nov  6 02:58 4demo
-rwxrwxrwx. 1 root root 329 Nov  6 03:24 5demo
-rw-r--r--. 1 root root  34 Nov  6 06:40 6demo
-rw-r--r--. 1 root root 147 Nov  6 22:20 7demo

```

所以，**为了防止出现格式混乱的情况，建议在输出变量时加上双引号**。

## 再谈反引号和 $()

原则上讲，上面提到的两种变量替换的形式是等价的，可以随意使用；但是，反引号毕竟看起来像单引号，有时候会对查看代码造成困扰，而使用 `$()` 就相对清晰，能有效避免这种混乱。而且有些情况必须使用` $()`：

- **`$()` 支持嵌套，反引号不行**。

下面的例子演示了使用计算 ls 命令列出的第一个文件的行数，这里使用了两层嵌套。

```bash
[]$ Fir_File_Lines=$(wc -l $(ls | sed -n '1p'))
[]$ echo "$Fir_File_Lines"
36 anaconda-ks.cfg
```

要注意的是，**`$() `仅在 Bash Shell 中有效，而反引号可在多种 Shell 中使用**。所以这两种命令替换的方式各有特点，究竟选用哪种方式全看个人需求。