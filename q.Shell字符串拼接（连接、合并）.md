# Shell字符串拼接（连接、合并）

在 Shell 中不需要使用任何运算符，将两个字符串并排放在一起就能实现拼接，非常简单粗暴。请看下面的例子：

```shell
#!/bin/bash

name="Shell"
url="baidu.com/"

str1=$name$url  #中间不能有空格
str2="$name $url"  #如果被双引号包围，那么中间可以有空格
str3=$name": "$url  #中间可以出现别的字符串
str4="$name: $url"  #这样写也可以
str5="${name}Script: ${url}index.html"  #这个时候需要给变量名加上大括号

echo $str1
echo $str2
echo $str3
echo $str4
echo $str5
```

运行结果：

```bash
Shellbaidu.com/
Shell baidu.com/
Shell: baidu.com/
Shell: baidu.com/
ShellScript: baidu.com/index.html
```

对于第 7 行代码，`$name` 和 `$url` 之间之所以不能出现空格，是因为当字符串不被任何一种引号包围时，遇到空格就认为字符串结束了，空格后边的内容会作为其他变量或者命令解析。