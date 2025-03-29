---
tags:
    - Theory
---

# SQL

SQL (Structured Query Language) 包括以下几个部分：

- Data Definition Language (DDL): 提供定义关系模式、删除关系、修改关系模式的命令
    - Integrity: 提供定义数据完整性约束的命令
    - View Definition: 提供定义视图的命令
- Data Manipulation Language (DML): 提供从数据库查询数据，以及插入、更新、删除元组的命令
- Transaction Control: 提供定义事务开始与结束的命令
- Embedded SQL and Dynamic SQL: 提供在 SQL 语句中嵌入其他 SQL 语句的命令
- Authorization: 提供定义用户对关系和视图的访问权限的命令

## SQL Data Definition

### Data Types

#### Basic Data Types

- char(n): 定长字符串，n 为字符数
- varchar(n): 变长字符串，n 为字符数
- int: 整型
- smallint: 短整型
- bigint: 长整型
- numeric(p,s): 定点数，p 为总位数，s 为小数点后位数
- real, double precision: 浮点数与双精度浮点数
- float(n): 精度至少为 n 位的浮点数

!!! tip
    char(n) 在存储长度不足 n 个字符的字符串时会在后面补空格，而 varchar(n) 不会.

    当比较两个 char 类型的字符串时，会在较短的字符串后补空格再比较；在比较 char 与 varchar 时，是否补空格与数据库的实现有关，因而建议全部使用 varchar 类型.

#### Built-in Data Types

- date: 日期 `'2005-7-27'`
- time: 时间 `'09:00:30.75'`
- timestamp: 时间戳（日期 + 时间） `'2005-7-27 09:00:30.75'`
- interval: 时间间隔 `'1 year 2 months 3 days 4 hours 5 minutes 6 seconds'`
    时间间隔可以由两个 date / time / timestamp 值相减得到

通过 `year(x)`, `month(x)`, `day(x)`, `hour(x)`, `minute(x)`, `second(x)` 函数可以分别获取日期 / 时间 / 时间戳的年 / 月 / 日 / 时 / 分 / 秒.

通过 `current_date()`, `current_time()` 可以获得当前日期 / 时间.

#### Large-Object Types

- clob(size): 字符串型大数据对象
- blob(size): 二进制型大数据对象

一般在数据库中存储 MB 或 GB 级别的对象是非常低效且耗费资源的，因此我们通常在数据库中保存大对象的定位器，宿主语言通过 SQL 得到定位器后，再从文件系统中读取对象.

#### User-Defined Data Types

SQL 支持两种形式的用户定义数据类型：独特类型（Distinct type）和结构化数据类型（Structured data type）.

由于某些用相同的基本类型存储的属性有不同的域，我们可以通过独特类型来区分这些不同的属性：

```sql
CREATE TYPE MyType AS BasicType FINAL;
```

不同类型的变量不能相互复制，会导致错误，若需要强制类型转换，可以通过 `CAST` 实现：

```sql
CAST(variable AS MyType)
```

### Create table

```sql
CREATE TABLE table_name (
    A1 D1,
    A2 D2,
    ...
    An Dn,
    (integrity_constraint_1),
    (integrity_constraint_2),
    ...
    (integrity_constraint_k)
);
```

或者也可以把完整性约束放在属性定义中：

```sql
CREATE TABLE table_name (
    A1 D1 (constraint_1),
    A2 D2 (constraint_2),
    ...
    An Dn (constraint_n)
);
```

完整性约束的类型：

- primary key $(A_{j_1}, A_{j_2}, \ldots, A_{j_p})$: 声明 $(A_{j_1}, A_{j_2}, \ldots, A_{j_p})$ 为主键，要求关系中没有两个元组的主键相同，并且主键中没有一个属性为 null
- foreign key $(A_{k_1}, A_{k_2}, \ldots, A_{k_q})$ references S: 声明 $(A_{k_1}, A_{k_2}, \ldots, A_{k_q})$ 为外键，要求关系中任意元组在属性 $(A_{k_1}, A_{k_2}, \ldots, A_{k_q})$ 必须对应于 S 中某元组在主键上的取值（其实不一定要求主键）
- not null: 声明该属性不允许空值
- default value: 声明该属性的默认值
- unique: 声明该属性的值必须唯一

也可以按照已有的表的模式创建新表：

```sql
CREATE TABLE new_table_name LIKE existing_table_name;
```

