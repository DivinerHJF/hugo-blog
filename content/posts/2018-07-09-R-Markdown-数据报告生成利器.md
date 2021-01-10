---
title: "R Markdown: 数据报告生成利器"
date: 2018-07-09T22:44:09+08:00
categories: 数据报告单
tags:
    - 技术
    - R Markdown
    - 数据报告
    - 可重复性
    - 可视化
mathjax: true
codes: [r,]
comments: true
vertical: false
---

<center>
    <i>   
        R Markdown 站在 knitr 和 Pandoc 的肩膀上，前者执行嵌入于文档中的计算机代码，并将 R Markdown 转换为 Markdown；后者将 Markdown 呈现出您想要的输出格式（如 PDF、HTML、Word 等）
    </i>
</center>

<br>![R Markdown](https://blog-1255524710.cos.ap-beijing.myqcloud.com/cover/hex-rmarkdown.png)
<!--more-->

此篇文章翻译自谢益辉新书 [《R Markdown: The Definitive Guide》](https://bookdown.org/yihui/rmarkdown/) 的前三章节，内容有所删减，主要介绍了 R Markdown 的相关结构及语法规则，如果想了解更多更详细的内容推荐您阅读原书。

# 安装

这里假设您已经安装了 R ([https://www.r-project.org](https://www.r-project.org/))  和 RStudio IDE ([https://www.rstudio.com](https://www.rstudio.com/)) 。Rstudio 并不是必须的，但安装它会使您更加容易地使用 R Markdown。如果您没有安装 RStudio IDE，您将不得不安装 Pandoc（ [http://pandoc.org](http://pandoc.org) ），否则就不需要单独安装 Pandoc，因为在安装 RStudio 时已经将它捆绑安装了。接下来，您可以在 R 中安装 `rmarkdown` 包： 

```R 安装 rmarkdown 包的两种方式
# Install from CRAN
install.packages('rmarkdown')

# Or if you want to test the development version,
# install from GitHub
if (!requireNamespace("devtools"))
  install.packages('devtools')
devtools::install_github('rstudio/rmarkdown')
```

如果您想要生成 PDF 类型文档输出，您将需要安装 LaTeX 。对于那些以前没有安装过 LaTeX 的 R Markdown 用户，我们建议您安装 TinyTeX（https://yihui./tinytex/）： 

```R 安装 tinytex 包
install.packages("tinytex")
tinytex::install_tinytex()  # install TinyTeX
```

TinyTeX 相当于一款轻量级、跨平台、易于维护的 LaTeX 。在将 LaTeX  或 R Markdown 文档编译成 PDF 时，`tinytex` 可以帮助您自动安装所需的相关 R 包，同时还能确保一个 LaTeX  文档被编译成正确的次数，以解决所有的交叉引用问题。如果您不明白这两件事是什么意思，应该按照我们的建议来安装 TinyTeX，因为这些细节往往并不值得您花费时间和精力去关心。 

使用 `rmarkdown` 包、RStudio/Pandoc 和 LaTeX，您应该能够编译大多数R Markdown 文档。在某些情况下，您可能需要其他软件包，我们将在必要时提到它们。 



> **参考文献**
>
> - R Core Team. 2018. *R: A Language and Environment for Statistical Computing*. Vienna, Austria: R Foundation for Statistical Computing. [https://www.R-project.org/](https://www.r-project.org/).
> - Xie, Yihui. 2018f. *Tinytex: Helper Functions to Install and Maintain Tex Live, and Compile Latex Documents*. <https://github.com/yihui/tinytex>.





----



# 基础知识

R Markdown 为数据科学提供了一个创作框架。R Markdown 能胜任以下两个任务：

- 保存并执行代码；
- 生成可共享的高质量报告。

R Markdown 的设计初衷是为了更容易地实现报告内容的可重复性，这是因为计算代码和叙述都在同一个文档中，结果是由源代码自动生成的。并且 R Markdown 支持数十种静态和动态/交互式输出格式。 

如果您更喜欢观看视频进行学习，我们建议您查看网站 [https://rmarkdown.rstudio.com](https://rmarkdown.rstudio.com)，并在 “Get Started ” 中观看视频，其中包括了 R Markdown 的基础知识。 



## 文档结构

下面是一份非常简易的 R Markdown 文档，是一个带有 `.Rmd` 拓展名的纯文本文档。

```yaml YAML 元数据
---
title: "Hello R Markdown"
author: "Awesome Me"
date: "2018-02-14"
output: html_document
---
```

~~~markdown 主体内容
This is a paragraph in an R Markdown document.

Below is a code chunk:

```{r}
fit = lm(dist ~ speed, data = cars)
b   = coef(fit)
plot(cars)
abline(fit)
```

The slope of the regression is `r b[1]`.
```
~~~

您可以使用任何文本编辑器（包括但不限于 RStudio）来创建这样的文本文档。如果使用 RStudio，您可以从 `File -> New File -> R Markdown ` 中创建一个新的 Rmd 文档。 

一份 R Markdown 文档有三个基础组成部分：元数据，文本和代码。元数据是在三个连接符 `---` 之间的内容。元数据的语法是 YAML（ [YAML 不是标记语言](https://en.wikipedia.org/wiki/YAML) ），所以有时它也被称为 YAML metadata 或 YAML frontmatter。 需要注意的是，缩进在 YAML 中十分重要，忽视它会让你付出惨重代价。请参阅谢益辉所写的 《bookdown》（2016）一书中的 [附录b.2](https://bookdown.org/yihui/bookdown/r-markdown.html) 来了解一些简单的例子，这些示例展示了 YAML 语法。

文档的主体遵循元数据书写的规则。文本的语法是 Markdown，将在第 2.5 节中进行介绍。有两种类型的计算机代码，在第 2.6 节中进行了详细解释： 

- **代码块**：以三个重音符及所使用语言开始，其中 `r` 代表所使用的程序语言，并以三个重音符结束。 可以在花括号中填写块选项（如：将图形高度设置为5英寸：`{r, fig.height=5}`）。
- **内联代码**：以 ``r` 开始，并以单个重音符结束。



## 文档编译

最简单的方式莫过于在 RStudio 中单击 `Knit` 按钮，对应的快捷键为 `Ctrl + Shift + K` （在 macOS 中为 `Cmd + Shift + K`）。当然也可以直接运行代码 `rmarkdown::render` 来进行渲染编译，如：

```R 文档渲染编译
rmarkdown::render('foo.Rmd', 'pdf_document')
```

当编译多个文档时，使用函数更加方便，因为可以直接使用循环来进行渲染编译。



## 参考卡片

RStudio 已经创建了大量的参考卡片，它们可以在 https://www.rstudio.com/resources/cheatsheets/ 上免费获得。



## 输出格式

有两种输出格式：documents 和 presentations 。所有可用的格式如下所示 ：

- `beamer_presentation`
- `github_document`
- `html_document`
- `ioslides_presentation`
- `latex_document`
- `md_document`
- `odt_document`
- `pdf_document`
- `powerpoint_presentation`
- `rtf_document`
- `slidy_presentation`
- `word_document`

我们将在第 3 章和第 4 章详细地记录这些输出格式。在其他扩展包中提供了更多的输出格式（从第 5 章开始）。对于 Rmd 文件的 YAML 元数据中的输出格式名称，如果格式来自扩展包，您需要包含包名（若格式来自于 `rmarkdown` 包，则不需要 `rmarkdown::`前缀 ），例如：

```yaml 输出格式设置
output: tufte::tufte_html
```

每种输出格式通常都有多种格式选项。所有这些选项都记录在 R 包 help 页面上。例如，您可以在 R 中键入 `?rmarkdown::` 打开关于 `html_document` 格式的 help 页面。当您想要使用某些选项时，必须将这些值从 R 转换成 YAML，例如：

```R ?rmarkdown::
html_document(toc = TRUE, toc_depth = 2, dev = 'svg')
```

可以用 YAML 写为： 

```yaml 输出格式参数设置
output:
  html_document:
    toc: true
    toc_depth: 2
    dev: 'svg'
```

YAML 中的字符串通常不需要引号（``dev：“svg”` 和 `dev:svg` 是相同的），除非它们包含特殊字符，比如冒号 `：`。如果您不确定是否应该引用字符串，那么用 `yaml` 包来测试它，例如：

```R YAML 语法
cat(yaml::as.yaml(list(
  title = 'A Wonderful Day',
  subtitle = 'hygge: a quality of coziness'
)))

## title: A Wonderful Day
## subtitle: 'hygge: a quality of coziness'
```

注意，上面例子中的副标题是由于冒号而引用单引号的。

 

如果某一选项有子选项（这意味着该选项的值是 R 中的列表），则子选项需要进一步缩进，例如：

```yaml 子选项-缩进
output:
  html_document:
    toc: true
    includes:
      in_header: header.html
      before_body: before.html
```

一些选项将被传递给 **knitr** ，比如 `dev`、`fig_width` 和 `fig_height`。这些选项的详细文档可以在 [knitr 文档页面](<https://yihui.name/knitr/options/> ) 上找到。请注意，实际的 knitr 选项名称可能有所不同。特别是，knitr 在名称中使用 `.`，但 rmarkdown 使用 `_`，例如，在 rmarkdown 中，`fig_width`  对应于knitr 中的 `fig.width` 。

一些选项将被传递给 **Pandoc**，比如 `toc`、`toc_depth` 和 `number_sections` 。当有疑问时，您应该参考 Pandoc 文档。R Markdown 输出格式函数通常有一个`pandoc_args`  参数，它应该是传递给 Pandoc 的参数的字符向量。如果您发现任何没有由输出格式参数表示的 Pandoc 特性，您可以使用这个终极论证，例如：

```yaml pandoc_args 参数
output:
  pdf_document:
    toc: true
    pandoc_args: ["--wrap=none", "--top-level-division=chapter"]
```



## Markdown 语法

### 内联格式

- *斜体* ：`_text_` 或 `*text*` 

- **粗体** ：`**text** `

- 下标 ：`H~3~PO~4~`  渲染为 $H_3PO_4$ 

- 上标 ：`Cu^2+^`  渲染为 $Cu^{2+}$ 

- 脚注 ： `^[This is a footnote.]` 

- 内联代码 ：`` `code` `` , 可以使用 n+1 个重音符输出包含 n 个重音符的代码块。

- 超链接 ：`[text](link) ` 

- 图片链接 ： `![alt text or image title](path/to/image)`

- 引用 ： 

  ```ruby 书籍引用
  @Manual{R-base,
    title = {R: A Language and Environment for Statistical
      Computing},
    author = {{R Core Team}},
    organization = {R Foundation for Statistical Computing},
    address = {Vienna, Austria},
    year = {2017},
    url = {https://www.R-project.org/},
  }
  ```



### 块级元素

- **标题**

  ```Markdown 三级标题的书写
  # First-level header
  
  ## Second-level header
  
  ### Third-level header
  ```

  如果不想让某个标题被编号，可以在标题后面添加 `{-}` 或者 `{.unnumbered}`，如：

  ```Markdown 标题不编号
  # Preface {-} 
  ```

- **无序列表**

  ```Markdown 无序列表
  - one item
  - one item
  - one item
    - one more item
    - one more item
    - one more item
  ```

- **有序列表**

  ```Markdown 有序列表
  1. the first item
  2. the second item
  3. the third item
    - one unordered item
    - one unordered item
  ```

- **引用**

  ```Markdown 名言引用
  > "I thoroughly disapprove of duels. If a man should challenge me,
    I would take him kindly and forgivingly by the hand and lead him
    to a quiet place and kill him."
  >
  >                                                 --- Mark Twain
  ```

  > "I thoroughly disapprove of duels. If a man should challenge me,
  > I would take him kindly and forgivingly by the hand and lead him
  > to a quiet place and kill him."
  >
  > ​                                               --- Mark Twain

- **代码块**

  ~~~Markdown 代码块
  ```
  This text is displayed verbatim / preformatted
  ```
  
  Or indent by four spaces:
  
      This text is displayed verbatim / preformatted
  ~~~



### 数学表达式

`$f(k) = {n \choose k} p^{k} (1-p)^{n-k}$ ` :  $f(k) = {n \choose k} p^{k} (1-p)^{n-k}$ 

`$$f(k) = {n \choose k} p^{k} (1-p)^{n-k}$$ ` : 

$$
f(k) = {n \choose k} p^{k} (1-p)^{n-k}
$$



---



## 代码块选项

单击 `Insert` 按钮，对应快捷键为 `Ctrl + Alt + I` （macOS：`Cmd + Option + I` ）。


在 <https://yihui.name/knitr/options> 中有大量的代码块可选项，在此我们列出常用的一部分：

- `eval=TRUE` ：执行当前代码块；

  ~~~R eval 巧用
  ```{r}
  # execute code if the date is later than a specified day
  do_it = Sys.Date() > '2018-02-14'
  ```
  
  ```{r, eval=do_it}
  x = rnorm(100)
  ```
  ~~~

- `echo=TRUE` ：输出源代码；

- `result` ：当设置为 `'hide'` ，文本输出将被隐藏；当设置为 `'asis'` ，文本输出将被 “原样” 书写。

- `collapse=TRUE` ：将文本输出和源代码合并为单个代码块输出，更加紧凑；

- `warning`, `message`,  `error` ：是否在输出文档中显示警告、消息和错误；

- `include=FALSE` ：运行当前代码并且不显示任何源代码与输出结果；

- `cache` ：是否启用高速缓存。如果启用了缓存，则在下一次编译文档时不会对相同的代码块进行评估（如果代码块没有被修改），这将节省您的时间；

- `fig.width`，`fig.height` ：（图形设备）块的大小（英寸）。注意：`fig.dim = c(6, 4)` 意味着 `fig.width = 6` 并且 `fig.height = 4`；

- `out.width`， `out.height` ：输出文档中 R 图片的输出大小。可以使用百分比，例如 `out.width = '80%'` 表示页面宽度的 80%； 

- `fig.align` ：图片的对齐方式；

- `dev` ：图形设备保存 R 图片的格式。如 `'pdf'`, `'png'`, `'svg'`, `'jpeg'`；

- `fig.cap` ：图片标题；

- `child` ：您可以在主文档中包含子文档。这个选项选择一条通向外部文件的路径。 



如果某个选项需要经常被设置为多个代码块中的值，您可以考虑在文档的第一个代码块中全局设置它：

~~~R 全局设置
```{r, setup, include=FALSE}
knitr::opts_chunk$set(fig.width = 8, collapse = TRUE)
```
~~~



### 图片

本地图片亦可以使用代码块选项进行调节，例如：

~~~R 本地图片选项
```{r, out.width='25%', fig.align='center', fig.cap='...'}
knitr::include_graphics('images/hex-rmarkdown.png')
```
~~~

如果您想要淡入一个不是由 R 代码生成的图形，您可以使用 `knitr::include_graphics()`  函数，它使您能够更好地控制图像的属性，而不是像 `![alt text or image title](path/to/image)` 这样的 Markdown 语法难以调解图片属性。 



### 表格

使用 `knitr::kable()`  函数可以简易的创建表格，表格标题可以通过 `caption`  来设置，例如：

~~~R 表格选项
```{r tables-mtcars}
knitr::kable(iris[1:5, ], caption = 'A caption')
```
~~~

如果您正在寻找更高级的表格样式控制，建议您使用 [kableExtra](https://cran.r-project.org/package=kableExtra) 包，它提供了定制 PDF 和 HTML 表格外观的功能。在第 12.3 节中解释了 `bookdown` 包如何扩展 rmarkdown 的功能，以允许在文本中轻松地交叉引用数字和表格。 



> **参考文献**
>
> - Xie, Yihui. 2015. *Dynamic Documents with R and Knitr*. 2nd ed. Boca Raton, Florida: Chapman; Hall/CRC. <https://yihui.name/knitr/>. 





---



# 输出文档

## HTML 文档

为了输出 HTML 文档，首先要在 YAML 元数据中写入 `output: html_document` ：

```yaml
---
title: Habits
author: John Doe
date: March 22, 2005
output: html_document
---
```



### 目录（Table of contents）

```yaml 目录可选项
---
title: "Habits"
output:
  html_document:
    toc: true
    toc_depth: 2
    toc_float:
        collapsed: false
        smooth_scroll: false
---
```

- `toc: true` ：输出目录；
- `toc_depth` ：所输出标题的最小级别；
- `toc_float: true` ：目录悬停于内容左侧，并一直可见；
- `collapsed` (默认为 `TRUE`)  ：初始只显示顶级标题，随内容滚动目录逐级展开；
- `smooth_scroll` (默认为 `TRUE`) ：点击目录标题是否导航到指定内容。



### 目录编号 (Section numbering) 

```yaml 目录编号
---
title: "Habits"
output:
  html_document:
    toc: true
    number_sections: true
---
```

注意，如果文档中没有一级标题，那么二级标题将被命名为 `0.1`, `0.2` ……



### 选项卡 (Tabbed sections)

```markdown 选项卡
## Quarterly Results {.tabset .tabset-fade .tabset-pills}

### By Product

(tab content)

### By Region

(tab content)
```


![Tabbed sections](https://bookdown.org/yihui/rmarkdown/images/tabset.png)


- `.tabset`  ：使主标题的所有子标题与 .tabset 属性一起出现在选项卡中，而不是作为独立的部分；
- `.tabset-fade`  ：选项卡切换时淡入淡出；
- `.tabset-pills`  ：改变选项卡外观，使其类似 “药丸”。



### 外观与风格

```yaml 主题与高亮
---
title: "Habits"
output:
  html_document:
    theme: united
    highlight: tango
---
```

- `theme`  ：主题是从 [Bootswatch](https://bootswatch.com/3/) 主题库中提取的，适用的主题包括：`default`, `cerulean`, `journal`, `flatly`, `readable`, `spacelab`, `united`, `cosmo`, `lumen`, `paper`, `sandstone`, `simplex`, 和 `yeti`. 
- `highlight` ：代码高亮模式。支持的风格包括： `default`,  `tango`,  `pygments`,  `kate`,  `monochrome`,  `espresso`,  `zenburn`,  `haddock` 和 `textmate`.  



### 图片选项

```yaml 图片选项
---
title: "Habits"
output:
  html_document:
    fig_width: 7
    fig_height: 6
    fig_caption: true
---
```

- `fig_width` 和 `fig_height`  ：图片宽度和高度；
- `fig_caption`  ：控制图片是否包括标题；
- `dev`  ：图片渲染格式，默认为 `png`。



### 表格打印


 默认表格输出格式为：

| Option  | Description                                                |
| :-----: | :--------------------------------------------------------- |
| default | Call the `print.data.frame` generic method                 |
|  kable  | Use the `knitr::kable` function                            |
| tibble  | Use the `tibble::print.tbl_df` function                    |
|  paged  | Use `rmarkdown::print.paged_df` to create a pageable table |

设定为 paged 格式后输出形式为：

```yaml 表格设置
---
title: "Motor Trend Car Road Tests"
output:
  html_document:
    df_print: paged
---

​``` {r, rows.print=5}
mtcars
​``` 
```


![mtcars](https://bookdown.org/yihui/rmarkdown/images/paged.png)


TABLE 3.2: The options for paged HTML tables. 

| Option         | Description                                           |
| -------------- | ----------------------------------------------------- |
| max.print      | The number of rows to print.                          |
| max.print      | The number of rows to print.                          |
| cols.print     | The number of columns to display.                     |
| cols.min.print | The minimum number of columns to display.             |
| pages.print    | The number of pages to display under page navigation. |
| pages.print    | The number of pages to display under page navigation. |
| rownames.print | When set to `FALSE` turns off row names.              |



### 代码折叠

```yaml 代码隐藏
---
title: "Habits"
output:
  html_document:
    code_folding: hide
---
```

- `code_folding: hide` ：初始默认不显示代码，查看者可点击进行显示；
- `code_folding: show` ：初始默认显示代码，查看者可点击进行隐藏；



### 高级定制

#### 保留 Markdown 文件

当运行一个 R Markdown 文件（`*.Rmd`）时，将创造一个 Markdown 文件（`*.md`）并将该文件通过 Pandoc 转换为 HTML 文件。如果想要保留 Markdown 文件，可以使用 `keep_md` 选项：

```yaml 保留 .md 文档
---
title: "Habits"
output:
  html_document:
    keep_md: true
---
```



#### 添加本地 HTML 文档

可以通过添加额外的 HTML 内容或完全替换核心 Pandoc 模板来完成更高级的输出定制。为了在文档头部或文档主体之前/之后包含内容，您可以使用以下选项： 

```yaml 添加 HTML 文档
---
title: "Habits"
output:
  html_document:
    includes:
      in_header: header.html
      before_body: doc_prefix.html
      after_body: doc_suffix.html
---
```



#### 自定义模板

```yaml 自定义模板
---
title: "Habits"
output:
  html_document:
    template: quarterly_report.html
---
```

有关模板的其他详细信息，请参考 [Pandoc 模板](http://pandoc.org/MANUAL.html#templates) 上的文档。您还可以研究默认的 HTML 模板 [`default.html5`](https://github.com/jgm/pandoc-templates/) 。



其他类型文档格式控制方式类似，如欲详细了解请 [参考原作](https://bookdown.org/yihui/rmarkdown/documents.html) 。


---
