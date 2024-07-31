### 一.SQL概述：

##### 1.什么是SQL？

：SQL（Structured Query Language）是“结构化查询语言”，它是对关系型数据库的操作语言。它可以应用到所有关系型数据库中，例如：MySQL、Oracle、SQL Server 等。



##### 2.语法要求?

* SQL 语句可以单行或多行书写，以分号结尾；
* 可以用空格和缩进来来增强语句的可读性；
* 关键字不区别大小写，建议使用大写；
  
  

##### 3.SQL分类

<mark>DDL（Data Definition Language）</mark>：数据定义语言，用来定义数据库对象：库、表、列等；
<mark>DML（Data Manipulation Language）</mark>：数据操作语言，用来定义数据库记录（数据）；
<mark>DCL（Data Control Language）：</mark>数据控制语言，用来定义访问权限和安全级别；
<mark>DQL（Data Query Language）</mark>：数据查询语言，用来查询记录（数据）

### 二.DDL（Data Definition Language）数据定义语言

<mark>基本操作：show databases（查看所有数据库）</mark>

<mark>切换数据库：use mydb1,切换到mydb1数据库</mark>



##### 1.操作数据库：



###### 1.创建数据库：

CREATE DATABASE mydb1:如果存在这个数据库，就会报错

CREATE DATABASE IF NOT EXISTS mydb1:在此数据库不存在时创建，避免报错



###### 2.删除数据库：

DROP DATABASE IF EXISTS mydb1：不存在也不会报错

DROP DATABASE  mydb1：不存在此数据库，报错



###### 3.修改数据库编码：

ALTER DATABASE mydb1 CHARACTER SET utf8：修改数据库 mydb1 的编码为 utf8

<mark>：在 MySQL 中所有的 UTF-8 编码都不能使用中间的`“-”`，即 UTF-8 要书写为 UTF8</mark>



##### 2.数据类型：

- int：整型

- double：浮点型，例如 double(5,2)表示最多 5 位，其中必须有 2 位小数，即最大值为 999.99；

- decimal：泛型型，在表单线方面使用该类型，因为不会出现精度缺失问题；

- char：固定长度字符串类型；(当输入的字符不够长度时会补空格)

- varchar：可变长度字符串类型；

- text：字符串类型；（存储大量文本数据，如文章或描述）

- blob：字节类型；（存储二进制数据，如图片与文件）

- date：日期类型，格式为：yyyy-MM-dd；

- time：时间类型，格式为：HH:MM:SS

- timestamp：时间戳类型；（存储日期与时间值，格式：YYYY-MM-DD HH:MM:SS）
  
  

##### 3.操作表：

###### 1.创建表：

```sql
CREATE TABLE 表名(
    列名 列类型, 
    列名 列类型,
    ...... 
);

```

```sql
CREATE TABLE stu(
    sid CHAR(6), 
    sname VARCHAR(20), 
    age INT, 
    gender VARCHAR(10)
);

```



###### 2.查看表结构：

<mark>：DESC 表名；</mark>



###### 3.删除表：

<mark>：DROP TABLE 表名；</mark>



###### 4.修改表：（以上文stu表为例）