### Drop table

```sql
DROP TABLE table_name;
```

### Alter table

```sql
-- Add column
ALTER TABLE table_name ADD COLUMN Attribute_name DataType;

-- Drop column
ALTER TABLE table_name DROP COLUMN Attribute_name;

-- Modify column
ALTER TABLE table_name MODIFY COLUMN Attribute_name NewDataType [integrity_constraint];

-- Rename column
ALTER TABLE table_name CHANGE COLUMN Old_attribute_name New_attribute_name DataType;

-- Add primary key constraint
ALTER TABLE table_name ADD PRIMARY KEY (Attribute_name);

-- Add foreign key constraint
ALTER TABLE table_name ADD CONSTRAINT fk_name 
    FOREIGN KEY (Attribute_name) 
    REFERENCES fk_table_name (Fk_attribute_name);

-- Rename table
ALTER TABLE table_name RENAME TO new_table_name;
```

## SQL Query

### 基本结构

```sql
SELECT [DISTINCT | ALL] A1, A2, ..., An
FROM table1, table2, ..., tablek
WHERE condition;
```

SELECT 相当于投影算子 $\Pi$，FROM 相当于笛卡尔积算子 $\times$，WHERE 相当于选择算子 $\sigma$，上述 SQL 语句相当于

$$
    \Pi_{A_1, A_2, \ldots, A_n} (\sigma_{condition} (table_1 \times table_2 \times \cdots \times table_k)).
$$

- SELECT 子句中可以直接使用属性名，也可以使用算数表达式、函数等，运算对象为常数或元组的属性. 对于多关系查询，需要指定属性的表名.
- WHERE 子句中可以使用逻辑连词 `and`, `or`, `not`，比较运算符 `<`, `>`, `<=`, `>=`, `=`, `<>`，以及 `between` 和 `in` 等

#### Condition Expression in WHERE clause

- 对于整数，可以使用 `<`, `>`, `<=`, `>=`, `=`, `<>` 等比较运算符，也可使用 `between` 表示范围

    ```sql
    SELECT *
    FROM R_1
    WHERE A_1 BETWEEN 10 AND 20;
    ```

    表示查询 $R_1$ 中属性 $A_1$ 的值在 10 到 20 之间的所有元组.

- 对于字符串，可以使用 `=` 与 `<>` 进行比较（是否区分大小写取决于数据库系统的具体实现），也可使用 `like` 进行模式匹配（大小写敏感）

    - `%` 匹配任意字符串
    - `_` 匹配单个字符

    ```sql
    SELECT *
    FROM R_1
    WHERE A_1 LIKE 'abc__%';
    ```

    表示查询 $R_1$ 中属性 $A_1$ 的值以 `abc` 开头，后面跟着至少两个字符的所有元组.

    可以使用 `ESCAPE` 关键字来指定转义字符，如

    ```sql
    SELECT *
    FROM R_1
    WHERE A_1 LIKE 'a\%b\_c%' ESCAPE '\';
    ```

    表示查询 $R_1$ 中属性 $A_1$ 的值以 `a%b_c` 开头的所有元组.

#### Sort

可以使用 `ORDER BY` 子句对查询结果进行排序，`ORDER BY` 后面可以指定属性名或表达式，也可以指定 `ASC` 或 `DESC` 关键字来指定升序或降序排序.

```sql
SELECT *
FROM R_1
ORDER BY A1 ASC, A2 DESC;
```

表示查询 $R_1$ 中所有元组，并按照属性 $A_1$ 的值升序排序，再按照属性 $A_2$ 的值降序排序.

### Join

#### Natural join

假设 $A_1, A_2, \ldots, A_p$ 是关系 $R_1$ 与 $R_2$ 所有的公共属性，则

```sql
SELECT *
FROM R_1 NATURAL JOIN R_2;
```

等价于

```sql
SELECT *
FROM R_1, R_2
WHERE R_1.A1 = R_2.A1 AND R_1.A2 = R_2.A2 AND... AND R_1.Ap = R_2.Ap;
```

FROM 子句中笛卡尔积作用的对象也可以是如 `R_1 NATURAL JOIN R_2 NATURAL JOIN R_3` 这样包含连接的表达式，如

```sql
SELECT *
FROM R_1 NATURAL JOIN R_2, R_3
```

