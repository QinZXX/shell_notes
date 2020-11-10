# Shell数组: Shell数组定义以及获取数组元素

和其他编程语言一样，Shell 也支持数组。数组（Array）是若干数据的集合，其中的每一份数据都称为元素（Element）。

Shell 并且没有限制数组的大小，理论上可以存放无限量的数据。Shell 数组元素的下标也是从 0 开始计数。

获取数组中的元素要使用下标`[ ]`，下标可以是一个整数，也可以是一个结果为整数的表达式；当然，下标必须大于等于 0。

遗憾的是，常用的 Bash Shell 只支持一维数组，不支持多维数组。

## Shell 数组的定义

在 Shell 中，用括号`( )`来表示数组，数组元素之间用空格来分隔。由此，定义数组的一般形式为：

```bash
array_name=(ele1  ele2  ele3 ... elen)
```

注意，赋值号`=`两边不能有空格，必须紧挨着数组名和数组元素。

下面是一个定义数组的实例：

```bash
nums=(29 100 13 8 91 44)
```


Shell 是弱类型的，它并不要求所有数组元素的类型必须相同，例如：

```bash
arr=(20 56 "hello")
```

第三个元素就是一个“异类”，前面两个元素都是整数，而第三个元素是字符串。

Shell 数组的长度不是固定的，定义之后还可以增加元素。例如，对于上面的 nums 数组，它的长度是 6，使用下面的代码会在最后增加一个元素，使其长度扩展到 7：

```bash
nums[6]=88
```


此外，也无需逐个元素地给数组赋值，下面的代码就是只给特定元素赋值：

```bash
ages=([3]=24 [5]=19 [10]=12)
```

以上代码就只给第 3、5、10 个元素赋值，所以数组长度是 3。

## 获取数组元素

获取数组元素的值，一般使用下面的格式：

```bash
${array_name[index]}
```

其中，array_name 是数组名，index 是下标。例如：

```bash
n=${nums[2]}
```

表示获取 nums 数组的第二个元素，然后赋值给变量 n。



再如：

```bash
echo ${nums[3]}
```

表示输出 nums 数组的第 3 个元素。



使用`@`或`*`可以获取数组中的所有元素，例如：

```bash
${nums[*]}
${nums[@]}
```

两者都可以得到 nums 数组的所有元素。



完整的演示：

```shell
#!/bin/bash

nums=(29 100 13 8 91 44)
echo ${nums[@]}  #输出所有数组元素
nums[10]=66  #给第10个元素赋值（此时会增加数组长度）
echo ${nums[*]}  #输出所有数组元素
echo ${nums[4]}  #输出第4个元素
```

运行结果：

```bash
29 100 13 8 91 44
29 100 13 8 91 44 66
91
```

## 获取数组长度：数组元素个数

这里的所谓数组长度，指的是数组元素的个数。

利用`@`或`*`，可以将数组扩展成列表，然后使用`#`来获取数组元素的个数，格式如下：

```shell
${#array_name[@]}
${#array_name[*]}
```

其中 `array_name` 表示数组名。两种形式是等价的，选择其一即可。

如果某个元素是字符串，还可以通过指定下标的方式获得该元素的长度，如下所示：

```shell
${#arr[2]}
```

获取 arr 数组的第 2 个元素（假设它是字符串）的长度。

#### 回忆字符串长度的获取

回想一下 Shell 是如何获取字符串长度的？其实和获取数组长度如出一辙，它的格式如下：

```shell
${#string_name}
```

`string_name` 是字符串名。

## 实例演示

下面我们通过实际代码来演示一下如何获取数组长度。

```shell
#!/bin/bash

nums=(29 100 13)
echo ${#nums[*]}

#向数组中添加元素
nums[10]="baidu.com"
echo ${#nums[@]}
echo ${#nums[10]}

#删除数组元素
unset nums[1]
echo ${#nums[*]}
```

运行结果：

