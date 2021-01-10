---
title: "数据库笔记 03 - 窗口函数篇"
date: 2020-12-01T13:37:44+08:00
categories: 数据库漫游
tags:
    - 技术
    - 窗口函数
    - 数据库
mathjax: false
codes: [sql]
comments: true
vertical: false
---

窗口函数的基本用法：

```sql
函数名() OVER() AS xx
```

- 窗口函数就是将表以窗口为单位进行分割，并在其中进行**分组排序**的函数。

- `OVER` 关键字用来指定函数执行的窗口范围，若后面括号中什么都不写，则意味着窗口包含满足 `WHERE` 条件的所有行，窗口函数基于所有行进行计算；如果不为空，则支持以下 4 种语法来设置窗口。

    ```sql
    mysql> SELECT
        -> RANK() OVER w AS rk,
        -> PERCENT_RANK() OVER w AS prk,
        -> stu_id, lesson_id, score
        -> FROM t_score
        -> WHERE stu_id < 3
        -> WINDOW w AS (PARTITION BY stu_id ORDER BY score)
        -> ;
    ```

    - `window_name`：给窗口指定一个别名。如果 SQL 中涉及的窗口较多，采用别名可以看起来更清晰易读

    - `partition by` 子句：窗口按照哪些字段进行分组，窗口函数在不同的分组上分别执行

    - `order by` 子句：按照哪些字段进行排序，窗口函数将按照排序后的记录顺序进行编号

    - `frame` 子句：frame 是当前分区的一个子集，子句用来定义子集的规则，通常用来作为滑动窗口使用，这样的统计方法称为**移动平均**

        ```sql
        --指定“最靠近的3行”作为汇总对象
        --使用 ROWS（行）和 PRECEDING(之前)两个关键字，将框架指定为“截止到之前~行”
         SELECT 
            product_id, 
            product_name, 
            sale_price,        
            AVG(sale_price) OVER (ORDER BY product_id ROWS 2 PRECEDING) AS moving_avg   
        FROM 
            Product;
        ```

        > 使用关键字 FOLLOWING(之后)替换 PRECEDING，就可以指定“截止到之后~行”作为框架。如果希望将当前记录的前后行作为汇总对象，可以同时使用 PRECEDING(之前)和 FOLLOWING（之后）关键字来实现。

        ```sql
        --将当前记录的前后行作为汇总对象
        --当前记录的前后行的具体含义就是：之前1行的记录 + 自身（当前记录）+ 之后1行的记录
        SELECT 
            product_id, 
            product_name, 
            sale_price,       
            AVG(sale_price) OVER (ORDER BY product_id ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_avg   
        FROM 
            Product;
        ```



## 窗口函数的执行顺序

> **原理**：窗口函数的执行顺序（逻辑上的）是在 FROM, JOIN, WHERE, GROUP BY, HAVING 之后，在 ORDER BY, LIMIT, SELECT DISTINCT 之前。它执行时 GROUP BY 的聚合过程已经完成了，所以不会再产生数据聚合。

注：窗口函数是在 where 之后执行的，所以如果 where 子句需要用窗口函数作为条件，需要多一层查询，在子查询外面进行

```sql
select user_id, avg(diff)
from
(
	select user_id, lead(log_time) over(partition user_id order by log_time) - log_time as diff
	from user_log
) t
where datediff(now(), t.log_time) <= 30
group by user_id
```



## 窗口函数和普通聚合函数的区别

1. 聚合函数是将多条记录聚合为一条；窗口函数是每条记录都会执行，有几条记录执行完还是几条

2. 所有的聚合函数都能用作窗口函数，起到类似**累加/累乘**的效果

    ```sql
    --将SUM函数作为窗口函数使用
    --使用聚合函数作为窗口函数时，需要在其括号内指定相应的列
     SELECT 
        product_id, 
        product_name, 
        sale_price,        
        SUM(sale_price) OVER (ORDER BY product_id) AS current_sum   
    FROM 
        Product;
    ```

3. 目前为止我们学过的函数大多数都没有使用位置的限制，最多也就是<u>在 WHERE 子句不能使用聚合函数</u>。但是，使用窗口函数的位置却有很大的限制，确切的说，<u>窗口函数只能在 SELECT 子句中使用</u>。



## 常用窗口函数

### 序号函数

> row_number(), rank(), dense_rank()
>
> 应用场景：【排名问题】 - 如，各科的第一名分别是谁？

- `ROW_NUMBER()`：顺序排序——1、2、3

- `RANK()`：并列排序，跳过重复序号——1、1、3

- `DENSE_RANK()`：并列排序，不跳过重复序号——1、1、2

    

### 分布函数

> percent_rank(), cume_dist()
>
> 应用场景：【比例问题】 - 如，小于等于当前成绩的比例？