表示将 $R_1$ 与 $R_2$ 自然连接，再与 $R_3$ 做笛卡尔积.

#### Inner join

Natural join 会导致两个表中没有关系但是同名的属性被连接，为解决这个问题我们可以使用 Inner join. Inner join 与 Natural join 类似，但是用户可以指定连接条件：

```sql
SELECT *
FROM R_1 INNER JOIN R_2 USING (A1);
```

等价于

```sql
SELECT *
FROM R_1, R_2
WHERE R_1.A1 = R_2.A1;
```

`INNER JOIN` 中的 `INNER` 可以省略，`USING` 可以用 `ON` 替代：

```sql
SELECT *
FROM R_1 JOIN R_2 ON R_1.A1 = R_2.A1;
```

#### Outer join

Natural join 与 Inner join 都会将两个表中无法连接的元组舍弃，若我们需要保留这些丢失的信息，则需要使用 Outer join.

- Left outer join（⟕）: 保留左表中所有的元组，右表中无法连接的元组用 NULL 填充

```sql
SELECT *
FROM R_1 LEFT OUTER JOIN R_2 USING (A1);
```

- Right outer join（⟖）: 保留右表中所有的元组，左表中无法连接的元组用 NULL 填充

```sql
SELECT *
FROM R_1 RIGHT OUTER JOIN R_2 USING (A1);
```

- Full outer join（⟗）: 保留左右表中所有的元组，无法连接的元组用 NULL 填充

```sql
SELECT *
FROM R_1 FULL OUTER JOIN R_2 USING (A1);
```

!!! note "Join Condition"
    连接条件（join condition）可由下列语句指定：

    - `NATURAL`: 连接两个表中所有的同名属性
    - `USING (A1)`: 连接两个表中属性 $A_1$ 相同的元组
    - `ON Exp1`: 连接两个表中满足表达式 `Exp1` 的元组

!!! tip
    多表连接在数据规模较大时会严重影响性能.

### Rename

使用 `as` 关键字可以对 SELECT 子句中的属性进行重命名，也可以对 FROM 子句中的关系（表达式）进行重命名，相当于重命名算子 $\rho$.

```sql
SELECT A1 as a, A2 as b, ..., An as c
FROM R1 as r1, R2 as r2, ..., Rn as rn
WHERE condition;
```

我们还可以给同一个表起多个表别名（table alias），用于表达某些特殊的查询需求，如

```sql
SELECT DISTINCT T.name
FROM instructor AS T, instructor AS S
WHERE T.salary > S.salary AND S.dept_name = 'Biology';
```

表示找到所有薪水超过生物学院最低薪水的老师的名字. `T` 和 `S` 这样的别名在 SQL 标准中被称为相关名称（correlation name），也被称为相关变量（correlation variable）或元组变量（tuple variable）.

### Set Operations

集合运算（set operations）包括并、交、差等，要求参与运算的两个关系的属性必须完全相同.

#### Union

使用 `UNION` 关键字可以将两个或多个 SELECT 语句的结果合并成一个结果集，相当于并算子 $\cup$.

```sql
(SELECT A1, A2 FROM R1 WHERE condition1)
UNION
(SELECT A1, A2 FROM R2 WHERE condition2)
```

表示将 $R_1$ 中满足条件 `condition1` 的元组与 $R_2$ 中满足条件 `condition2` 的元组合并成一个结果集.

`UNION` 默认去重，如果需要保留重复的元组，可以使用 `UNION ALL` 关键字.

#### Intersect

使用 `INTERSECT` 关键字可以求两个或多个 SELECT 语句的交集.

```sql
(SELECT A1, A2 FROM R1 WHERE condition1)
INTERSECT
(SELECT A1, A2 FROM R2 WHERE condition2)
```

表示求 $R_1$ 中满足条件 `condition1` 的元组与 $R_2$ 中满足条件 `condition2` 的元组的交集.

#### Except

使用 `EXCEPT` 关键字可以求两个或多个 SELECT 语句的差集，相当于差算子 $\setminus$.

```sql
(SELECT A1, A2 FROM R1 WHERE condition1)
EXCEPT
(SELECT A1, A2 FROM R2 WHERE condition2)
```

表示求 $R_1$ 中满足条件 `condition1` 的元组与 $R_2$ 中满足条件 `condition2` 的元组的差集.

### Aggregation

