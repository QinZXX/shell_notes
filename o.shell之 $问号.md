## shell之 $？：获取函数返回值或者上一个命令的退出状态

`$?` 是一个特殊变量，用来**获取上一个命令的退出状态**，或者**上一个函数的返回值**。

所谓退出状态，就是**上一个命令**执行后的返回结果。**退出状态是一个数字，一般情况下，大部分命令执行成功会返回 0，失败返回 1**，这和C语言的 main() 函数是类似的。

不过，**也有一些命令返回其他值，表示不同类型的错误**。

## 1) $? 获取上一个命令的退出状态

编写下面的代码，并保存为 `test.sh`：

```shell
#!/bin/bash

if [ "$1" == 100 ]
then 
	exit 0  #参数正确，退出状态为0
else   
	exit 1  #参数错误，退出状态1
fi
```

`exit`表示退出当前 Shell 进程，我们**必须在新进程中运行** `test.sh`，否则当前 Shell 会话（终端窗口）会被关闭，我们就无法取得它的退出状态了。

例如，运行 `test.sh` 时传递参数 100：

```bash
[qzx@localhost ~]$ cd demo
[qzx@localhost demo]$ bash ./test.sh 100  #作为一个新进程运行
[qzx@localhost demo]$ echo $?
0
```


再如，运行 `test.sh` 时传递参数 89：

```bash
[qzx@localhost demo]$ bash ./test.sh 89  #作为一个新进程运行
[qzx@localhost demo]$ echo $?
1
```

## 2) $? 获取函数的返回值

编写下面的代码，并保存为 `test.sh`：

```shell
#!/bin/bash

#得到两个数相加的和
function add(){ 
	return `expr $1 + $2`
}
add 23 50  #调用函数
echo $?  #获取函数返回值
```

运行结果：
73

严格来说，**Shell 函数中的 return 关键字用来表示函数的退出状态，而不是函数的返回值**；Shell 不像其它编程语言，没有专门处理返回值的关键字。

以上处理方案在其它编程语言中没有任何问题，但是在 Shell 中是非常错误的，Shell 函数的返回值和其它编程语言大有不同。