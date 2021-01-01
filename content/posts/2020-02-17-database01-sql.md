---
title: "数据库漫游指南 01 - SQL 语法篇"
date: 2020-02-17T22:28:47+08:00
categories: 技术
tags:
    - SQL
    - 数据库
mathjax: false
codes: [sql,]
comments: true
vertical: false
---

SQL（Structured Query Language）是「结构化查询语言」，它是对关系型数据库的操作语言。它可以应用到所有关系型数据库中，例如：MySQL、Oracle、SQL Server等。它主要划分为以下几类：

- **DQL - Data Query Language**：数据查询语言，用来查询记录（数据）。<u>select</u>
- **DDL - Data Definition Language**：数据定义语言，用来定义数据库对象；<u>create/drop/alter</u>
- **DML - Data Manipulation Language**：数据操作语言，用来定义数据库记录；<u>Insert/delete/update</u>
- **DCL - Data Control Language**：数据控制语言，用来定义访问权限和安全级别；

## DQL：数据查询语言

```sql
SELECT [DISTINCT]
	<select_list> /*要查询的列名称*/
FROM
	<left_table> <join_type> 
	JOIN <right_table> ON <join_condition>/*要查询的表名称*/
WHERE
	<where_condition> /*行条件*/
GROUP BY
	<group_by_list> /*对结果分组*/
HAVING
	<having_condition> /*分组后的行条件*/
ORDER BY
	<sorting_columns> [ASC|DESC] /*对结果排序*/
LIMIT
	<offset_start, row_length> /*结果限定*/
```



### MySQL - 执行顺序

随着 Mysql 版本的更新换代，其优化器也在不断的升级，优化器会分析不同执行顺序产生的性能消耗不同而动态调整 `执行顺序`。下面是经常出现的查询顺序：

```sql
FROM <left_table> 
ON <join_condition>
<join_type> JOIN <right_table>
WHERE <where_condition>
GROUP BY <group_by_list>
HAVING <having_condition>
SELECT 
[DISTINCT] <select_list>
ORDER BY <sorting_columns> [ASC|DESC]
LIMIT <offset_start, row_length>
```



### Condition - 行筛选条件

#### IN - 多值匹配

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...);
```



#### LIKE - 字符匹配

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern;
```


| 通配符                         | 描述                       |
| :----------------------------- | :------------------------- |
| %                              | 替代 0 个或多个字符        |
| _                              | 替代一个字符               |
| [*charlist*]                   | 字符列中的任何单一字符     |
| [^*charlist*] 或 [!*charlist*] | 不在字符列中的任何单一字符 |



#### BETWEEN - 范围匹配

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

> - **AND & OR 运算符**
>
> 如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。
>
> 如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。



### HAVING - 分组筛选

在 SQL 中增加 `HAVING` 子句原因是，`WHERE` 关键字无法与聚合函数 `GROUP BY` 一起使用。`HAVING` 子句可以让我们筛选分组后的各组数据。

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```



### TOP / LIMIT / ROWNUM - 结果限定

- SQL Server / MS Access 语法

```sql
SELECT TOP number|percent column_name(s)
FROM table_name;
```

- Oracle 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;
```

- MySQL 语法

```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

> 附注：**OFFSET**
>
> 为了与 PostgreSQL 兼容，MySQL 也支持句法： `LIMIT # OFFSET #`
>
> 1. selete * from testtable limit 2,1;
>
> 2. selete * from testtable limit 2 offset 1;
>
> **注意：**
>
> 1. 数据库数据计算是从 0 开始的
>
> 2. offset X 是跳过 X 个数据，limit Y 是选取 Y 个数据
>
> 3. limit X, Y 中 X 表示跳过 X 个数据，读取 Y 个数据



### ---------------------------------------

### AS - 别名

- 列的 SQL 别名语法

```sql
SELECT column_name AS alias_name
FROM table_name;
```

- 表的 SQL 别名语法

```sql
SELECT column_name(s)
FROM table_name AS alias_name;
```

> **在下面的情况下，使用别名很有用：**
>
> 1. 在查询中涉及超过一个表
>
> 2. 在查询中使用了函数
>
> 3. 列名称很长或者可读性差
>
> 4. 需要把两个列或者多个列结合在一起