聚合（aggregation）是指对一组数据进行汇总，得到一个单一的结果，如求和、平均值、最大值、最小值、计数等.

#### Basic Aggregation

可以使用聚合函数对查询结果进行聚合操作，这些函数接收一个或多个属性作为输入，并返回一个单一的值.

- `COUNT`: 计算满足条件的元组数
- `SUM`: 求和
- `AVG`: 计算平均值
- `MAX`: 计算最大值
- `MIN`: 计算最小值

`SUM` 与 `AVG` 只能用于数值型属性，而另外三个函数可以用于任何属性.

```sql
SELECT AVG(A1) as avg_A1, SUM(A2) as sum_A2, COUNT(DISTINCT A3) as count_distinct_A3
FROM R1
WHERE condition;
```

表示求 $R_1$ 中满足条件 `condition` 的元组中，属性 $A_1$ 的平均值、属性 $A_2$ 的总和、属性 $A_3$ 的不同值的个数.

#### Group by

使用 `GROUP BY` 子句可以对查询结果进行分组，并对每个组内的数据进行聚合操作.

```sql
SELECT A1, SUM(A2) as sum_A2
FROM R1
WHERE condition
GROUP BY A1;
```

表示对 $R_1$ 中所有满足条件 `condition` 的元组按照属性 $A_1$ 进行分组，并求每个组内属性 $A_2$ 的总和.

!!! note
    - 含有 `GROUP BY` 子句的查询要求 `SELECT` 子句中除了作为分组依据的属性外，其余的属性必须出现在聚合函数中.
    - 除了 `COUNT(*)` 之外的所有聚合函数都忽略输入集合中的 NULL 值.

若要进一步对组进行筛选，可以使用 `HAVING` 子句：

```sql
SELECT A1, SUM(A2) as sum_A2
FROM R1
WHERE condition
GROUP BY A1
HAVING sum_A2 > 100;
```

表示对 $R_1$ 中所有满足条件 `condition` 的元组按照属性 $A_1$ 进行分组，并求每个组内属性 $A_2$ 的总和，但只保留总和大于 100 的组.


### Nested Query

嵌套查询（nested query）是指在一个查询语句中嵌入另一个查询语句. 由于查询语句返回的是一个结果集合，因而我们需要一些运算符对结果集合进行处理.

#### Set Membership

`IN` 运算符可以判断一个值是否属于一个集合，`NOT IN` 运算符可以判断一个值是否不属于一个集合.

```sql
SELECT DISTINCT course_id
FROM section
WHERE semester = 'Fall' AND year = 2009 AND course_id IN (
    SELECT course_id
    FROM section
    WHERE semester = 'Spring' AND year = 2010
);
```

表示查询在 2009 年秋季和 2010 年春季都开设的课程 ID.

#### Set Comparison

`SOME` 表示集合中至少有一个元素满足条件，`ALL` 表示集合中所有元素都满足条件.

```sql
SELECT name
FROM instructor
WHERE salary > SOME (
    SELECT salary
    FROM instructor
    WHERE dept_name = 'Biology'
);
```

表示查询薪水高于生物学院最低薪水的所有老师的姓名.

```sql
SELECT dept_name
FROM instructor
GROUP BY dept_name
HAVING AVG(salary) >= ALL (
    SELECT AVG(salary)
    FROM instructor
    GROUP BY dept_name
);
```

表示查询平均薪水最高的部门名称.

#### Empty Relation

`EXISTS` 可以返回一个子查询的结果中是否存在元组.

使用 `NOT EXISTS (B EXCEPT A)` 可以判断 $B$ 是否含于 $A$.

#### Duplicated Tuples

`UNIQUE` 可以返回一个子查询的结果中是否存在重复的元组，注意如果结果为空，`UNIQUE` 同样返回 `TRUE`.

#### Nested Query in FROM clause

子查询返回的是一个关系，同样可以作为 FROM 子句的对象. 例如：

```sql
SELECT *
FROM (
    SELECT A1, AVG(A2)
    FROM R1
    GROUP BY A1
) AS R2(A1, avg_A2)
WHERE avg_A2 > 100;
```

返回 `A2` 的平均值大于 100 的分组.

某些 SQL 实现（如 MySQL）要求 FROM 子句中的子查询必须有别名，否则会报错.

#### Scalar Subquery

