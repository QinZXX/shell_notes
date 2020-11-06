# Shell模块化（source命令）

所谓模块化，就是把代码分散到多个文件或者文件夹。对于大中型项目，模块化是必须的，否则会在一个文件中堆积成千上万行代码，这简直是一种灾难。

基本上所有的编程语言都支持模块化，以达到代码复用的效果，比如，Python中有 import，C/C++ 中有 #include。在 Shell 中，我们可以使用 `source` 命令来实现类似的效果。

我之前已经提到了 source 命令，这里我们再来讲解一下。

source 命令的用法为：

```bash
source filename
```

也可以简写为：

```bash
. filename
```

两种写法的效果相同。对于第二种写法，注意点号`.`和文件名中间有一个空格。

source 是 shell 内置命令的一种，它会读取 filename 文件中的代码，并依次执行所有语句。也可以理解为，source 命令会强制执行脚本文件中的全部命令，而忽略脚本文件的权限。

#### 实例

创建两个脚本文件 `func.sh` 和 `main.sh`：`func.sh` 中包含了若干函数，`main.sh` 是主文件，`main.sh` 中会包含 `func.sh`。

`func.sh` 文件内容：

```shell
#计算所有参数的和
function sum(){
	local total=0
	
    for n in $@ 
    do
    	((total+=n))    
    done
    
    echo $total    
    return 0
}
# $@：表示所有脚本参数的内容
```


`main.sh`文件内容：

```shell
#!/bin/bash

source func.sh

echo $(sum 10 20 55 15)
```

运行 main.sh，输出结果为：
100

source 后边可以使用相对路径，也可以使用绝对路径，这里使用的是相对路径。

## 避免重复引入

C/C++ 中的头文件可以避免被重复引入；换句话说，即使被多次引入，效果也相当于一次引入。我们在头文件中进行了特殊处理。

Shell source 命令和 C/C++ 中的 #include 类似，都没有避免重复引入的功能，只要你使用一次 source，它就引入一次脚本文件中的代码。

那么，在 Shell 中究竟该如何避免重复引入呢？

**我们可以在模块中额外设置一个变量，使用 if 语句来检测这个变量是否存在，如果发现这个变量存在，就 return 出去。**

这里需要强调一下 return 关键字。return 在 C++、Java 等大部分编程语言中只能退出函数，除此以外再无他用；但是**在 Shell 中，return 除了可以退出函数，还能退出由 source 命令引入的脚本文件**。

所谓**退出脚本文件，就是在被 source 引入的脚本文件（子文件）中，一旦遇到 return 关键字，后面的代码都不会再执行了，而是回到父脚本文件中继续执行 source 命令后面的代码。**

**return 只能退出由 source 命令引入的脚本文件，对其它引入脚本的方式无效**。

下面我们通过一个实例来演示如何避免脚本文件被重复引入。本例会涉及到两个脚本文件，分别是主文件 `main.sh` 和 模块文件 `module.sh`。

模块文件 `module.sh`：

```shell
if [ -n "$__MODULE_SH__" ]; then
	return
fi
__MODULE_SH__='module.sh'

echo "today is 2020/11/6"

```

注意第一行代码，**一定要使用双引号把`$__MODULE_SH__`包围起来**，具体原因在后面的《Shell test》中讲。

主文件 `main.sh`：

```shell
#!/bin/bash

source module.sh
source module.sh

echo "here executed"
```

`./`表示当前文件，你也可以直接写作`source module.sh`。

运行 `main.sh`，输出结果为：

```bash
today is 2020/11/6
here executed
```

我们在 `main.sh` 中两次引入 `module.sh`，但是只执行了一次，说明第二次引入是无效的。

`main.sh` 中的最后一条 echo 语句产生了输出结果，说明 return 只是退出了子文件，对父文件没有影响。