### JOIN - 多表连接

![sql-join.png](https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png)



> **SQL JOIN 语法：**
>
> 1. INNER JOIN：如果表中有至少一个匹配，则返回行
>
> 2. LEFT JOIN：即使右表中没有匹配，也从左表返回所有的行
>
> 3. RIGHT JOIN：即使左表中没有匹配，也从右表返回所有的行
>
> 4. FULL JOIN：只要其中一个表中存在匹配，则返回行



### AUTO INCREMENT - 自增序列

在每次插入新记录时，自动地创建主键字段的值。

下面的 SQL 语句把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段：

```sql
CREATE TABLE Persons
(
ID int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (ID)
)
```

MySQL 使用 AUTO_INCREMENT 关键字来执行 auto-increment 任务。

默认地，AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。

要让 AUTO_INCREMENT 序列以其他的值起始，请使用下面的 SQL 语法：

```sql
ALTER TABLE Persons AUTO_INCREMENT=100
```

要在 "Persons" 表中插入新记录，我们不必为 "ID" 列规定值（会自动添加一个唯一的值）：

```sql
INSERT INTO Persons (FirstName,LastName)
VALUES ('Lars','Monsen')
```





-----

## DML：数据操作语言

### INSERT INTO - 插入纪录

有两种编写形式，第一种形式无需指定要插入数据的列名，只需提供被插入的值即可：

```sql
INSERT INTO table_name
VALUES (value1,value2,value3,...);
```

第二种形式需要指定列名及被插入的值：

```sql
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```

- **insert into select 和 select into from 的区别**

```sql
--插入一行,要求表scorebak 必须存在
insert into scorebak select * from socre where neza='neza'

--也是插入一行,要求表scorebak 不存在
select *  into scorebak from score  where neza='neza'
```




### UPDATE - 更新记录

```sql
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE some_column=some_value;
```

> **请注意 SQL UPDATE 语句中的 WHERE 子句！**
>
> WHERE 子句规定哪条记录或者哪些记录需要更新。如果您省略了 WHERE 子句，所有的记录都将被更新！



### DELETE - 删除记录

```sql
DELETE FROM table_name
WHERE some_column=some_value;
```

> **请注意 SQL DELETE 语句中的 WHERE 子句！**
>
> WHERE 子句规定哪条记录或者哪些记录需要删除。
>
> 如果您省略了 WHERE 子句，所有的记录都将被删除！当然，这意味着表结构、属性、索引将保持不变。





-----

## DDL：数据定义语言

### CREATE DATABASE - 创建数据库

```sql
CREATE DATABASE dbname;
```



### CREATE TABLE - 创建数据表

```sql
CREATE TABLE table_name
(
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
....
);
```

- column_name 参数规定表中列的名称。
- data_type 参数规定列的数据类型（例如 varchar、integer、decimal、date 等等）。
- size 参数规定表中列的最大长度。

