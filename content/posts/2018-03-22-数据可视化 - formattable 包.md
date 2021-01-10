---
title: "数据可视化-formattable包"
date: 2018-03-22T14:00:11+08:00
categories: 数据可视化
tags: 
    - 技术
    - R
    - formattable
    - 数据展示
    - 数据可视化
mathjax: false
codes: [r,]
comments: true
vertical: false
---

<center><i>
    该软件包使用预定义的格式化规则更加丰富形象地展示数据，<br />
    在存储原始数据的同时将输出结果用格式化输出，使数据在保留原有属性的基础上更加易读
</i></center>

<br>![formattable.png](https://blog-1255524710.cos.ap-beijing.myqcloud.com/cover/formattable.png)
<!--more-->

## 加载包
这个包的功能很简单，但是却很具创意性，它颠覆了 R 语言数据以及数据表的呈现方式，数据方面提供了百分数、会计技术等多个 R 尚未支持的数据格式；数据表方面支持自定义视觉化元素，如对某一列数据进行字号、颜色、背景、以及图形化处理，整体的版式仍然保留表格的样式，但是已经具有了表和图结合的意味。

首先加载所需的程序包：
```r 安装formattable包
devtools::install_github("renkun-ken/formattable")
install.packages("formattable")
library("formattable")
```

<br />

---
## 格式化数据
该包提供了几个典型的可格式化对象，它们包括：`percent`, `comma`, `currency`, `accounting` 和 `scientific` 。这些对象实质上是具有预定义格式规则和参数的数值型向量。

### percent
```r percent-函数定义：将输出向量自定义为百分比数字格式
percent(x, digits = 2L, format = "f", ...)

## Default S3 method:
percent(x, digits = 2L, format = "f", ...)

## S3 method for class 'character'
percent(x, digits = NA, format = "f", ...)
```

> 参数列表：
> x：数值型向量
> digits：一个整数，用于指示百分比字符串的位数
> format：格式类型，传递给formatC

```r Examples-函数使用
p <- percent(c(0.1, 0.02, 0.03, 0.12))
p

> # 函数保留了保留其数学运算属性
> p + 0.05
[1] 15.00% 7.00%  8.00%  17.00%
> mean(p)
[1] 6.75%

> # formattable格式继承了numeric属性，因而保留了数学运算能力
> class(p)
[1] "formattable" "numeric" 
```


---
### comma
```r comma-函数定义：将输出向量自定义为千位分隔符数字格式
comma(x, digits, format = "f", big.mark = ",", ...)

## Default S3 method:
comma(x, digits = 2L, format = "f", big.mark = ",", ...)

## S3 method for class 'character'
comma(x, digits = max(get_digits(x)), format = "f",
  big.mark = ",", ...)
```

> 参数列表：
> x：数值型向量
> digits：一个整数，用于指示百分比字符串的位数
> format：格式类型，传递给formatC

```r Examples-函数使用
> comma(1000000)
[1] 1,000,000.00
> comma(c(1250000, 225000))
[1] 1,250,000.00 225,000.00  
> comma(c(1250000, 225000), format = "d")
[1] 1,250,000 225,000  
> comma("123,345.123")
[1] 123,345.123
```


---
### currency
```r currency-函数定义：将输出向量自定义为百分比数字格式
currency(x, symbol, digits, format = "f", big.mark = ",", ..., sep = "")

## Default S3 method:
currency(x, symbol = "$", digits = 2L, format = "f",
  big.mark = ",", ..., sep = "")

## S3 method for class 'character'
currency(x, symbol = get_currency_symbol(x),
  digits = max(get_digits(x)), format = "f", big.mark = ",", ...)
```

> 参数列表：
> x：数值型向量
> symbol：货币符号
> digits：一个整数，用于指示百分比字符串的位数
> format：格式类型，传递给formatC
> big.mark：千分隔符
> sep：符号和值之间的分隔符

```r Examples-函数使用
> currency(200000)
[1] $200,000.00
> currency(1200000, "USD", format = "d", sep = " ")
[1] USD 1,200,000
> currency("$ 120,250.50")
[1] $120,250.50
> currency("HK$ 120, 250.50")
[1] HK$120,250.50
```


---
### accounting
```r accounting-函数定义：将输出向量自定义为会计数字格式
accounting(x, digits = 2L, format = "f", big.mark = ",", ...)

## Default S3 method:
accounting(x, digits = 2L, format = "f", big.mark = ",",
  ...)

## S3 method for class 'character'
accounting(x, digits = max(get_digits(x)), format = "f",
  big.mark = ",", ...)
```

> 参数列表：
> x：数值型向量
> digits：一个整数，用于指示小数位数
> format：格式类型，传递给formatC
> big.mark：千分隔符

```r Examples-函数使用
> # 两位小数，同时负值加括号
> balance <- accounting(c(1000, 500, 200, -150, 0, 1200))
> balance
[1] 1,000.00 500.00   200.00   (150.00) 0.00     1,200.00
> accounting(c(1200, -3500, 2600), format = "d")
[1] 1,200   (3,500) 2,600  

> # 函数保留了保留其数学运算属性
> balance + 1000
[1] 2,000.00 1,500.00 1,200.00 850.00   1,000.00 2,200.00
```


---
### scientific
```r scientific-函数定义：
scientific(x, format = c("e", "E"), digits = 4, ...)
```

> 参数列表：
> x：数值型向量
> digits：一个整数，用于指示小数位数
> format：格式类型，传递给formatC

```r Examples-函数使用
> scientific(1253421, digits = 8)
[1] 1.25342100e+06
> scientific(1253421, digits = 8, format = "E")
[1] 1.25342100E+06
```

---
### 复杂数据结构
`formattable()` 将高度可定制的格式应用于各种类的对象，如 `numeric`, `logical`, `factor`, `Date`, `data.frame` 等。例如，数据框可能也可存储格式化的列向量（这是自然地，因为数据框就是由若干个等长的向量组成的）：

```r 数据框数据格式化
> df <- data.frame(
+   id = c(1, 2, 3, 4, 5), 
+   name = c("A1", "A2", "B1", "B2", "C1"),
+   balance = accounting(c(52500, 36150, 25000, 18300, 7600), format = "d"),
+   growth = percent(c(0.3, 0.3, 0.1, 0.15, 0.15), format = "d"),
+   ready = formattable(c(TRUE, TRUE, FALSE, FALSE, TRUE), "yes", "no"))
> df
  id name balance growth ready
1  1   A1  52,500    30%   yes
2  2   A2  36,150    30%   yes
3  3   B1  25,000    10%    no
4  4   B2  18,300    15%    no
5  5   C1   7,600    15%   yes
```

<br />

---


## 格式化数据表
### 举个栗子
普通的表格数据如下所示：
```r 普通表格
> df <- data.frame(
+   id = 1:10,
+   name = c("Bob", "Ashley", "James", "David", "Jenny", 
+            "Hans", "Leo", "John", "Emily", "Lee"), 
+   age = c(28, 27, 30, 28, 29, 29, 27, 27, 31, 30),
+   grade = c("C", "A", "A", "C", "B", "B", "B", "A", "C", "C"),
+   test1_score = c(8.9, 9.5, 9.6, 8.9, 9.1, 9.3, 9.3, 9.9, 8.5, 8.6),
+   test2_score = c(9.1, 9.1, 9.2, 9.1, 8.9, 8.5, 9.2, 9.3, 9.1, 8.8),
+   final_score = c(9, 9.3, 9.4, 9, 9, 8.9, 9.25, 9.6, 8.8, 8.7),
+   registered = c(TRUE, FALSE, TRUE, FALSE, TRUE, TRUE, TRUE, FALSE, FALSE, FALSE),
+   stringsAsFactors = FALSE)
> df
   id   name age grade test1_score test2_score final_score registered
1   1    Bob  28     C         8.9         9.1        9.00       TRUE
2   2 Ashley  27     A         9.5         9.1        9.30      FALSE
3   3  James  30     A         9.6         9.2        9.40       TRUE
4   4  David  28     C         8.9         9.1        9.00      FALSE
5   5  Jenny  29     B         9.1         8.9        9.00       TRUE
6   6   Hans  29     B         9.3         8.5        8.90       TRUE
7   7    Leo  27     B         9.3         9.2        9.25       TRUE
8   8   John  27     A         9.9         9.3        9.60      FALSE
9   9  Emily  31     C         8.5         9.1        8.80      FALSE
10 10    Lee  30     C         8.6         8.8        8.70      FALSE
```

格式化表格，具有以下可视化效果：
```r 格式化表格
> formattable(df, 
+             list(
+               age = color_tile("white", "orange"),
+               grade = formatter(
+                 "span", 
+                 style = x ~ ifelse(x == "A", style(color = "green", font.weight = "bold"), NA)
+               ),
+               area(col = c(test1_score, test2_score)) ~ normalize_bar("pink", 0.2),
+               final_score = formatter(
+                 "span",
+                 style = x ~ style(color = ifelse(rank(-x) <= 3, "green", "gray")),
+                 x ~ sprintf("%.2f (rank: %02d)", x, rank(-x))
+               ),
+               registered = formatter(
+                 "span",
+                 style = x ~ style(color = ifelse(x, "green", "red")),
+                 x ~ icontext(ifelse(x, "ok", "remove"), ifelse(x, "Yes", "No"))
+               )
+             )
+ )
```

![formattable.png](https://blog-1255524710.cos.ap-beijing.myqcloud.com/cover/formattable.png)

表格中使用的图标集由[GLYPHICONS.com](GLYPHICONS.com)提供并包含在[Bootstrap](https://getbootstrap.com/docs/3.3/components/)中。

注意到一共用了 `文字格式自定义、文字背景自定义、文本自定义` 三种自定义可视化类型：
> color_tile函数用于输出按照数值量级进行颜色背景填充的列。
>
> formatter函数提供字体显示格式的自定义，grade列自定义了值为A的记录显示绿色，并将字体加粗，否则忽略。
> test1_score, test2_score两列通过area函数在对应字体背景位置使用条形图来代表指标量级大小，颜色填充粉色。
> final_score列对指标按照top3显示绿色，其余显示灰色，同时将内容显示格式自定义为浮点型+(rank:名次)进行显示。
> registered列则在对填充颜色按照对应布尔值进行显示（TRUE显示绿色、FALSE显示红色）之外，在左侧添加了对用的icon文本（TRUE显示绿色对号，FALSE显示红色叉号）。


---
### 文字格式自定义
#### color_text
```r eg-color_text("color1", "color2")：输出按照数值量级进行字体颜色填充的列
formattable(mtcars, list(mpg = color_text("black", "red")))
```

#### formatter
```r formatter-函数定义：创建一个HTML元素制作的格式化函数
formatter(.tag, style = ...)
```

> 参数列表：
> .tag：HTML 标签，默认为 span
> style：CSS语句
> 注：类似 x ~ expr 的公式将表现得像 function(x) expr

诸多CSS样式于此可见：[List of CSS properties](https://www.w3.org/Style/CSS/all-properties)


### 文字背景自定义
#### 色块函数
```r 色块函数
# color_tile("color1", "color2")
# 输出按照数值量级进行颜色背景填充的列
formattable(mtcars, list(mpg = color_tile("white", "pink")))

# color_bar(color = "lightgray", fun = "proportion", ...)
# 输出有颜色填充的，大小以数值量级为比例的色块
formattable(mtcars, list(mpg = color_bar("lightgray", proportion)))

# normalize_bar(color = "lightgray", ...)
# proportion_bar(color = "lightgray", ...)
# 按正常配置输出大小以数值量级为比例的色块
formattable(mtcars, list(mpg = normalize_bar()))
formattable(mtcars, list(mpg = proportion_bar()))
```

#### area
```r area-函数定义：选取特定表格区域进行格式化
area(row, col) ~ formatter/normalize_bar……
```

> 参数列表：
> row/col：选取的行与列，默认为全选

### 文本自定义
#### icontext
```r icontext-函数定义：文本修饰函数-添加图标、更改内容
icontext(icon, text = list(NULL), ...)
```

> 参数列表：
> icon：图标名称的字符矢量或字符矢量列表
> text：文本的字符向量

使用的图标集由[GLYPHICONS.com](GLYPHICONS.com)提供并包含在[Bootstrap](https://getbootstrap.com/docs/3.3/components/)中。

<br />

---
## 致谢

> ### 参考文章
> * [Github：任坤的formattable项目](https://github.com/renkun-ken/formattable)
> * [杜雨：一款脑洞大开的表格可视化神器](https://mp.weixin.qq.com/s/QAr7LYx4jgvOlNBVAUZX5g)