#### 1.select in 操作：

in操作符可以允许在where 子句中规定多个值：

SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2...)

Eg: SELECT * FROM person WHERE Lastname IN ('Yuki','Shirley');



SQL语句选择位于“Germany”，“France”和“UK”的所有客户

```SQL
SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');
```



#### 2.SQL BETWEEN 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
```

例子：

包含：SELECT * FROM person WHERE Lastname BETWEEN 'Yuki' AND 'Shirley'

不包含：SELECT * FROM person WHERE Lastname NOT BETWEEN 'Yuki' AND 'Shirley'



#### 3.Alias （别名）的用法：

```sql
SELECT po.OrderID, p.LastName, p.FirstName
FROM Persons AS p, Product_Orders AS po
WHERE p.LastName='Adams' AND p.FirstName='John'
```



#### 4.Join，inner join ， left join, right join 和 full join

注释：left 和right 分别是对应左右的两个表为主表。

```sql
SELECT column_name(s)
FROM table_name1
INNER JOIN / left join /right join table_name2 
ON table_name1.column_name=table_name2.column_name
```



#### 5.union连接

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

**请注意1**，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。

**请注意2**，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

**请注意3**，UNION ALL和UNION区别在于，前者会列出所有的，后者会去掉重复的

```sql
SELECT column_name(s) FROM table_name1
UNION / UNION ALL
SELECT column_name(s) FROM table_name2
```



#### 6.select into

SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表中。

SELECT INTO 语句常用于创建表的备份复件或者用于对记录进行存档。

注释：IN 子句可用于向另一个数据库中拷贝表：另一个数据库为：mdb

```sql
SELECT *
INTO Persons IN 'Backup.mdb'
FROM Persons
```



#### 7.SQL函数--group by

GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组。

列如：

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
```

```sql
SELECT Customer,OrderDate,SUM(OrderPrice) FROM Orders
GROUP BY Customer,OrderDate
```



#### 8.SQL常见面试题：

一：

1.用一条SQL 语句 查询出每门课都大于80 分的学生姓名

name   kecheng   fenshu
张三    语文       81
张三     数学       75
李四     语文       76
李四     数学       90
王五     语文       81
王五     数学       100
王五     英语       90

**答案：**

```sql
SELECT name FTROM student GROUP BY name HAVING min(fenshu)>80;
```

**或者** 

```sql
select distinct name from student where name not in (select distinct name from student where fenshu <=80)
```

思路是:先找到分数低于80分一下的学生并去重，然后查询出不包含这个范围的学生。



二：学生表 如下:
自动编号   学号   姓名 课程编号 课程名称 分数
1        2005001 张三 0001     数学    69
2        2005002 李四 0001      数学    89
3        2005001 张三 0001      数学    69
删除除了自动编号不同, 其他都相同的学生冗余信息

**答案：**

```SQL
delete from student where 自动编号 not in (select min(自动编号) from student group by 学生，姓名，编程)
```



三：请用SQL 语句实现：从TestDB 数据表中查询出所有月份的发生额都比101 科目相应月份的发生额高的科目。请注意：TestDB 中有很多科目，都有1 －12 月份的发生额。
AccID ：科目代码，Occmonth ：发生额月份，DebitOccur ：发生额。
数据库名：JcyAudit ，数据集：Select * from TestDB

**答案：**

```sql
select a.* from TestDB a, (select Occmonth, max(DebitOccur) as DebitOccur101 from TestDB where AccID='101', group by Occmonth) b where a.Occmonth=b.Occmonth and a.DebitOccur > b.DebitOccur101
```



四：怎么把这样一个表儿
year   month amount
1991   1     1.1
1991   2     1.2
1991   3     1.3
1991   4     1.4
1992   1     2.1
1992   2     2.2
1992   3     2.3
1992   4     2.4
查成这样一个结果
year m1   m2   m3   m4
1991 1.1 1.2 1.3 1.4
1992 2.1 2.2 2.3 2.4 

**答案：**

```sql
select year,
(select amount from   aaa a where month=1   and a.year=aaa.year) as m1,
(select amount from   aaa a where month=2   and a.year=aaa.year) as m2,
(select amount from   aaa a where month=3   and a.year=aaa.year) as m3,
(select amount from   aaa a where month=4   and a.year=aaa.year) as m4
from aaa   group by year
```



五：原表:
courseid coursename score
1 java 70
2 oracle 90
3 xml 40
4 jsp 30
5 servlet 80