> **提示：**如需了解 MySQL 和 SQL Server 中的数据类型，可以访问：[数据类型参考手册](https://www.runoob.com/sql/sql-datatypes.html)



#### Constraints - 约束规则

- SQL CREATE TABLE + CONSTRAINT 语法

```sql
CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
....
);
```

在 SQL 中，我们有如下约束：

- **NOT NULL** - 指示某列不能存储 NULL 值

  ```sql
  CREATE TABLE Persons (
      ID int NOT NULL,
      LastName varchar(255) NOT NULL,
      FirstName varchar(255) NOT NULL,
      Age int
  );
  
  -- 添加 NOT NULL 约束
  ALTER TABLE Persons
  MODIFY Age int NOT NULL;
  
  -- 删除 NOT NULL 约束
  ALTER TABLE Persons
  MODIFY Age int NULL;
  ```

- **UNIQUE** - 保证某列的每行必须有唯一的值

  ```sql
  -- CREATE TABLE 时的 SQL UNIQUE 约束
  -- MySQL：
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  UNIQUE (P_Id)
  )
  
  -- SQL Server / Oracle / MS Access：
  CREATE TABLE Persons
  (
  P_Id int NOT NULL UNIQUE,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255)
  )
  
  -- 多个列的 UNIQUE 约束
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
  )
  
  -- uc_PersonID 是一个约束名！上面建的是唯一约束，为了方便区别约束名一般起得有规律点。
  -- 比如 UC（就是 UNIQUE CONSTRAINT 的缩写意思是唯一约束）
  -- uc_PersonID 就是对表中的PersonID 建唯一约束，强制约束 Id_P 和 LastName 唯一。
  
  
  -- ALTER TABLE 时的 SQL UNIQUE 约束
  -- MySQL / SQL Server / Oracle / MS Access：
  ALTER TABLE Persons
  ADD UNIQUE (P_Id)
  
  -- 多个列的 UNIQUE 约束
  ALTER TABLE Persons
  ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
  
  
  -- 撤销 UNIQUE 约束
  -- MySQL：
  ALTER TABLE Persons
  DROP INDEX uc_PersonID
  
  -- SQL Server / Oracle / MS Access：
  ALTER TABLE Persons
  DROP CONSTRAINT uc_PersonID
  ```

- **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合

  ```sql
  -- CREATE TABLE 时的 SQL PRIMARY KEY 约束
  -- MySQL：
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  PRIMARY KEY (P_Id)
  )
  
  -- SQL Server / Oracle / MS Access：
  CREATE TABLE Persons
  (
  P_Id int NOT NULL PRIMARY KEY,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255)
  )
  
  -- 定义多个列的 PRIMARY KEY 约束：
  -- 注释：在下面的实例中，只有一个主键 PRIMARY KEY（pk_PersonID）。
  -- 然而，pk_PersonID 的值是由两个列（P_Id 和 LastName）组成的。
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
  )
  
  
  -- ALTER TABLE 时的 SQL PRIMARY KEY 约束
  -- 注释：如果您使用 ALTER TABLE 语句添加主键，必须把主键列声明为不包含 NULL 值（在表首次创建时）。
  -- MySQL / SQL Server / Oracle / MS Access：
  ALTER TABLE Persons
  ADD PRIMARY KEY (P_Id)
  
  -- 定义多个列的 PRIMARY KEY 约束：
  ALTER TABLE Persons
  ADD CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
  
  
  -- 撤销 PRIMARY KEY 约束
  -- MySQL：
  ALTER TABLE Persons
  DROP PRIMARY KEY
  
  -- SQL Server / Oracle / MS Access：
  ALTER TABLE Persons
  DROP CONSTRAINT pk_PersonID
  ```

- **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性

  ```sql
  -- CREATE TABLE 时的 SQL PRIMARY KEY 约束
  -- MySQL：
  CREATE TABLE Orders
  (
  O_Id int NOT NULL,
  OrderNo int NOT NULL,
  P_Id int,
  PRIMARY KEY (O_Id),
  FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
  )
  
  -- SQL Server / Oracle / MS Access：
  CREATE TABLE Orders
  (
  O_Id int NOT NULL PRIMARY KEY,
  OrderNo int NOT NULL,
  P_Id int FOREIGN KEY REFERENCES Persons(P_Id)
  )
  
  -- 定义多个列的 PRIMARY KEY 约束：
  CREATE TABLE Orders
  (
  O_Id int NOT NULL,
  OrderNo int NOT NULL,
  P_Id int,
  PRIMARY KEY (O_Id),
  CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id)
  REFERENCES Persons(P_Id)
  )
  
  
  -- ALTER TABLE 时的 SQL PRIMARY KEY 约束
  -- MySQL / SQL Server / Oracle / MS Access：
  ALTER TABLE Orders
  ADD FOREIGN KEY (P_Id)
  REFERENCES Persons(P_Id)
  
  -- 定义多个列的 PRIMARY KEY 约束：
  ALTER TABLE Orders
  ADD CONSTRAINT fk_PerOrders
  FOREIGN KEY (P_Id)
  REFERENCES Persons(P_Id)
  
  
  -- 撤销 PRIMARY KEY 约束
  -- MySQL：
  ALTER TABLE Orders
  DROP FOREIGN KEY fk_PerOrders
  
  -- SQL Server / Oracle / MS Access：
  ALTER TABLE Orders
  DROP CONSTRAINT fk_PerOrders
  ```

- **CHECK** - 保证列中的值符合指定的条件

  ```sql
  -- CREATE TABLE 时的 SQL PRIMARY KEY 约束
  -- MySQL：
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CHECK (P_Id>0)
  )
  
  -- SQL Server / Oracle / MS Access：
  CREATE TABLE Persons
  (
  P_Id int NOT NULL CHECK (P_Id>0),
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255)
  )
  
  -- 定义多个列的 PRIMARY KEY 约束：
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
  )
  
  
  -- ALTER TABLE 时的 SQL PRIMARY KEY 约束
  -- MySQL / SQL Server / Oracle / MS Access：
  ALTER TABLE Persons
  ADD CHECK (P_Id>0)
  
  -- 定义多个列的 PRIMARY KEY 约束：
  ALTER TABLE Persons
  ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
  
  
  -- 撤销 PRIMARY KEY 约束
  -- MySQL：
  ALTER TABLE Persons
  DROP CHECK chk_Person
  
  -- SQL Server / Oracle / MS Access：
  ALTER TABLE Persons
  DROP CONSTRAINT chk_Person
  ```

- **DEFAULT** - 规定没有给列赋值时的默认值

  ```sql
  -- CREATE TABLE 时的 SQL PRIMARY KEY 约束
  -- My SQL / SQL Server / Oracle / MS Access：
  CREATE TABLE Persons
  (
      P_Id int NOT NULL,
      LastName varchar(255) NOT NULL,
      FirstName varchar(255),
      Address varchar(255),
      City varchar(255) DEFAULT 'Sandnes'
  )
  
  
  -- ALTER TABLE 时的 SQL PRIMARY KEY 约束
  -- MySQL：
  ALTER TABLE Persons
  ALTER City SET DEFAULT 'SANDNES'
  
  -- SQL Server / MS Access：
  ALTER TABLE Persons
  ADD CONSTRAINT ab_c DEFAULT 'SANDNES' for City
  
  -- Oracle：
  ALTER TABLE Persons
  MODIFY City DEFAULT 'SANDNES'
  
  
  -- 撤销 PRIMARY KEY 约束
  -- MySQL：
  ALTER TABLE Persons
  ALTER City DROP DEFAULT
  
  -- SQL Server / Oracle / MS Access：
  ALTER TABLE Persons
  ALTER COLUMN City DROP DEFAULT
  ```

### CREATE INDEX - 创建索引

```sql
-- 在表上创建一个简单的索引，允许使用重复的值：
CREATE INDEX index_name
ON table_name (column_name)

-- 在表上创建一个唯一的索引，不允许使用重复的值：
CREATE UNIQUE INDEX index_name
ON table_name (column_name)

-- 多列索引，可以在括号中列出这些列的名称，用逗号隔开：
CREATE INDEX PIndex
ON Persons (LastName, FirstName)
```



### ALTER TABLE - 修改表

```sql
-- 如需在表中添加列，请使用下面的语法:
ALTER TABLE table_name
ADD column_name datatype

-- 如需删除表中的列，请使用下面的语法
-- （请注意，某些数据库系统不允许这种在数据库表中删除列的方式）：
ALTER TABLE table_name
DROP COLUMN column_name

-- 要改变表中列的数据类型，请使用下面的语法：
-- SQL Server / MS Access：
ALTER TABLE table_name
ALTER COLUMN column_name datatype
-- My SQL / Oracle：
ALTER TABLE table_name
MODIFY COLUMN column_name datatype
-- Oracle 10G 之后版本:
ALTER TABLE table_name
MODIFY column_name datatype;
```



### DROP - 删除

#### DROP INDEX - 撤销索引

```sql
-- 用于 MS Access 的 DROP INDEX 语法：
DROP INDEX index_name ON table_name

-- 用于 MS SQL Server 的 DROP INDEX 语法：
DROP INDEX table_name.index_name

-- 用于 DB2/Oracle 的 DROP INDEX 语法：
DROP INDEX index_name

-- 用于 MySQL 的 DROP INDEX 语法：
ALTER TABLE table_name DROP INDEX index_name
```

#### DROP TABLE - 撤销表

```sql
DROP TABLE table_name
```

#### DROP DATABASE - 撤销数据库

```sql
DROP DATABASE database_name
```





-----

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



