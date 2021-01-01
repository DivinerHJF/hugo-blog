---
title: "Linux 探索 01 - Bash Script"
date: 2020-03-07T18:22:24+08:00
categories: 江湖百晓生
tags:
    - 技术
    - shell
    - bash
mathjax: false
codes: [bash,]
comments: true
vertical: false
---



# Shell 脚本

Shell 编程跟 JavaScript、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

Linux 的 Shell 种类众多，本教程关注的是 Bash，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。

在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 **#!/bin/sh**，它同样也可以改为 **#!/bin/bash**。

> **#!** 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。

Shell 脚本（shell script），是一种为 shell 编写的脚本程序。业界所说的 shell 通常都是指 shell 脚本，但读者朋友要知道，shell 和 shell script 是两个不同的概念。由于习惯的原因，简洁起见，本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。

```bash
#!/bin/bash
echo "Hello World !"
```

> echo 命令用于向窗口输出文本。







------

# 变量与数据

## Shell 变量

### 定义变量

定义变量时，变量名不加美元符号$，且变量名和等号之间`不能`有空格：

```bash
your_name="runoob.com"
```

除了显式地直接赋值，还可以用语句给变量赋值，如：

```bash
for file in `ls /etc`
或
for file in $(ls /etc)
```

以上语句将 /etc 下目录的文件名循环出来。

#### 变量命名

同时，变量名的命名须遵循如下规则：

- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头
- 中间不能有空格，可以使用下划线（_）
- 不能使用标点符号
- 不能使用 bash 里的关键字（可用 help 命令查看保留关键字）

#### 变量类型

运行shell时，会同时存在三种变量：

- 局部变量

  在脚本或命令中定义，仅在当前 shell 实例中有效，其他 shell 启动的程序不能访问局部变量。

- 环境变量

  所有的程序，包括 shell 启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候 shell 脚本也可以定义环境变量。

- shell 变量

  shell 变量是由 shell 程序设置的特殊变量。shell 变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了 shell 的正常运行。



### 使用变量

使用一个定义过的变量，只要在变量名前面加`美元符号`即可，如：

```bash
your_name="qinjx"
echo $your_name
echo ${your_name}
```

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，推荐给所有变量加上花括号，这是个好的编程习惯。



### 只读变量

使用 `readonly 命令`可以将变量定义为只读变量，只读变量的值不能被改变。

```bash
#!/bin/bash
myUrl="http://www.google.com"
readonly myUrl
```



### 删除变量

使用 `unset 命令`可以删除变量。语法：

```bash
unset variable_name
```

变量被删除后不能再次使用，unset 命令不能删除只读变量。





------

## 数据类型

### 注释

以 `#`开头的行就是注释，会被解释器忽略。多行注释还可以使用以下格式：

```bash
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

EOF 也可以使用其他符号：

```bash
:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```



### 字符串

字符串可以用单引号，也可以用双引号，也可以不用引号。

- 单引号

  ```bash
  str='this is a string'
  ```

  - 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
  - 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

- 双引号

  ```bash
  your_name='runoob'
  str="Hello, I know you are \"$your_name\"! \n"
  echo -e $str
  ```

  - 双引号里可以有变量；
  - 双引号里可以出现转义字符。

#### 拼接字符串

```bash
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```

输出结果为：

```bash
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```



#### 获取字符串长度

```bash
string="abcd"
echo ${#string} #输出 4
```



#### 提取子字符串

```bash
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

> **注意**：第一个字符的索引值为 **0**。



#### 查找子字符串

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)：

```bash
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```

> **注意：** 以上脚本中 **`** 是反引号，而不是单引号 **'**



### 数组

- bash支持一维数组（不支持多维数组），并且没有限定数组的大小；
- 类似于 C 语言，数组元素的下标由 0 开始编号；
- 获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。

#### 定义数组

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```bash
array_name=(value0 value1 value2 value3)
```

还可以单独定义数组的各个分量：

```bash
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

> 可以不使用连续的下标，而且下标的范围没有限制。



#### 读取数组

读取数组元素值的一般格式是：

```bash
valuen=${array_name[n]}
```

使用 **@** 符号可以获取数组中的所有元素，例如：

```bash
echo ${array_name[@]}
```



#### 获取数组的长度

获取数组长度的方法与获取字符串长度的方法相同：

```bash
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```







------

# 命令与函数

## 运算符













------

## 基本命令











------

## 自定义函数





















------

# 流程与文件

## 流程控制











------

## 脚本执行

1. **作为可执行程序**

   将上面的代码保存为 test.sh，并 cd 到相应目录：

   ```bash
   chmod +x ./test.sh  #使脚本具有执行权限
   ./test.sh  #执行脚本
   ```

   注意，一定要写成 **./test.sh**，而不是 **test.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

2. **作为解释器参数**

   这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：

   ```bash
   /bin/sh test.sh
   /bin/php test.php
   ```

   这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。





------

## 文件包含

和其他语言一样，Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。

Shell 文件包含的语法格式如下：

```bash
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
```

### 实例

创建两个 shell 脚本文件。

test1.sh 代码如下：

```bash
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

url="http://www.runoob.com"
```

test2.sh 代码如下：

```bash
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

#使用 . 号来引用test1.sh 文件
. ./test1.sh

# 或者使用以下包含文件代码
# source ./test1.sh

echo "菜鸟教程官网地址：$url"
```

接下来，我们为 test2.sh 添加可执行权限并执行：

```bash
$ chmod +x test2.sh
$ ./test2.sh
菜鸟教程官网地址：http://www.runoob.com
```

> **注：**被包含的文件 test1.sh 不需要可执行权限。





------

## 输入/输出重定向

### 基本命令

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

> 文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）



### 深入讲解

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据；
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据；
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。

如果希望 stderr 重定向到 file，可以这样写：

```bash
$ command 2 > file
```

如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：

```bash
$ command > file 2>&1
# 或者
$ command >> file 2>&1
```

如果希望对 stdin 和 stdout 都重定向，可以这样写：

```bash
$ command < file1 >file2
```

command 命令将 stdin 重定向到 file1，将 stdout 重定向到 file2。



### Here Document

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。

它的基本的形式如下：

```bash
command << delimiter
    document
delimiter
```

它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。

> - 结尾的 delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
> - 开始的 delimiter 前后的空格会被忽略掉。

例如，在命令行中通过 wc -l 命令计算 Here Document 的行数：

```bash
$ wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
EOF
3          # 输出结果为 3 行
```



### /dev/null 文件

如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：

```bash
$ command > /dev/null
```

/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

如果希望屏蔽 stdout 和 stderr，可以这样写：

```bash
$ command > /dev/null 2>&1
```


-----

# 参考文献
- [Shell 教程|菜鸟教程](https://www.runoob.com/linux/linux-shell.html)
