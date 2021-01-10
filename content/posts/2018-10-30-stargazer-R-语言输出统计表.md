---
title: "stargazer：R 语言输出统计表"
date: 2018-10-30T16:20:03+08:00
categories: 编程面面观
tags: 
    - 技术
    - R
    - stargazer
    - 可视化
    - 统计表
mathjax: false
codes: [r,]
comments: true
vertical: false
---

<center>
    <i>
    	使用 stargazer 可以将 R 构建的模型结果以 LATEX 、 HTML 和 ASCII 格式输出，方便我们生成标准格式的表格<br />
    	结合 rmarkdown 来进行使用，会使我们优雅地写出一篇拥有期刊级统计表的文章
    </i>
</center>

<br>![table.png](https://blog-1255524710.cos.ap-beijing.myqcloud.com/cover/table.png)

<!--more-->



## 简介

R 包 `stargazer` 可以将 **数据统计汇总** （格式可以为数据框、向量和矩阵等）和 **统计模型结果** 输出为标准统计表格式的 `LATEX` 、`HTML` 和 `ASCII` 格式的字符文本，***将其复制到对应的软件中*** 即可生成标准的统计表，当然也可以配合 `rmarkdown` 使用直接渲染输出为表格，更加方便直接。



## 安装及加载

可以使用常规方法导入 `stargazer` 包：

```R 安装 stargazer 包
install.packages("stargazer")
library(stargazer)
```

<div class="note info"><p>
stargazer 包的输出结果是相应格式的，例如输出 LATEX 格式，可以直接将结果粘贴进在线编辑器 [Overleaf](https://www.overleaf.com) 中输出表格。下文直接将结果以对应表格的形式展示。
</p></div>


---


## 数据统计汇总


### 统计汇总数据

如果要展示数据集的基本描述性分析数据（由 R 函数 `summary` 得到），可以使用以下命令直接得到：

```R 展示 attitude 数据集基本汇总数据
stargazer(attitude)
```

![统计汇总数据](http://static.datartisan.com/upload/attachment/2016/12/qCOW9qcQ.png)


### 原始数据展示

如果想输出某些数据框的特定行的原始内容，需要指定要查看的数据框的一部分，并将设置参数 `summary = FALSE`, 如下所示：

```R 展示 attitude 数据集前四行
data("attitude")
stargazer(attitude[1:4,], summary = FALSE, rownames = TRUE)
```

![展示数据集](http://static.datartisan.com/upload/attachment/2016/12/O5nnDnBH.png)

可以看到，`attitude` 数据集中包括 `rating`、`complaints` 等多个变量，数据展示形式为 **三线表** 。


### 列联表

`stargazer ` 也可以用来展示向量、矩阵或者数据框的内容。在这里我们建立了 `attitude` 数据集中变量 `rating`、`complaints`、`privileges` 的相关系数矩阵，并予以展示：

```R 输出列联表
correlation.matrix <- cor(attitude[,c("rating", "complaints", "privileges")])
stargazer(correlation.matrix, title = "Correlation Matrix")
```

![矩阵展示](http://static.datartisan.com/upload/attachment/2016/12/cC2cR66K.png)



---



## 统计模型结果

### 回归表

在 R 中可以很方便的使用 `lm()` 和 `glm()` 函数来构建回归模型，我们同样可以在同一张表中对这些模型进行比较，参数 `title` 用来设定表的标题，参数 `align` 使每列中的系数沿小数点对齐：

```R 输出回归表
## 构建两个线性回归模型
linear.1 <- lm(rating ~ complaints + privileges + learning + raises + critical,
data = attitude)
linear.2 <- lm(rating ~ complaints + privileges + learning, data = attitude)
## 构建一个 probit 模型
attitude$high.rating <- (attitude$rating > 70)
probit.model <- glm(high.rating ~ learning + critical + advance, data = attitude,
family = binomial(link = "probit"))

stargazer(linear.1, linear.2, probit.model, title = "Results", align = TRUE)
```

![回归表](http://static.datartisan.com/upload/attachment/2016/12/Cyqs7adZ.png)



#### 回归表的修饰

为了使表格更加标准，我们还可以通过调整参数进行以下操作：

- 删除表中的空白行：`no.space`
- 移除不关心的统计量：`omit.stat`
- 修改因变量和自变量的名称：`dep.var.labels` 、 `covariate.labels` 

```R 回归表的修饰
stargazer(linear.1, linear.2, probit.model, title = "Regression Results",
align = TRUE, dep.var.labels = c("Overall Rating","High Rating"),
covariate.labels = c("Handling of Complaints", "No Special Privileges",
"Opportunity to Learn", "Performance-Based Raises", "Too Critical","Advancement"),
omit.stat = c("LL", "ser", "f"), no.space = TRUE)
```

![回归表的修饰](http://static.datartisan.com/upload/attachment/2016/12/xHMLhXyR.png)

本例中对原表格做了以下修改：

> 1. 使用 `dep.var.labels` 和 `covariate.lables` 参数分别将因变量和自变量重命名为容易理解的形式；
>
> 2. 使用 `omit.stat` 参数移除对数似然比（`"LL"`）、标准化残差（`"ser"`）和 F 统计量（`"f"`）；
>
> 3. 使用`no.space`参数将输出表格中的空行删去。





#### 展示置信区间

- 设置是否展示置信区间：`ci`
- 设置置信区间的置信度：`ci.level`
- 使回归系数与置信区间并排展示：`single.row`

```R 展示置信区间
stargazer(linear.1, linear.2, title = "Regression Results",
dep.var.labels = c("Overall Rating", "High Rating"),
covariate.labels = c("Handling of Complaints", "No Special Privileges",
"Opportunity to Learn", "Performance-Based Raises", "Too Critical", "Advancement"),
omit.stat = c("LL","ser","f"), ci = TRUE, ci.level = 0.90, single.row = TRUE)
```

![展示置信区间](http://static.datartisan.com/upload/attachment/2016/12/rUe11IM4.png)



#### 其他修饰功能

> 控制自变量展示的顺序：`order`
> 控制要展示的统计量：`keep.stat` , `keep.stat = "n"` 即只展示样本量的大小，并移除其他统计量

```R 其他修饰功能
stargazer(linear.1, linear.2, title = "Regression Results",
dep.var.labels = c("Overall Rating", "High Rating"),
order = c("learning", "privileges"),
keep.stat = "n", ci = TRUE, ci.level = 0.90, single.row = TRUE)
```

![其他修饰功能](http://static.datartisan.com/upload/attachment/2016/12/u0K7suc0.png)



#### 控制输出格式

可以使用 `type` 参数控制以 `ASCII` 、`text`、`html`、`latex` 格式输出，默认为`LATEX` 格式。

```R 控制输出格式
stargazer(linear.1, linear.2, type = "text", title = "Regression Results",
dep.var.labels = c("Overall Rating", "High Rating"),
order = c("learning", "privileges"), 
keep.stat = "n", ci = TRUE, ci.level = 0.90, single.row = TRUE, header = F)
```

![控制输出格式](http://static.datartisan.com/upload/attachment/2016/12/gOA9wyCN.png)



#### 自定义统计量

我们使用 `sandwich` 包来计算异方差-稳健标准误，并将其与默认计算的标准差一同展示。

```R 自定义统计量
library(sandwich)
cov <- vcovHC(linear.1, type = "HC")
robust.se <- sqrt(diag(cov))

stargazer(linear.1, linear.1, se = list(NULL, robust.se),
column.labels = c("default", "robust"))
```

![自定义统计量](http://static.datartisan.com/upload/attachment/2016/12/PC8L8NoB.png)



### 支持的模型

目前 `stargazer` 支持以下模型结果的展示：

> aftreg (eha), arima (stats), betareg (betareg), binaryChoice (sampleSelection), bj (rms), brglm (brglm), censReg (censReg), coeftest (lmtest), coxph (survival), coxreg (eha), clm (ordinal), clogit (survival), cph (rms), dynlm (dynlm), ergm(ergm), errorsarlm (spdev), felm (lfe), gam (mgcv), garchFit (fGarch), gee (gee), glm (stats), Glm (rms), glmer (lme4), glmrob(robustbase), gls (nlme), Gls (rms), gmm (gmm), heckit (sampleSelection), hetglm (glmx), hurdle (pscl), ivreg (AER), lagarlm (spdep), lm(stats), lme (nlme), lmer (lme4), lmrob (robustbase), lrm (rms), maBina (erer), mclogit (mclogit), mlogit (mlogit), mnlogit (mnlogit), mlreg (eha), multinom (nnet), nlme (nlme), nlmer (lme4), ols (rms), pgmm(plm), phreg (eha), plm (plm), pmg (plm), polr (MASS), psm (rms), rem.dyad (relevent), rlm(MASS), rq (quantreg), Rq (rms), selection (sampleSelection), svyglm (survey), survreg (survival), tobit (AER), weibreg (eha), zeroinfl (pscl), as well as from the implementation of these in zelig. In addition, stargazer also supports the following zelig models: “relogit”, “cloglog.net”, “gamma.net”, “probit.net” and “logit.net”.



### 支持的模板

`style` 参数可以用来选择统计表的展现形式，你可以通过  `?stargazer` 查看具体参数的设置来获取具体支持的格式，目前支持的期刊统计图格式有 `American Economic Review`、 `Quarterly Journal of Economics`  等。


## 结合 rmarkdown 使用

~~~r 代码格式
```{r, results='asis'}
stargazer(model, header = F)
```
~~~

> **注意事项：**
> - 要加上 `results='asis'` 保证输出的是表格，而不是 LATEX 文本；
> - 参数 `align` 失效，不能使用；
> - 加上参数 `header=FALSE`，避免输出关于包作者的一些文本信息。


## 致谢


> ### 参考文章
> * [stargazer.pdf](https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf)
> * Hlavac, Marek (2018). stargazer: Well-Formatted Regression and Summary Statistics 
>   Tables. R package version 5.2.2. https://CRAN.R-project.org/package=stargazer
