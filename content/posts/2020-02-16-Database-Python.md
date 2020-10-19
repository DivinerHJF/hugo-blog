---
title: "Python 漫游指南 - 数据库"
date: 2020-02-16T15:19:59+08:00
categories: 技术
tags:
    - 数据库
    - Python
mathjax: false
codes: []
comments: true
vertical: false
---

这两天待在家里比较闲，利用网络资源学了点儿数据库相关知识，同时对如何用 Python 与数据库进行交互有了进一步的了解。在这里主要是概览性的记录一下学习到的知识点，以备日后温习之用。

## Key 1. SQL 和 NoSQL

### SQL

SQL 是一种  **结构性查询语言**，用于在关系数据库管理系统（RDBMS）中检索和管理数据。

关系型数据库侧重于以二维表格的形式组织数据。 在实际的关系数据库中的关系也称表，一个关系数据库就是由若干个表组成，每个关系表是一个如下形式的二维数组：

```markdown
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  3 | kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | MP        |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
```

常见的以 SQL 作为标准数据库语言的关系型数据库有：[MySQL](http://www.mysql.com/)、[PostgreSQL](http://www.postgresql.org/)、[DB2](http://www-01.ibm.com/software/data/db2/)、[SQL Server](https://www.microsoft.com/en-us/sqlserver/default.aspx) 等。有关 SQL 语句的教程网上有很多，有兴趣的可以看看 [SQL 教程](https://www.runoob.com/sql/sql-tutorial.html)，比较全面。

### Nosql

Nosql (Not Only SQL) 指一系列旨在实现数据库模型的方法和项目，与传统的通过 SQL 访问数据的关系型数据库有很大的不同。 使用 NoSQL 的情况下，现实中实体之间的复杂关系可以通过使用不同的数据结构来存储，如：哈希表、数组、树等。

这两天主要学习了 MongoDB 和 Redis 的基本语法及其 Python 实现，有关语法可以参考 [MongoDB 教程](https://www.runoob.com/mongodb/mongodb-tutorial.html) 和 [Redis 教程](https://www.runoob.com/redis/redis-tutorial.html)。

### 总结 - 中文教程

- [SQL](https://www.runoob.com/sql/sql-tutorial.html)
- [MongoDB](https://www.runoob.com/mongodb/mongodb-tutorial.html)
- [Redis](https://www.runoob.com/redis/redis-tutorial.html)



## Key 2. Python 操作数据库

使用 Python 操作数据库主要有两种方式，以 MySQL 为例：

1. 使用原生模块：`pymysql`
2. ORM 框架：`SQLAchemy`

SQLAlchemy 是 python 编程语言下的一款 ORM 框架，面向对象编程把所有实体看成对象（object），关系型数据库则是采用实体之间的关系（relation）连接数据。很早就有人提出，关系也可以用对象表达，这样的话，就能使用面向对象编程，来操作关系型数据库。

简单说，ORM 就是通过实例对象的语法，完成关系型数据库的操作的技术，是"对象-关系映射"（Object/Relational Mapping） 的缩写，ORM 把数据库映射成对象：

- 数据库的表（table） --> 类（class）
- 记录（record，行数据）--> 对象（object）
- 字段（field）--> 对象的属性（attribute）

在 Python 中 MongoDB 也有类似的两种操作方式。用 Python 操作各种数据库的具体方法我以 `Jupyter Notebook` 的形式做了笔记，并上传到了我的 [Github ](https://github.com/DivinerHJF/Database-Python) 页面上。