添加列：`ALTER TABLE stu ADD (classname varchar(100));【给 stu 表添加 classname 列】

修改列的数据类型：ALTER TABLE stu MODIFY gender CHAR(2);【修改 stu 表的 gender 列类型为 CHAR(2)】

修改列名：ALTER TABLE stu change gender sex CHAR(2);【修改 stu 表的 gender 列名为 sex】

删除列：ALTER TABLE stu DROP classname;【删除 stu 表的 classname 列】

修改表名称：ALTER TABLE stu RENAME TO student;【修改 stu 表名称为 student】



### 三.DML（Data Manipulation Language）：数据操作语言:

##### 1.插入数据：

<mark>：语法1</mark>

**INSERT INTO 表名 (列名1，列名2，列名3)       <指定列名>**

```sql
INSERT INTO stu(sid, sname,age,gender) VALUES('s_1001', 'zhangSan', 23, 'male');
```

```sql
INSERT INTO stu(sid, sname) VALUES('s_1001', 'zhangSan');
```

<mark>：语法2</mark>

**INSERT INTO 表名 VALUES(值一，值二，.......)        <没有指定列名>**

```sql
INSERT INTO stu VALUES('s_1002', 'liSi', 32, 'female');
```



##### 2.修改数据：

**UPDATE 表名 SET 列名1=值1，...........，列名n=值n [WHERE 条件]**

```sql
UPDATE stu SET sname=’zhangSanSan’, age=’32’, gender=’female’ WHERE sid=’s_1001’;
UPDATE stu SET sname=’liSi’, age=’20’WHERE age>50 AND gender=’male’;
UPDATE stu SET sname=’wangWu’, age=’30’WHERE age>60 OR gender=’female’;
UPDATE stu SET gender=’female’WHERE gender IS NULL
UPDATE stu SET age=age+1 WHERE sname=’zhaoLiu’;
```



##### 3.删除数据：

<mark>：语法1</mark>

**DELETE FROM 表名 [WHERE 条件];**

```sql
DELETE FROM stu WHERE sid=’s_1001’003B;
DELETE FROM stu WHERE sname=’chenQi’ OR age > 30;
DELETE FROM stu;
```

<mark>：语法2</mark>

**TRUNCATE TABLE 表名;**

```sql
TRUNCATE TABLE stu;
```

<mark>两者之间的区别：</mark>
***虽然 TRUNCATE 和 DELETE 都可以删除表的所有记录，但有原理不同。DELETE的效率没有 TRUNCATE 高！
TRUNCATE 其实属性 DDL 语句，因为它是先 DROP TABLE，再 CREATE TABLE。
而且TRUNCATE删除的记录是无法回滚的，但DELETE删除的记录是可以回滚的（回滚是事务的知识！）。***

### 四.DCL（Data Control Language）：数据控制语言

##### 1.创建用户：

<mark>CREATE USER ‘用户名’@地址 IDENTIFIED BY '密码';</mark>

```sql
CREATE USER ‘user1’@localhost IDENTIFIED BY ‘123’;//‘local host’可以更换为其他主机名或IP地址
CREATE USER ‘user2’@’%’ IDENTIFIED BY ‘123’;//利用通配符，允许用户从任何主机连接
```



##### 2.给用户授权：

<mark>GRANT 权限 1, … , 权限 n ON 数据库.* TO ‘用户名’@地址;</mark>

```sql
GRANT CREATE,ALTER,DROP,INSERT,UPDATE,DELETE,SELECT ON mydb1.* TO 'user1'@localhost;
GRANT ALL ON mydb1.* TO user2@localhost;
```



##### 3.撤销授权：

<mark>REVOKE 权限 1, … , 权限 n ON 数据库.* FROM ‘用户名’@地址;</mark>

```sql
REVOKE CREATE,ALTER,DROP ON mydb1.* FROM 'user1'@localhost;
```



##### 4.查看用户权限：

<mark>SHOW GRANTS FOR ‘用户名’@地址;</mark>

```sql
SHOW GRANTS FOR 'user1'@localhost;
```



##### 5.删除用户：

<mark>DROP USER ‘用户名’@地址;</mark>

```sql
DROP USER ‘user1’@localhost;
```



##### 6.修改用户密码(以root身份)：

```sql
use mysql;
alter user '用户名'@localhost identified by '新密码';
```



### 五.DQL（Data Query Language）：数据查询语言<mark></mark>

<mark>：语法</mark>

**select 列名 ----> 要查询的列名称
from 表名 ----> 要查询的表名称
where 条件 ----> 行条件
group by 分组列 ----> 对结果分组
having 分组条件 ----> 分组后的行条件
order by 排序列 ----> 对结果分组
limit 起始行, 行数 ----> 结果限定**
<学生表 stu>   <雇员表 emp>  <部门表  dept>

##### 1.基础查询：

###### 查询所有列：

<mark>SELECT * FROM 表名;</mark>

```sql
SELECT * FROM stu;
```



###### 查询指定列：

<mark>SELECT 列名 1, 列名 2, …列名 n FROM 表名;</mark>

```sql
SELECT sid, sname, age FROM stu;
```



##### 2.条件查询：

<mark>：条件查询就是在查询时给出 WHERE 子句，在 WHERE 子句中可以使用如下运算符及关键字：</mark>

* <mark>=、!=、<>、<、<=、>、>=；</mark>
* <mark>BETWEEN…AND；</mark>
* <mark>IN(set)；</mark>
* <mark>IS NULL；</mark>
* <mark>AND；</mark>
* <mark>OR；</mark>
* <mark>NOT；</mark>

![](file:///C:/Users/%E6%99%93/Pictures/Saved%20Pictures/QQ20240729-155631.png)



##### 3.模糊查询：

<mark>SELECT 字段 FROM 表 WHERE 某字段 Like 条件</mark>

**其中关于条件，SQL 提供了两种匹配模式：**

1. **`%` ：表示任意 0 个或多个字符。可匹配任意类型和长度的字符，有些情况下若是中文，请使用两个百分号（%%）表示。**
2. **`_` ： 表示任意单个字符。匹配单个任意字符，它常用来限制表达式的字 符长度语句。**

![](file:///C:/Users/%E6%99%93/Pictures/Saved%20Pictures/QQ20240729-155834.png)



##### 4.字段控制查询：



###### 去掉重复记录

**<mark>SELECT DISTINCT sal FROM emp;</mark>**

*例如 emp 表中 sal 字段就存在相同的记录。当只查询 emp 表的 sal 字段时，那么会出现重复记录，那么想去除重复记录，需要使用 DISTINCT：*



###### 查看雇员的月薪与佣金之和

<mark>`SELECT *,`  
`sal+comm FROM emp;`</mark>

*因为 sal 和 comm 两列的类型都是数值类型，所以可以做加运算。如果 sal 或 comm 中有一个字段不是数值类型，那么会出错。*

<mark>SELECT *, sal+IFNULL(comm,0) FROM emp;</mark>

*comm 列有很多记录的值为 NULL，因为任何东西与 NULL 相加结果还是 NULL，所以结算结果可能会出现 NULL。下面使用了把 NULL 转换成数值 0 的函数 IFNULL：*



###### 给列名添加别名

<mark>SELECT *, sal+IFNULL(comm,0) AS total FROM emp;</mark>

*在上面查询中出现列名为 sal+IFNULL(comm,0)，这很不美观，现在我们给这一列给出一个别名，为 total：*

<mark>SELECT *, sal+IFNULL(comm,0) total FROM emp;</mark>

*给列起别名时，是可以省略 AS 关键字的：*



##### 5.排序：

![](file:///C:/Users/%E6%99%93/Pictures/Saved%20Pictures/QQ20240729-160530.png)



##### 6.聚合函数：

<mark>聚合函数是用来做纵向运算的函数：</mark>

<mark>COUNT()</mark>：统计指定列不为 NULL 的记录行数；
<mark>MAX()</mark>：计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算；
<mark>MIN()</mark>：计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算；
<mark>SUM()</mark>：计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为 0；
<mark>AVG()</mark>：计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为 0；

*<`IFNULL()` 是MySQL中的一个函数，用于检查两个参数：如果第一个参数不是 `NULL`，则返回第一个参数的值；如果第一个参数是 `NULL`，则返回第二个参数的值。这个函数在处理可能包含 `NULL` 值的数据时非常有用，可以确保查询结果的一致性。>*



###### 1.COUNT:

**例：**

**统计月薪与佣金之和大于 2500 元的人数？**

<mark>SELECT COUNT(*) AS cnt FROM emp WHERE sal+IFNULL(comm,0) > 2500;</mark>



###### 2.MAX  MIN:

**例：**

**查询最高工资和最低工资：**

<mark>SELECT MAX(sal), MIN(sal) FROM emp;</mark>



###### 3.SUM  AVG：

**（例：**

**统计所有员工平均工资：）**

<mark>SELECT AVG(sal) FROM emp;</mark>

<mark>SELECT SUM(sal), COUNT(sal) FROM emp;</mark>

**（例：**

**查询所有雇员月薪+佣金和：）**

<mark>SELECT SUM(sal+IFNULL(comm,0)) FROM emp;</mark>



##### 7.分组查询：

###### 1.GROUP BY子句

![](file:///C:/Users/%E6%99%93/Pictures/Saved%20Pictures/QQ20240730-084436.png)



###### 2.HAVING子句：

例：

查询工资总和大于 9000 的部门编号以及工资和：

```sql
SELECT deptno, SUM(sal)
FROM emp
GROUP BY deptno
HAVING SUM(sal) > 9000;

