---
title: PostgreSQL 快速入门
tags: 
- PostgreSQL
- Database
date: 2023-08-02 20:31:35
cover:
---

## PostgreSQL命令

1. 连接到数据库：

```shell
psql -U username -d database_name
```

请将`username`替换为您的用户名，`database_name`替换为您要连接的数据库名称。

2. 显示数据库列表：

```shell
\l
```

这将列出所有可用的数据库。

3. 切换到特定数据库：

```shell
\c database_name
```

请将`database_name`替换为您要切换到的数据库名称。

4. 显示表列表：

```shell
\dt
```

这将列出当前数据库中的所有表。

5. 显示表结构：

```shell
\d table_name
```

请将`table_name`替换为您要查看结构的表名称。

6. 执行SQL查询：

```shell
SELECT * FROM table_name;
```

请将`table_name`替换为您要查询的表名称。

7. 退出psql：

```shell
\q
```

这将退出psql命令行界面。


## 常用对象

在PostgreSQL中，有多种类型的数据库对象可用于组织和管理数据。以下是一些常见的数据库对象：

1. 表（Table）：表是存储数据的基本单位。它由行和列组成，每个列定义了表中的一个属性。

2. 视图（View）：视图是基于一个或多个表的查询结果的虚拟表。它可以简化复杂的查询，并提供一种抽象的方式来访问数据。

3. 索引（Index）：索引是一种数据结构，用于加快对表中数据的访问速度。它可以根据一个或多个列的值快速定位到匹配的行。

4. 序列（Sequence）：序列是一种生成唯一数值的对象。它通常用于为表的主键列生成自增的唯一标识符。

5. 函数（Function）：函数是一段可重复使用的代码，接受输入参数并返回一个值或结果集。它可以用于执行特定的计算或操作。

6. 存储过程（Stored Procedure）：存储过程是一组预定义的SQL语句，可以在数据库中进行调用。它可以接受输入参数，并根据需要执行一系列操作。

7. 触发器（Trigger）：触发器是与表相关联的一段代码，当满足特定条件时自动执行。它可以用于在数据插入、更新或删除时执行额外的逻辑操作。

8. 模式（Schema）：模式是用于组织和管理数据库对象的命名空间。它可以将数据库对象分组到逻辑上独立的单元中，以便更好地管理和控制访问权限。

## 常用数据类型

在PostgreSQL中，有多种常用的数据类型可用于存储和处理不同类型的数据。以下是一些常见的数据类型：

1. 整数类型（Integer Types）：用于存储整数值，如`integer`、`smallint`、`bigint`等。

2. 浮点数类型（Floating-Point Types）：用于存储浮点数值，如`real`、`double precision`等。

3. 字符串类型（Character Types）：用于存储文本数据，如`character varying`、`text`等。

4. 日期和时间类型（Date and Time Types）：用于存储日期、时间和时间戳，如`date`、`time`、`timestamp`等。

5. 布尔类型（Boolean Type）：用于存储布尔值，即`true`或`false`。

6. 数组类型（Array Types）：用于存储多个相同类型的值的数组，如`integer[]`、`text[]`等。

7. JSON类型（JSON Type）：用于存储JSON格式的数据，如`json`、`jsonb`等。

8.  UUID类型（UUID Type）：用于存储唯一标识符，如`uuid`。

## 表操作

在PostgreSQL中，可以使用SQL语句对表进行各种操作。以下是一些常见的表操作：

1. 创建表（Create Table）：使用CREATE TABLE语句创建一个新的表，并指定表的名称和列的定义。例如：

```sql
CREATE TABLE table_name (
    id serial PRIMARY KEY, -- 主键自增
    table1_id integer REFERENCES table1(id),
    column1 datatype DEFAULT default_value,
    ...
    column3 datatype,
    column2 datatype,
    PRIMARY KEY (column1, column2),--复合主键（Composite Primary Key）
    ...
);

--外部表（Foreign Table）
CREATE FOREIGN TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
) SERVER server_name OPTIONS (option1 'value1', option2 'value2', ...);
```

`SERIAL`是PostgreSQL中的一种伪类型，它实际上是一个自增长的整数列。

使用`SERIAL`数据类型可以简化自增长列的创建过程。当我们将列的数据类型设置为`SERIAL`时，PostgreSQL会自动创建一个序列（sequence），并将其与该列关联起来。每次插入新行时，序列会自动递增，并将递增后的值赋给`SERIAL`列。


1. 修改表结构（Alter Table）：使用ALTER TABLE语句修改表的结构，包括添加、修改或删除列，添加或删除约束等。例如：

```sql
-- 添加列
ALTER TABLE table_name ADD COLUMN column_name datatype;

-- 修改列类型
ALTER TABLE table_name ALTER COLUMN column_name TYPE new_datatype;

-- 删除列
ALTER TABLE table_name DROP COLUMN column_name;
```

3. 插入数据（Insert Into）：使用INSERT INTO语句向表中插入新的行数据。例如：

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

4. 更新数据（Update）：使用UPDATE语句更新表中的行数据。例如：

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

5. 删除数据（Delete）：使用DELETE语句删除表中的行数据。例如：

