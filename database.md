# 数据库操作

## 一、分类

**SQL**：一门操作关系型数据库的编程语言，定义操作所有关系型数据库的**统一标准**。

| 分类 | 全称                       | 说明                                                     |
| ---- | -------------------------- | :------------------------------------------------------- |
| DDL  | Data Definition Language   | 数据定义语言，用来定义数据库对象（数据库，表，字段）。   |
| DML  | Data Manipulation Language | 数据操作语言，用来对数据库的表进行增删改。               |
| DQL  | Data Query Language        | 数据查询语言，用来查询数据库中表的记录。                 |
| DCL  | Data Control Language      | 数据控制语言，用来创建数据库用户，控制数据库的访问权限。 |

## 二、DDL

### 1.操作数据库

| 操作语法                                                     | 作用              |
| ------------------------------------------------------------ | ----------------- |
| show database;                                               | 查询所有数据库。  |
| select database();                                           | 查询当前数据库。  |
| use 数据库名;                                                | 使用/切换数据库。 |
| create database [if not exists] 数据库名 [default charset utf8mb4]; | 创建数据库。      |
| drop database [if exists] 数据库名;                          | 删除数据库。      |

### 2.创建表

- #### 代码结构

```mysql
create table tablename(
	字段1 字段类型 [约束] [comment 字段1注释],
	......
	字段2 字段类型 [约束] [comment 字段2注释]
)[comment 表注释];
```

- #### 字段类型

  **数值类型**

  | 类型      | 含义                                |
  | --------- | ----------------------------------- |
  | tinyint   | 整数，1字节。                       |
  | smallint  | 整数，2字节。                       |
  | mediumint | 整数，3字节。                       |
  | int       | 整数，4字节。                       |
  | bigint    | 整数，8字节。                       |
  | float     | 浮点数，4字节，近似值不能精确运算。 |
  | double    | 浮点数，8字节，近似值不能精确运算。 |
  | decimal   | 定点数，存精确小数（如金额）。      |

  **字符串类型**

  | 类型       | 含义                                           |
  | ---------- | ---------------------------------------------- |
  | char(M)    | 定长字符串，存固定长度的值。                   |
  | varchar(M) | 变长字符串，按需分配空间。                     |
  | text系列   | 大文本，用于长文章。                           |
  | blob系列   | 二进制大对象，用于存图片，音视频等二进制数据。 |
  | enum、set  | 单选、多选：特殊枚举。                         |

  **日期时间类型**

  | 类型      | 含义                                        |
  | --------- | ------------------------------------------- |
  | date      | 日期（YYYY-MM-DD）                          |
  | time      | 时间（HH:MM:SS）                            |
  | datetime  | 日期+时间，范围广，不受时区影响。           |
  | timestamp | 时间戳，范围到2038年，受时区影响，存UTC值。 |
  | year      | 年份（1字节）                               |

  **text和blob**

  | 子类型                | 最大容量                |
  | --------------------- | ----------------------- |
  | tinytext/tinyblob     | 255字节                 |
  | text/blob             | 65535字节（约64kb）     |
  | mediumtext/mediumblob | 16777215字节（约16MB）  |
  | longtext/longblob     | 4294967295字节（约4GB） |


- #### 约束类型

  | 约束     | 描述                                              | 关键字      |
  | -------- | ------------------------------------------------- | ----------- |
  | 非空约束 | 限制改字段值不能为null                            | not null    |
  | 唯一约束 | 保证字段的所有数据都是唯一，不重复的              | unique      |
  | 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一          | primary key |
  | 默认约束 | 保存数据时，如果未指定该字段值，则采用默认值      | default     |
  | 外键约束 | 让两张表的数据独立连接，保证数据的一致性和完整性  | foreign key |
  | 检查约束 | 限制值必须满足指定条件（MySQL8.0.16起才真正起效） | check       |

  ​


### 3.表结构的查询，修改，删除

| 语法                                                         | 含义                   |
| ------------------------------------------------------------ | ---------------------- |
| show tables;                                                 | 查询当前数据库的所有表 |
| desc 表名;                                                   | 查询表结构             |
| show create table 表名;                                      | 查询建表语句           |
| alter table 表名 add 字段名 类型 \[comment 注释] [约束];     | 添加字段               |
| alter table 表名 modify 字段名 新数据类型;                   | 修改字段类型           |
| alter table 表名 change 旧字段名 新字段名 类型 \[comment 注释] [约束]; | 修改字段名和字段类型   |
| alter table 表名 drop column 字段名;                         | 删除字段               |
| alter table 表名 rename to 新表名;                           | 修改表名               |
| drop table [if exists] 表名;                                 | 删除表                 |

## 三、DML

### 1.增加数据（insert）

| 语法                                                         | 含义                     |
| ------------------------------------------------------------ | ------------------------ |
| insert into 表名(字段名1, 字段名2) values (值1, 值2);        | 指定字段添加数据         |
| insert into 表名 values (值1，值2，......);                  | 全部字段添加数据         |
| insert into 表名(字段名1, 字段名2) values (值1, 值2),(值1, 值2); | 批量添加数据（指定字段） |
| insert into 表名 values (值1, 值2),(值1, 值2);               | 批量添加数据（全部字段） |

### 2.修改数据（update）

```SQL
update 表名 set 字段名1 = 值1 , 字段名2 = 值2, ...... [where 条件];
```

### 3.删除数据（delete）

```SQL
delete from 表名 [where 条件];
```

## 四、DQL

### 1.完整的DQL语法

```
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表
limit
	分页参数
```

