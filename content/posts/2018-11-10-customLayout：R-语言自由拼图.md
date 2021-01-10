---
title: "customLayout：R 语言自由拼图"
date: 2018-11-10T10:51:49+08:00
categories: 数据可视化
tags: 
    - 技术
    - R
    - customLayout
    - 绘图
    - 图片布局
    - PPT
mathjax: false
codes: [r,]
comments: true
vertical: false
---

<center>
    <i>
    	customLayout 包是一个图表对象布局工具，它可以让我们用更加简单易解的操作命令来完成繁琐的图片布局设置。<br />如果你常常为了一页多图的繁琐布局设置而苦恼，那么这个包绝对值得好好学习。
    </i>
</center>

<br />![customLayout.png](https://blog-1255524710.cos.ap-beijing.myqcloud.com/cover/customLayout.png)
<!--more-->

由图可以看到 `customLayout` 包的所有函数以及这个包的整体框架，简单来讲 `customLayout` 包就是一个图表对象布局工具，它可以让我们用更加简单易解的操作命令来完成繁琐的图片布局设置。令人惊喜的是，对于 `R` 中的 `base` 绘图对象以及 `grid` 绘图对象它都能很好地支持，同时它还支持 `PowerPoint` 对象版式的设计。

如果你常常为了一页多图的繁琐布局设置而苦恼，那么这个包绝对值得好好学习，它的学习成本很低——十分钟你就能很好的掌握，但你能得到的效益超乎想象——它能节省你大量的时间。

接下来我们就将按照封面图的流程来分别讲述每个函数的作用，我们将首先讲解图片版式，然后是 PPT 版式，最后还会提及查看当前配置的相关函数。



## 创建并组合布局

### 创建：lay_new()

这个函数是后续所有命令的基础，所有的绘图命令都将以它开始，它的主要任务是创建一个 `custom layout` 对象，后续的拼接、嵌套、设置的对象都是基于这种 `custom layout` 对象。

```r lay_new-函数定义：
lay_new(mat, widths = NULL, heights = NULL)
```

> **参数列表：**
> mat：矩阵-指定每个图形的位置
> widths：向量-指定 `mat` 中每列所代表的相对宽度
> heights：向量-指定 `mat` 中每行所代表的相对高度


{% label info@<b>举例阐释</b> %}
```r eg-创建
library(customLayout)
lay <- lay_new(matrix(1:4, nc = 2),
```

![lay_new.png](https://blog-1255524710.cos.ap-beijing.myqcloud.com/images/lay_new.png)


### 拼接：lay_bind_row()



### 嵌套：lay_split_field()



---


## 预览当前布局

### 展示：lay_show()



---


## 图片版式

### 基础图层：lay_set()

{% label info@<b>举例阐释</b> %}
```r eg-创建
library(customLayout)
set.seed(123)
par(mar = c(3, 2, 2, 1))

# Prepare layout
lay <- lay_new(matrix(1:4, nc = 2),
widths = c(3, 2),
heights = c(2, 1))
lay2 <- lay_new(matrix(1:3))
cl <- lay_bind_col(lay, lay2, widths = c(3, 1))
lay_set(cl) # initialize drawing area

# add plots
plot(1:100 + rnorm(100))
plot(rnorm(100), type = "l")
hist(rnorm(500))
acf(rnorm(100))
pie(c(3, 4, 6), col = 2:4)
pie(c(3, 2, 7), col = 2:4 + 3)
pie(c(5, 4, 2), col = 2:4 + 6)
```

![]()

### 网格图层：lay_grid()



---


## PPT 版式



### 内容设计



### 填充内容



---


## 查看当前配置

### 图片：print.CustomLayout()



### PPT：print.OfficerCustomLayout()


---

## 致谢


> ### 参考文章
> * [customLayout 包官方文档](https://cran.r-project.org/web/packages/customLayout/customLayout.pdf)