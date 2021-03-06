---
layout:     note
title:      MySQL
description: 关系型数据库MySQL
---

* 目录(this text will be scraped).
{:toc}

#SQL语句

SQL对于大小写不敏感。

###执行顺序

SQL是一种声明式的语言，描述的是对结果的要求，而非执行步骤。语句的执行顺序和语法顺序并不一致，因此使用自定义变量要注意声明顺序的问题。比如一个`FROM`最先执行，下面是几个常用语句的执行顺序。

+ FROM 
+ WHERE
+ GROUP BY
+ HAVING
+ SELECT
+ DISTINCT
+ UNION
+ ORDER BY




###DML & DDL

DML数据操作语言和DDL数据定义语言，DML用于执行数据的CURD，而DDL用于创建关系，约束等。常用的DDL有

+ CREATE DATABASE
+ ALTER DATABASE
+ CREATE TABLE
+ ALTER TABLE
+ DROP TABLE
+ CREATE INDEX
+ DROP INDEX

###CURD

#####insert

    INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing')
    INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')

向Person表中插入新的数据，可以直接插入，也可以按照字段名字插入.


#####update

    UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'
    WHERE LastName = 'Wilson'

修改表中的某一行的几个字段

#####delete

    DELETE FROM Person WHERE LastName = 'Wilson' 

删除表中满足条件的元素

#####select

    SELECT [DISTINCT] first_name, last_name FROM employees WHERE salary > 100000

上面语句声明了，从`employees`表中选择`salary`大于100000的元组，获得其`first_name`和`last_name`属性。**DISTINCT**可以去除结果中的重复元素。`WHERE`语句后面跟着一个布尔表达式。不同的布尔表达式可以使用`AND`和`OR`连接形成组合。表达式可用的操作符包括:
    
    = <> > < >= <= BETWEEN LIKE

FROM语句输出的是一张联合表，假如a 有3个字段，b有5个字段，则输出的联合表有8(=5+3)个字段。联合表的数据就是a和b的笛卡尔乘积，数量为15（=3*5）.

    ... FROM a,b


####更多查询
查询操作中和**SELECT**配合的语句也很关键.

#####ORDER BY
对结果集进行排序，例如对结果根据年龄降序：

    ... ORDER BY age DESC 

####GROUP BY
将数据集**划分**，对每个数据集操作,和聚合函数(如MAX,COUNT,AVG,SUM)合作使用.例如统计不同消费者的个人订单和，GROUP BY关键是分组，这里的意义是让SUM函数作用在属于同一个Customer的orderPrice上。

    SELECT Customer,SUM(OrderPrice) FROM Orders
    GROUP BY Customer

#####HAVING
筛选GROUP BY 分组后的数据,延续上面的例子，增加HAVING对分组后的数据筛选，要求显示订单总和大于1000的客户,这里不能使用WHERE，原因在于WHERE在GROUP BY之前执行。

    SELECT Customer,SUM(OrderPrice) FROM Orders
    GROUP BY Customer
    HAVING SUM(OrderPrice) > 1000

#####UNION
合并多个SELECT的结果集,要求合并的结果集是相同数据种类，默认**并集**操作,UNION ALL操作可以保留重复项。

    SELECT column_name(s) FROM table_name1
    UNION
    SELECT column_name(s) FROM table_name2


###JOIN

JOIN可以根据多个表中列之间的关系来联合查询数据，JION并不是SELECT的一部分，只是构建连接表的关键词

    FROM a1 JOIN a2 ON a1.id = a2.id

JOIN方式共有5种：

+ EQUI JOIN
+ SEMI JOIN
+ ANTI JOIN
+ CROSS JOIN
+ DIVISION


#####EQUI JION

最常见的JOIN方式，包括INNER JOIN（或者JOIN） 和 OUTER JOIN（包括LEFT、RIGHT、FULL OUTER JOIN,不同的JOIN方式可以用一张图来解释：

![](https://raw.github.com/leeon/TechPics/master/database/mysql/SQL-Joins.png)

#####SEMI JOIN

使用形式有两种:IN和EXISTS,例如查找拥有图书的作者(普通JOIN也可以做到，但是没有必要)

    -- Using IN
    FROM author
    WHERE author.id IN (SELECT book.author_id FROM book)
     
    -- Using EXISTS
    FROM author
    WHERE EXISTS (SELECT 1 FROM book WHERE book.author_id = author.id)

#####ANTI JOIN

与SEMI JOIN相反，使用NOT IN 和 NOT EXISTS,这次查找没有图书的作者

    -- Using IN
    FROM author
    WHERE author.id NOT IN (SELECT book.author_id FROM book)
     
    -- Using EXISTS
    FROM author
    WHERE NOT EXISTS (SELECT 1 FROM book WHERE book.author_id = author.id)

#####CROSS JOIN

对连接的表做乘积，效果和`FROM a,b`一样，用处较少

#####DIVISION

too hard for me now...