- **基本查询（select...from...）**
- **条件查询（where）**
- **分组查询（group by）**
- **排序查询（order by）**
- **分页查询（limit）**

### 2.基本查询

| 语法                                                 | 含义                                 |
| ---------------------------------------------------- | ------------------------------------ |
| select 字段1,字段2,字段3 from 表名;                  | 查询多个字段                         |
| select * from 表名;                                  | 查询所有字段（通配符）               |
| select 字段1 [as 别名1], 字段2 [as 别名2] from 表名; | 为查询字段设置别名，as关键字可以省略 |
| select distinct 字段列表 from 表名;                  | 去除重复记录                         |

### 3.条件查询

```
select 字段列表 from 表名 where 条件列表;
```

**条件列表**

| 比较运算符          | 功能                                     |
| ------------------- | ---------------------------------------- |
| >                   | 大于                                     |
| \>=                 | 大于等于                                 |
| <                   | 小于                                     |
| <=                  | 小于等于                                 |
| =                   | 等于                                     |
| <> 或 !=            | 不等于                                   |
| between ... and ... | 在某个范围之内（含最小，最大值）         |
| in(......)          | 在in之后的列表中的值，多选一             |
| like 占位符         | 模糊匹配（_匹配单个字符，%匹配多个字符） |
| is null             | 是null                                   |

| 逻辑运算符 | 功能                         |
| ---------- | ---------------------------- |
| and 或 &&  | 并且（多个条件同时成立）     |
| or 或 \|\| | 或者（多个条件任意成立一个） |
| not 或 !   | 非，不是                     |

### 4.分组查询

```
select 字段列表 from 表名 [where 条件列表] group by 分组字段名 [having 分组后过滤条件];
```

**聚合函数**

| 函数  | 功能     |
| ----- | -------- |
| count | 统计数量 |
| max   | 最大值   |
| min   | 最小值   |
| avg   | 平均值   |
| sum   | 求和     |

- **where和having的区别**
- **执行时机不同**：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。
- **判断条件不同**：where不能对聚合函数进行判断，而having可以。


### 5.排序查询

```
select 字段列表 from 表名 [where 条件列表] [group by 分组字段名 having 分组后过滤条件] order by 排序字段 排序方式;
```

- 排序方式：升序（asc），降序（desc）；默认为升序asc，是可以不写的。

### 6.分页查询

```
select 字段 from 表名 [where 条件] [group by 分组字段 having 过滤条件] [order by 排序字段] limit 起始索引, 查询记录数;
```

- 起始索引从0开始。
- 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是limit。
- 如果起始索引为0，起始索引可以省略，直接写为limit 10。

### 7.多表关联

- **基本语法结构：**

```
select 字段列表 from 表1 join 表2 on 关联条件 join 表3 on 关联条件 where 过滤条件
```

- **各种join类型：**

| 语法       | 含义                                             |
| ---------- | ------------------------------------------------ |
| inner join | 内连接：只返回两表中匹配成功的记录（默认）       |
| left join  | 左外连接：返回左表所有记录，右表无匹配则显示null |
| right join | 右外连接：返回右表所有记录，左表无匹配则显示null |
| cross join | 笛卡尔积：返回两列表的笛卡尔积                   |
| self join  | 自连接：同一张表自己和自己关联，常用于层级数据   |

- **on与where的区别：**
- on作用于join的连接过程，决定两表如何匹配。
- where作用于join完成后的结果集，对最终结果进行过滤。

### 8.子查询

> **子查询是嵌套再另一个SQL语句中的select语句，可作为查询条件，数据源或字段值。**

- #### where子句中的子查询

  - 标量子查询（返回单值）：

    ```
    -- 查询工资高于平均工资的员工
    SELECT * FROM employees
    WHERE salary > (SELECT AVG(salary) FROM employees);
    ```

  - 列子查询（配合in/any/all):

    ```
    -- 查询下过订单的用户
    SELECT * FROM users
    WHERE id IN (SELECT DISTINCT user_id FROM orders);

    -- 大于任意一个（等价于大于最小值）
    SELECT * FROM products
    WHERE price > ANY (SELECT price FROM products WHERE category_id = 1);

    -- 大于所有（等价于大于最大值）
    SELECT * FROM products
    WHERE price > ALL (SELECT price FROM products WHERE category_id = 1);
    ```

  - exists子查询：

    ```
    -- 查询下过订单的用户
    SELECT * FROM users u
    WHERE EXISTS (
        SELECT 1 FROM orders o WHERE o.user_id = u.id
    );

    -- 查询没下过订单的用户
    SELECT * FROM users u
    WHERE NOT EXISTS (
        SELECT 1 FROM orders o WHERE o.user_id = u.id
    );
    ```


  - in和exists的区别：
  - in适合子查询结果集小的情况；
  - exists适合外查询结果集小，子查询大的情况，且遇到匹配即返回，效率高；


- #### from子句中的子查询（派生表）

  必须给子查询起别名：

  ```
  SELECT t.category_id, t.avg_price
  FROM (
      SELECT category_id, AVG(price) AS avg_price
      FROM products
      GROUP BY category_id
  ) AS t
  WHERE t.avg_price > 100;
  ```


- #### select子句中的子查询（标量）

  ```
  SELECT 
      u.name,
      (SELECT COUNT(*) FROM orders o WHERE o.user_id = u.id) AS order_count
  FROM users u;
  ```

  > 注意：SELECT 子查询必须只返回一行一列，否则报错


- #### having子句中的子查询

  ```
  SELECT category_id, AVG(price)
  FROM products
  GROUP BY category_id
  HAVING AVG(price) > (SELECT AVG(price) FROM products);
  ```

  ​