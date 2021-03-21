---
title: "博客创作计划"
date: 2021-03-21T13:07:01+08:00
type: plan
comments: true
vertical: false
---


<!--more-->



数据科学笔记：

- R【全部用 Rmarkdown 实现】

- 数据处理系列（tidyverse）

    - 数据读取

        - **data.table**：最高支持100G数据，fread和fwrite快的你不敢想，还支持数据类型判断
            - **readr**：RStudio出品，完美替代原生数据IO函数
            - **readxl，openxlsx**：读写excel文件

        - 数据操作

        - **dplyr**：操作数据矿的神器，谁用谁知道
            - **plyr**：list，array，dataframe三者的神奇变化包
            - **data.table**：数据IO和操作神器
            - **tidyr**：处理脏数据
            - **tidytext**：分词以及词频提取
            - **stringr**：处理字符串的神包
            - **jiebaR，tm**：中文分词利器，NLP专用
            - **purrr**：apply函数的绝佳替代
            - **lubridate**：时间处理最好的包，没有之一
            - **broom**：将各种统计学模型结果数据框化
            - **tidyverse**：RStudio出品的集成包，数据科学一揽子解决方案
            - **magrittr**：管道符

        - 缺失值处理

        - **VIM**：可视化缺失情况
            - **Hmisc**：处理缺失值
            - **mice**：利用各种算法补全缺失值的包

    - 文本分析系列（jiebaR、text2vec）

    - 数据可视化系列

    - 系统：graphics（基础）、lattice（网格）、ggplot2（图层）

        - 修饰：

        - customLayout（拼图）
            - ggthemr（ggplot2 主题美化）
            - ggrepel（ggplot2 注释）
            - ggforce（放大指定区域图像）
            - treemapify（ggplot2 补充包-树状图）
            - ggridges（ggplot2 补充包-脊线图）
            - cowplot（ggplot2 期刊主题）

        - 专业：

        - ggfortify（时间序列图+聚类模型结果）
            - corrplot（相关矩阵图）

        - 交互： http://gallery.htmlwidgets.org/

        - dygraphs（时间序列图 http://rstudio.github.io/dygraphs/）
            - Leaflet（交互式地图 http://rstudio.github.io/leaflet/）
            - rChartCalendar（日历热图 https://github.com/ramnathv/rChartsCalendar）
            - wordcloud2（词云图）
            - 茫茫多

    - 成果展示系列

    - rmarkdown
        - rbookdown
        - rblogdown
        - rpagedown