为了便于阅读, 查询此表后的结果显式如下( 及格分数为60):
courseid coursename score mark
1 java 70 pass
2 oracle 90 pass
3 xml 40 fail
4 jsp 30 fail
5 servlet 80 pass

**答案：**


```sql
select courseid,coursename,score, decode(sign(score-60),-1,'fail','pass')as mark from  course
```



六：create table testtable1
(
id int IDENTITY,
department varchar(12) 
)

select * from testtable1
insert into testtable1 values('设计')
insert into testtable1 values('市场')
insert into testtable1 values('售后')
/*
结果
id department
1   设计
2   市场
3   售后 
*/
create table testtable2
(
id int IDENTITY,
dptID int,
name varchar(12)
)
insert into testtable2 values(1,'张三')
insert into testtable2 values(1,'李四')
insert into testtable2 values(2,'王五')
insert into testtable2 values(3,'彭六')
insert into testtable2 values(4,'陈七')
/*
用一条SQL语句，怎么显示如下结果
id dptID department name
1   1      设计        张三
2   1      设计        李四
3   2      市场        王五
4   3      售后        彭六
5   4      黑人        陈七
*/

**答案：**

```sql
select testtable2.*, isNUll('department',0) from testtable1 t1 right join testtable2 t2 on  t1.id = t2.dptID
```



七：

有表A，结构如下： 
A: p_ID p_Num s_id 
1 10 01 
1 12 02 
2 8 01 
3 11 01 
3 8 03 
其中：p_ID为产品ID，p_Num为产品库存量，s_id为仓库ID。请用SQL语句实现将上表中的数据合并，合并后的数据为：
p_ID s1_id s2_id s3_id 
1 10 12 0 
2 8 0 0 
3 11 0 8 
其中：s1_id为仓库1的库存量，s2_id为仓库2的库存量，s3_id为仓库3的库存量。如果该产品在某仓库中无库存量，那么就是0代替。

**答案：**

```sql
select p_id ,
sum(case when s_id=1 then p_Num elese 0 end) as s1_id
,sum(case when s_id=2 then p_num else 0 end) as s2_id
,sum(case when s_id=3 then p_num else 0 end) as s3_id
from myPro group by p_id
```



八：删除相关联的表

**问题一：**

有两个表：group_file和teach_classroom，其中的group_file.group_id和teach_classroom.classroom_id对应唯一一条语句删除两个表的内容：

```sql
select a.*,b.classroom_name from group_file a,teach_classroom b where a.group_id=b.classroom_id;

delete a,b from group_file as a left join teach_classroom as b on a.group_id=b.classroom_id where b.classroom_id="classroom_20180508200911";
```

**问题二：**（面试题：土巴兔）

价格ks数据库有以下三张表，请写出下列问题的SQL语句：

学生（学号，姓名，年龄，性别）

课程（课程号，课程名，任课老师）

成绩（学号，课程号，成绩）

(1)查询年龄大于20岁的所有男同学的学号、姓名

```sql
select 学号，姓名 from 学生 where 年龄>20;
```



(2)删除三张表中所有学号为20020001的学生信息

```sql
delete 学生.*,成绩.* from 学生，成绩 where 学号=20020001
```



(3)把学号为20030002的学生的年龄改为22岁

```sql
update 学生 set 年龄=22 where 学号=20030002
```



九：更新相关联的表

```sql
update 表名 set(字段1,字段2,字段3) = (select 数值1,数值2,数值3 from dual) where 条件
```

实例：SQLServer批量更新两个关联表数据的方法。

分享给大家供大家参考，具体如下： 

方式1：

```sql 
UPDATE a SET WtNo=b.NO from WT_Task a INNER JOIN WT_BasicInformation b ON a.WtId=b.ID; 
```

方式2： 

```sql
UPDATE a SET a.WtNo=b.NO FROM WT_Task a,WT_BasicInformation b WHERE a.WtId=b.ID;
```



十：查询总数：查询出该俱乐部里男性和女性会员总数

sub表

| id | gender | age |
| :---| :--- | :---- |
| 23 | F | 24 |
| 24 | M | 30 |
|  25   |  M|27|
| 26 | F |33|
| 27 | F |28|

Count:返回指定条件的行数

```sql
select count(*) from sub where gender="F"
```



十一：alter 语句

ALTER TABLE 语句用于在已有的表中添加、修改或删除列。

```sql
ALTER TABLE table_name
ADD column_name datatype
```



我们希望在表 "Persons" 中添加一个名为 "Birthday" 的新列。

我们使用下列 SQL 语句：

```sql
alter table Persons add Birthday date
```