- percent_rank()：每行按照公式`(rank-1) / (rows-1)`进行计算。其中，`rank`为`RANK()函数`产生的序号，`rows`为当前窗口的记录总行数

    ```sql
    --给窗口指定别名：WINDOW w AS (PARTITION BY stu_id ORDER BY score)
    --此例中 rows = 7
    mysql> SELECT
        -> RANK() OVER w AS rk,
        -> PERCENT_RANK() OVER w AS prk,
        -> stu_id, lesson_id, score
        -> FROM t_score
        -> WHERE stu_id < 3
        -> WINDOW w AS (PARTITION BY stu_id ORDER BY score)
        -> ;
    +----+------+--------+-----------+-------+
    | rk | prk  | stu_id | lesson_id | score |
    +----+------+--------+-----------+-------+
    |  1 |    0 |      1 | L003      |    79 |
    |  2 | 0.25 |      1 | L002      |    86 |
    |  3 |  0.5 |      1 | L004      |    88 |
    |  4 | 0.75 |      1 | L005      |    98 |
    |  4 | 0.75 |      1 | L001      |    98 |
    |  1 |    0 |      2 | L006      |    82 |
    |  2 |    1 |      2 | L007      |    99 |
    +----+------+--------+-----------+-------+
    ```

- cume_dist()：分组内小于、等于当前 rank 值的行数 / 分组内总行数 eg:查询小于等于当前成绩（score）的比例

    ```sql
    --cd1：没有分区，则所有数据均为一组，总行数为 8
    --cd2：按照 lesson_id 分成了两组，行数各为 4
    mysql> SELECT stu_id, lesson_id, score,
        -> CUME_DIST() OVER (ORDER BY score) AS cd1,
        -> CUME_DIST() OVER (PARTITION BY lesson_id ORDER BY score) AS cd2
        -> FROM t_score
        -> WHERE lesson_id IN ('L001','L002')
        -> ;
    +--------+-----------+-------+-------+------+
    | stu_id | lesson_id | score | cd1   | cd2  |
    +--------+-----------+-------+-------+------+
    |      2 | L001      |    84 | 0.125 | 0.25 |
    |      1 | L001      |    98 |  0.75 |  0.5 |
    |      4 | L001      |    99 | 0.875 | 0.75 |
    |      3 | L001      |   100 |     1 |    1 |
    |      1 | L002      |    86 |  0.25 | 0.25 |
    |      4 | L002      |    88 | 0.375 |  0.5 |
    |      2 | L002      |    90 |   0.5 | 0.75 |
    |      3 | L002      |    91 | 0.625 |    1 |
    +--------+-----------+-------+-------+------+
    ```

    

### 前后函数

> lag(expr, n), lead(expr, n)
>
> 作用：返回位于当前行的前 n 行（`LAG(expr,n)`）或后 n 行（`LEAD(expr,n)`）的 expr 的值
>
> 应用场景：查询前 1 名同学的成绩和当前同学成绩的差值

```sql
mysql> SELECT stu_id, lesson_id, score, pre_score,
    -> score-pre_score AS diff
    -> FROM(
    ->     SELECT stu_id, lesson_id, score,
    ->     LAG(score,1) OVER w AS pre_score
    ->     FROM t_score
    ->     WHERE lesson_id IN ('L001','L002')
    ->     WINDOW w AS (PARTITION BY lesson_id ORDER BY score)) t
    -> ;
+--------+-----------+-------+-----------+------+
| stu_id | lesson_id | score | pre_score | diff |
+--------+-----------+-------+-----------+------+
|      2 | L001      |    84 |      NULL | NULL |
|      1 | L001      |    98 |        84 |   14 |
|      4 | L001      |    99 |        98 |    1 |
|      3 | L001      |   100 |        99 |    1 |
|      1 | L002      |    86 |      NULL | NULL |
|      4 | L002      |    88 |        86 |    2 |
|      2 | L002      |    90 |        88 |    2 |
|      3 | L002      |    91 |        90 |    1 |
+--------+-----------+-------+-----------+------+
```



### 头尾函数

> FIRST_VALUE(expr), LAST_VALUE(expr)
>
> 作用：返回第一个（`FIRST_VALUE(expr)`）或最后一个（`LAST_VALUE(expr)`）expr 的值
>
> 应用场景：截止到当前成绩，按照日期排序查询第 1 个和最后 1 个同学的分数

```sql
mysql> SELECT stu_id, lesson_id, score, create_time,
    -> FIRST_VALUE(score) OVER w AS first_score,
    -> LAST_VALUE(score) OVER w AS last_score
    -> FROM t_score
    -> WHERE lesson_id IN ('L001','L002')
    -> WINDOW w AS (PARTITION BY lesson_id ORDER BY create_time)
    -> ;
+--------+-----------+-------+-------------+-------------+------------+
| stu_id | lesson_id | score | create_time | first_score | last_score |
+--------+-----------+-------+-------------+-------------+------------+
|      3 | L001      |   100 | 2018-08-07  |         100 |        100 |
|      1 | L001      |    98 | 2018-08-08  |         100 |         98 |
|      2 | L001      |    84 | 2018-08-09  |         100 |         99 |
|      4 | L001      |    99 | 2018-08-09  |         100 |         99 |
|      3 | L002      |    91 | 2018-08-07  |          91 |         91 |
|      1 | L002      |    86 | 2018-08-08  |          91 |         86 |
|      2 | L002      |    90 | 2018-08-09  |          91 |         90 |
|      4 | L002      |    88 | 2018-08-10  |          91 |         88 |
+--------+-----------+-------+-------------+-------------+------------+
```
