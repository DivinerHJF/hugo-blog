---
title: "数据库笔记 02 - MySQL 函数"
date: 2020-02-17T23:46:13+08:00
categories: 数据库漫游
tags:
    - 技术
    - SQL
    - 数据库
mathjax: false
codes: [sql,]
comments: true
vertical: false
---


## MySQL 常用函数

### NULL 函数

ISNULL()、NVL()、IFNULL() 和 COALESCE() 函数



### 日期函数

MySQL 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式：YYYY-MM-DD
- DATETIME - 格式：YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式：YYYY-MM-DD HH:MM:SS
- YEAR - 格式：YYYY 或 YY

| 函数                                                         | 描述                                |
| :----------------------------------------------------------- | :---------------------------------- |
| [NOW()](https://www.runoob.com/sql/func-now.html)            | 返回当前的日期和时间                |
| [CURDATE()](https://www.runoob.com/sql/func-curdate.html)    | 返回当前的日期                      |
| [CURTIME()](https://www.runoob.com/sql/func-curtime.html)    | 返回当前的时间                      |
| [DATE()](https://www.runoob.com/sql/func-date.html)          | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.runoob.com/sql/func-extract.html)    | 返回日期/时间的单独部分             |
| [DATE_ADD()](https://www.runoob.com/sql/func-date-add.html)  | 向日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.runoob.com/sql/func-date-sub.html)  | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.runoob.com/sql/func-datediff-mysql.html) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.runoob.com/sql/func-date-format.html) | 用不同的格式显示日期/时间           |



### Aggregate 函数

SQL Aggregate 函数计算从列中取得的值，返回一个单一的值。

| 函数    | 描述                 |
| ------- | -------------------- |
| AVG()   | 返回平均值           |
| COUNT() | 返回行数             |
| FIRST() | 返回第一个记录的值   |
| LAST()  | 返回最后一个记录的值 |
| MAX()   | 返回最大值           |
| MIN()   | 返回最小值           |
| SUM()   | 返回总和             |



### Scalar 函数

SQL Scalar 函数基于输入值，返回一个单一的值。

| 函数                    | 描述                                     |
| ----------------------- | ---------------------------------------- |
| UCASE()                 | 将某个字段转换为大写                     |
| LCASE()                 | 将某个字段转换为小写                     |
| MID()                   | 从某个文本字段提取字符，MySql 中使用     |
| SubString(字段，1，end) | 从某个文本字段提取字符                   |
| LEN()                   | 返回某个文本字段的长度                   |
| ROUND()                 | 对某个数值字段进行指定小数位数的四舍五入 |
| NOW()                   | 返回当前的系统日期和时间                 |
| FORMAT()                | 格式化某个字段的显示方式                 |



有关上述函数的具体使用，可以参考：https://www.runoob.com/sql/sql-function.html