```shell
3
4
9
3
```

## shell数组拼接

所谓 Shell 数组拼接（数组合并），就是将两个数组连接成一个数组。

拼接数组的思路是：先利用`@`或`*`，将数组扩展成列表，然后再合并到一起。具体格式如下：

```shell
array_new=(${array1[@]}  ${array2[@]})
array_new=(${array1[*]}  ${array2[*]})
```

两种方式是等价的，选择其一即可。其中，array1 和 array2 是需要拼接的数组，array_new 是拼接后形成的新数组。

下面是完整的演示代码：

```shell
#!/bin/basha

rray1=(23 56)
array2=(99 "hello")
array_new=(${array1[@]} ${array2[*]})

echo ${array_new[@]}  #也可以写作 ${array_new[*]}
```

运行结果：
23 56 99 hello

## 删除数组元素（也可删除整个数组）

在 Shell 中，使用 unset 关键字来删除数组元素，具体格式如下：

```bash
unset array_name[index]
```

其中，array_name 表示数组名，index 表示数组下标。

如果不写下标，而是写成下面的形式：

```bash
unset array_name
```

那么就是删除整个数组，所有元素都会消失。

下面通过具体的代码来演示：

```shell
#!/bin/bash

arr=(23 56 99 "hello")
unset arr[1]
echo ${arr[@]}

unset arr
echo ${arr[*]}
```

运行结果：

```
23 99 hello
 
```

注意最后的空行，它表示什么也没输出，因为数组被删除了，所以输出为空。

## 关联数组

关联数组使用**字符串作为下标**，而不是整数，这样可以做到见名知意。

关联数组也称为“键值对（key-value）”数组，**键（key）也即字符串形式的数组下标，值（value）也即元素值**。

例如，我们可以创建一个叫做 color 的关联数组，并用颜色名字作为下标。

```shell
declare -A color
color["red"]="#ff0000"
color["green"]="#00ff00"
color["blue"]="#0000ff"
```

也可以在定义的同时赋值：

```shell
declare -A color=(["red"]="#ff0000", ["green"]="#00ff00", ["blue"]="#0000ff")
```

不同于普通数组，关联数组必须使用带有`-A`选项的 declare 命令创建。关于 declare 命令的详细用法在下一篇中再详细展开。（declare和 typeset）

### 访问关联数组元素

访问关联数组元素的方式几乎与普通数组相同，具体形式为：

```shell
array_name["index"]
```

例如：

```shell
color["white"]="#ffffff"  # 添加键值对
color["black"]="#000000"
```


加上`$()`即可获取数组元素的值：

```bash
$(array_name["index"])
```

例如：

```bash
echo $(color["white"])
white=$(color["black"])
```

### 获取所有元素的下标和值

使用下面的形式可以获得关联数组的所有元素值：

```bash
${array_name[@]}
${array_name[*]}
```

使用下面的形式可以获取关联数组的所有下标值：

```bash
${!array_name[@]}
${!array_name[*]}
```

### 获取关联数组长度

使用下面的形式可以获得关联数组的长度：

```shell
${#array_name[*]}
${#array_name[@]}
```


关联数组实例演示：

```shell
#!/bin/bash

declare -A color
color["red"]="#ff0000"
color["green"]="#00ff00"
color["blue"]="#0000ff"
color["white"]="#ffffff"
color["black"]="#000000"

#获取所有元素值
for value in ${color[*]}
do    
	echo $value
done
echo "****************"

#获取所有元素下标（键）
for key in ${!color[*]}
do   
	echo $key
done
echo "****************"

#列出所有键值对
for key in ${!color[@]}
do
	echo "${key} -> ${color[$key]}"
done
```

运行结果：

```bash
#ff0000
#0000ff
#ffffff
#000000
#00ff00
****************
red
blue
white
black
green
****************
red -> #ff0000
blue -> #0000ff
white -> #ffffff
black -> #000000
green -> #00ff00
```