标量子查询（scalar subquery）返回一个单一的值，可以作为表达式的一部分，出现在 SELECT, WHERE 和 HAVING 子句中.

```sql
SELECT A1, (
    SELECT AVG(A2)
    FROM R1
    WHERE R2.A1 = R1.A1
) AS avg_A2
FROM R2;
```

### WITH clause

`WITH` 子句可以临时定义一个关系，相当于赋值算子 $\leftarrow$，可以用于简化复杂的查询.

```sql
WITH R1 AS (
    SELECT A1, SUM(A2)
    FROM R2
    WHERE condition1
    GROUP BY A1
)
SELECT R1.A1, R3.A3
FROM R1, R3
WHERE R1.A1 = R3.A1;
```

!!! tip
    `WITH` 子句定义的临时表只能用在 FROM 里面.

## Modifying Data

### Delete

使用 DELETE 语句可以删除满足条件的元组，其语法与 SELECT 语句类似.

```sql
DELETE FROM R1
WHERE condition;
```

### Insert

使用 INSERT 语句可以向关系中插入元组，以下是手动插入数据的方式：

```sql
INSERT INTO R1 (A1, A2, ..., An)
    VALUES (value11, value12, ..., value1N)
           (value21, value22, ..., value2N)
           ...
           (valueM1, valueM2, ..., valueMN);
```

INSERT 语句也可以使用子查询自动插入数据：

```sql
INSERT INTO R1 (A1, A2, ..., An)
    SELECT B1 as A1, B2 as A2, ..., Bn as An
    FROM R2, R3, ..., Rm
    WHERE condition;
```

### Update

使用 UPDATE 语句可以更新满足条件的元组，其语法与 SELECT 语句类似.

```sql
UPDATE R1
SET A1 = E1, A2 = E2, ..., An = En
WHERE condition;
```

使用 CASE 表达式可以实现对不同条件的元组进行不同的更新操作，同时避免更新顺序的影响：

```sql
UPDATE R1
set A1 = CASE
    WHEN condition1 THEN E1
    WHEN condition2 THEN E2
    ...
    ELSE Ek
END
```

## View

视图（View）是一种基于查询建立的虚拟关系，不预先计算并存储视图中的数据，而是在使用虚拟关系的时候才通过查询计算出结果. 视图不是逻辑模型的一部分，而是用户层的概念.

!!! advice "视图的优点"
    - 可以向用户隐藏特定的数据，保护数据安全
    - 可以简化复杂的查询语句
    - 可以为用户提供更为直观的数据结构，隐藏逻辑层的复杂性

### View creation and selection

MySQL 中创建视图的语法如下：

```sql
CREATE VIEW view_name(NEW_A1, NEW_A2, ..., NEW_An) AS <query expression>;
```

视图不仅可以建立在表上，也可以建立在其他已定义的视图上.

使用视图进行查询的语法与查询普通表的语法一样：

```sql
SELECT * FROM view_name;
```

当在视图上查询时，系统会进行视图展开操作，将视图名展开为子查询.

!!! note "物化视图"
    物化视图（Materialized View）也是一种视图，但是将视图的结果存储在物理表中，以便快速查询.

    物化视图与普通的表的区别是需要跟随源表更新并保持在最新状态，该过程被称为物化视图维护（Materialized View Maintenance），因而物化视图适合需要频繁查询而更新较少的场景，特别是在大关系上的聚合查询，可以减少每次访问视图的开销.

### View update

由于视图查询的结果可能会缺少某些属性值，我们在对视图进行更新时有两种选择：

1. 拒绝操作
2. 将缺少的属性值设为 null

一般来说，如果定义视图的查询满足以下条件，则该视图是可更新的（Updatable）：

- FROM 子句中只包含一个数据库关系
- SELECT 子句中只包含关系的属性名，不包含任何表达式、聚合函数或 DISTINCT 关键字
- 任何未出现在 SELECT 子句中的属性可以取空值
- 查询中不含 GROUP BY 和 HAVING 子句

若一个视图满足以上限制，则可以像更新普通表一样更新一个视图，系统会自动将视图上的更新应用到源关系上.

## Index

索引（Index）是一种建立在表中某个属性上的数据结构，允许根据该属性快速查询表中的数据，不需要遍历整个表.

索引的创建方法：

```sql
CREATE INDEX index_name ON table_name (column_name);
```

## Transaction



## Integrity Constraints


## Authorization