```

<mark>：WHERE 是对分组前记录的条件，如果某行记录没有满足 WHERE 子句的条件，那么这行记录不会参加分组；而 HAVING 是对分组后数据的约束。</mark>



##### 8.LIMIT：

<mark>：用来限定查询结果的起始行，以及总行数。</mark>

问：查询 5 行记录，起始行从 0 开始？ 
`SELECT * FROM emp LIMIT 0, 5;`  

<mark>注意，起始行从 0 开始，即第一行开始！</mark>

问：查询 10 行记录，起始行从 3 开始  
`SELECT * FROM emp LIMIT 3, 10;`



##### 9.多表连接查询：

<mark>：表连接分为内连接和外连接。</mark>

例：以下是员工表staff和职位表deptno

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e0bf72e9dfa5b37b10f006ac924f7083.png)



###### 内连接：

<mark>SELECT staff.name,deptname from staff,deptno WHRER staff.name=deptname;</mark>

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/940682d2dd405b0ef63bb3e7b6b3ec30.png)



###### 外连接(左)：

<mark>SELECT staff.name,deptname FROM staff LEFT JOIN deptno ON staff.name=deptno.name;</mark>

<mark>左连接：包含左边表中所有的记录，右边表中没有匹配的记录显示为 NULL。</mark>

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/be03e6df75790cb77c7182615ef96fe3.png)



###### 外连接(右)：

<mark>SELECT deptname,deptno.name FROM staff RIGHT JOIN deptno ON deptno.name=staff.name;</mark>

<mark>右连接：包含右边表中所有的记录，左边表中没有匹配的记录显示为 NULL。</mark>

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2654cb499226bcef74c5c62f436199e2.png)



### 六.补充基础语法：

##### 1.条件分支：CASE WHEN

**例：**

**假设有一个学生表 `student`，包含以下字段：`name`（姓名）、`age`（年龄）。请你编写一个 SQL 查询，将学生按照年龄划分为三个年龄等级（age_level）：60 岁以上为 "老同学"，20 岁以上（不包括 60 岁以上）为 "年轻"，20 岁以下为 "小同学"。返回结果应包含学生的姓名（name）和年龄等级（age_level），并按姓名升序排序。**

```sql
SELECT name,                           //这个逗号利于理解 ^_^
CASE WHEN (age>60) THEN '老同学'
WHEN (age>20) THEN '年轻'
ELSE '小同学' END AS age_level
FROM student
ORDER BY name ASC;
```



##### 2.字符串处理：

<mark>UPPER:转换为大写</mark>

<mark>LOWER:转换为小写</mark>

<mark>LENGTH:计算长度</mark>

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）。请你编写一个 SQL 查询，筛选出姓名为 '热dog' 的学生，展示其学号（id）、姓名（name）及其大写姓名（upper_name）。**

```sql
SELECT id,name,UPPER(name) AS upper_name
FROM student
WHERE name='热dog';
```



##### 3.关联查询:

###### 1.CROSS JOIN:

<mark>`CROSS JOIN` 是一种简单的关联查询，不需要任何条件来匹配行，它直接将左表的 **每一行** 与右表的 **每一行** 进行组合，返回的结果是两个表的笛卡尔积。</mark>

**例：**

**假设有一个学生表 `student` ，包含以下字段：id（学号）、name（姓名）、age（年龄）、class_id（班级编号）；还有一个班级表 `class` ，包含以下字段：id（班级编号）、name（班级名称）。**

**请你编写一个 SQL 查询，将学生表和班级表的所有行组合在一起，并返回学生姓名（student_name）、学生年龄（student_age）、班级编号（class_id）以及班级名称（class_name）。**

```sql
SELECT s.name AS student_name,
s.age AS student_age,
s.class_id AS class_id,
c.name AS class_name
FROM student s            //别名，简化语句
CROSS JOIN class c;       //别名，简化语句
```



###### 2.INNER JOIN:

<mark>INNER JOIN 只返回两个表中满足关联条件的交集部分，即在两个表中都存在的匹配行。</mark>

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）、`age`（年龄）、`class_id`（班级编号）。还有一个班级表 `class`，包含以下字段：`id`（班级编号）、`name`（班级名称）、`level`（班级级别）。**

**请你编写一个 SQL 查询，根据学生表和班级表之间的班级编号进行匹配，返回学生姓名（`student_name`）、学生年龄（`student_age`）、班级编号（`class_id`）、班级名称（`class_name`）、班级级别（`class_level`）。**

```sql
SELECT s.name student_name,
s.age student_age,
s.class_id class_id,
c.name class_name,
c.level class_level
FROM student s
INNER JOIN class c ON s.class_id = c.id;
```



###### 3.OUTER JOIN:

<mark>OUTER JOIN 是一种关联查询方式，它根据指定的关联条件，将两个表中满足条件的行组合在一起，并 **包含没有匹配的行** 。</mark>

<mark>:LEFT OUTER JOIN 和 RIGHT OUTER JOIN</mark>

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）、`age`（年龄）、`class_id`（班级编号）。还有一个班级表 `class`，包含以下字段：`id`（班级编号）、`name`（班级名称）、`level`（班级级别）。**

**请你编写一个 SQL 查询，根据学生表和班级表之间的班级编号进行匹配，返回学生姓名（`student_name`）、学生年龄（`student_age`）、班级编号（`class_id`）、班级名称（`class_name`）、班级级别（`class_level`），要求必须返回所有学生的信息（即使对应的班级编号不存在）。**

```sql
SELECT s.name student_name,
s.age student_age,
s.class_id class_id,
c.name class_name,
c.level class_level
FROM student s
LEFT OUTER JOIN class c ON s.class_id=c.id; 
```



##### 4.子查询：(**)

<mark>子查询是指在一个查询语句内部 **嵌套** 另一个完整的查询语句，内层查询被称为子查询。子查询可以用于获取更复杂的查询结果或者用于过滤数据。</mark>

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）、`age`（年龄）、`score`（分数）、`class_id`（班级编号）。还有一个班级表 `class`，包含以下字段：`id`（班级编号）、`name`（班级名称）。**

**请你编写一个 SQL 查询，使用子查询的方式来获取存在对应班级的学生的所有数据，返回学生姓名（`name`）、分数（`score`）、班级编号（`class_id`）字段。**

```sql
SELECT s.name, s.score, s.class_id 
FROM student s
WHERE s.class_id IN (
     SELECT c.id 
     FROM class c
);
```



##### 5.子查询-EXITS:

<mark>:子查询中的一种特殊类型是 "exists" 子查询，用于检查主查询的结果集是否存在满足条件的记录，它返回布尔值（True 或 False），而不返回实际的数据</mark>

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）、`age`（年龄）、`score`（分数）、`class_id`（班级编号）。还有一个班级表 `class`，包含以下字段：`id`（班级编号）、`name`（班级名称）。**

**请你编写一个 SQL 查询，使用 exists 子查询的方式来获取 不存在对应班级的 学生的所有数据，返回学生姓名（`name`）、年龄（`age`）、班级编号（`class_id`）字段。**

```sql
SELECT s.name,s.age,s.class_id
FROM student s
WHERE NOT EXISTS (
    SELECT 1
    FROM class c
    WHERE c.id=s.class_id
);
```



##### 6.组合查询：

1. <mark>UNION 操作：它用于将两个或多个查询的结果集合并， **并去除重复的行** 。即如果两个查询的结果有相同的行，则只保留一行。</mark>

2. <mark>UNION ALL 操作：它也用于将两个或多个查询的结果集合并， **但不去除重复的行** 。即如果两个查询的结果有相同的行，则全部保留。</mark>

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）、`age`（年龄）、`score`（分数）、`class_id`（班级编号）。还有一个新学生表 `student_new`，包含的字段和学生表完全一致。**

**请编写一条 SQL 语句，获取所有学生表和新学生表的学生姓名（`name`）、年龄（`age`）、分数（`score`）、班级编号（`class_id`）字段，要求保留重复的学生记录。**

```sql
SELECT s.name,s.age,s.score,s.class_id
FROM student s
UNION ALL
SELECT sn.name,sn.age,sn.score,sn.class_id
FROM student_new sn;
```



##### 7.开窗函数-sum over：

<mark>开窗函数是一种强大的查询工具，它允许我们在查询中进行对分组数据进行计算、 **同时保留原始行的详细信息** 。</mark>

```sql
SUM(计算字段名) OVER (PARTITION BY 分组字段名)
```

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）、`age`（年龄）、`score`（分数）、`class_id`（班级编号）。**

**请你编写一个 SQL 查询，返回每个学生的详细信息（字段顺序和原始表的字段顺序一致），并计算每个班级的学生平均分（class_avg_score）。**

```sql
SELECT id, name, age, score, class_id,
 AVG(score) OVER (PARTITION BY class_id) AS class_avg_score 
 FROM student;
```

<mark>开窗函数可以与聚合函数（如 SUM、AVG、COUNT 等）结合使用，但与普通聚合函数不同，开窗函数不会导致结果集的行数减少。</mark>



8.开窗函数-sum over order by

<mark>sum over order by，可以实现同组内数据的 **累加求和** 。</mark>

**例：**

**假设有一个学生表 `student`，包含以下字段：`id`（学号）、`name`（姓名）、`age`（年龄）、`score`（分数）、`class_id`（班级编号）。**

**请你编写一个 SQL 查询，返回每个学生的详细信息（字段顺序和原始表的字段顺序一致），并且按照分数升序的方式累加计算每个班级的学生总分（class_sum_score）。**

```sql
SELECT id, name, age, score, class_id, 
SUM(score) OVER (PARTITION BY class_id ORDER BY score ASC) AS class_sum_score 
FROM student;
```