```sql
DELETE FROM table_name
WHERE condition;
```

6. 查询数据（Select）：使用SELECT语句从表中检索数据。例如：

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

7. 创建索引（Create Index）：使用CREATE INDEX语句创建索引，以加快对表中数据的访问速度。例如：

```sql
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```


## 注释

在PostgreSQL中，可以使用注释来为表、列、约束等数据库对象添加说明和描述。注释可以提供有关对象的额外信息，以便于理解和维护数据库结构。以下是在PostgreSQL中添加注释的方法：

1. 表注释（Table Comment）：可以使用COMMENT语句为表添加注释。例如：

```sql
COMMENT ON TABLE table_name IS 'This is a table comment.';
```

2. 列注释（Column Comment）：可以使用COMMENT语句为表中的列添加注释。例如：

```sql
COMMENT ON COLUMN table_name.column_name IS 'This is a column comment.';
```

3. 约束注释（Constraint Comment）：可以使用COMMENT语句为表中的约束添加注释。例如：

```sql
COMMENT ON CONSTRAINT constraint_name ON table_name IS 'This is a constraint comment.';
```

4. 数据库注释（Database Comment）：可以使用COMMENT语句为整个数据库添加注释。例如：

```sql
COMMENT ON DATABASE database_name IS 'This is a database comment.';
```

5. 查看注释（View Comments）：可以使用以下查询语句查看表、列、约束等对象的注释：

```sql
-- 查看表注释
SELECT obj_description('table_name'::regclass) AS table_comment;

-- 查看列注释
SELECT col_description('table_name'::regclass, 'column_name') AS column_comment;

-- 查看约束注释
SELECT obj_description('constraint_name'::regclass) AS constraint_comment;

-- 查看数据库注释
SELECT description FROM pg_catalog.pg_shdescription WHERE objoid = 'database_name'::regclass;
```

注意：在上述查询语句中，将'table_name'、'column_name'、'constraint_name'和'database_name'替换为实际的对象名称。

## 用户操作

在PostgreSQL中，可以使用SQL语句和特定的命令来进行用户操作。以下是一些常见的用户操作：

1. 创建用户（Create User）：使用CREATE USER语句创建一个新的用户，并指定用户名和密码。例如：

```sql
CREATE USER username WITH PASSWORD 'password';
```

2. 修改用户密码（Alter User）：使用ALTER USER语句修改用户的密码。例如：

```sql
ALTER USER username WITH PASSWORD 'new_password';
```

3. 授予角色（Grant Role）：使用GRANT语句将角色授予用户。例如：

```sql
GRANT role_name TO username;
```

4. 撤销角色（Revoke Role）：使用REVOKE语句从用户中撤销角色。例如：

```sql
REVOKE role_name FROM username;
```

5. 授予权限（Grant Privileges）：使用GRANT语句将特定的权限授予用户。例如：

```sql
GRANT privilege_name ON table_name TO username;
```

6. 撤销权限（Revoke Privileges）：使用REVOKE语句从用户中撤销特定的权限。例如：

```sql
REVOKE privilege_name ON table_name FROM username;
```

7. 删除用户（Drop User）：使用DROP USER语句删除用户。例如：

```sql
DROP USER username;
```

8. 查看用户（View Users）：可以使用以下查询语句查看数据库中的用户列表：

```sql
SELECT usename FROM pg_user;
```

## 常用查询

在PostgreSQL中，有许多常用的查询语句可以帮助你检索和操作数据库中的数据。以下是一些常见的查询示例：

1. 查询所有数据（Select All）：使用SELECT语句可以检索表中的所有数据。例如：

```sql
SELECT * FROM table_name;
```

2. 条件查询（Conditional Query）：使用WHERE子句可以根据条件过滤数据。例如，查询年龄大于等于18岁的用户：

```sql
SELECT * FROM users WHERE age >= 18;
```

3. 排序查询（Ordering Query）：使用ORDER BY子句可以按照指定的列对结果进行排序。例如，按照用户的姓名进行升序排序：

```sql
SELECT * FROM users ORDER BY name ASC;
```

4. 聚合查询（Aggregate Query）：使用聚合函数可以对数据进行统计和计算。例如，计算某个列的平均值：

```sql
SELECT AVG(column_name) FROM table_name;
```

5. 连接查询（Join Query）：使用JOIN语句可以将多个表连接在一起进行查询。例如，查询订单表和用户表中相关联的数据：

```sql
SELECT * FROM orders
JOIN users ON orders.user_id = users.id;
```

6. 分组查询（Grouping Query）：使用GROUP BY子句可以将数据按照指定的列进行分组。例如，按照用户的性别统计用户数量：

```sql
SELECT gender, COUNT(*) FROM users GROUP BY gender;
```

7. 子查询（Subquery）：可以在查询中嵌套使用子查询来获取更复杂的结果。例如，查询年龄大于平均年龄的用户：

```sql
SELECT * FROM users WHERE age > (SELECT AVG(age) FROM users);
```

这些是一些常见的查询示例，可以根据具体的需求和数据结构进行相应的查询操作。使用合适的查询语句和条件，可以有效地检索和操作数据库中的数据。


