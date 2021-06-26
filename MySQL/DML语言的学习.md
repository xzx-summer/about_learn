# DML语言的学习

数据操作语言：  

插入：insert

修改：update

删除：delete



## 插入语句

```mysql
#插入语句

#一、经典插入
/*
语法：
INSERT INTO 表名(列名,…) VALUES(值1,…);
*/
#1、插入的值的类型要与列的类型一致或兼容
INSERT INTO beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'TANG','女','1990-4-23','188888',NULL,2);
#2、不可以为NULL的列必须插入值，可以为NULL的列如何插入值？ 
#方式一：可以在对应的值处写NULL
INSERT INTO beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'TANG','女','1990-4-23','188888',NULL,2);
#方式二：列名不写进来，对应的值也不写为默认值
INSERT INTO beauty(id,name,sex,borndate,phone，boyfriend_id)
VALUES(14,'JINGX','女','1990-4-23','1111111',9);
#3、列的顺序是否可以调换？ 可以
INSERT INTO beauty(name,sex,id,phone)
VALUES('JJJ','女',15,'110');
#4、列数和值的个数必须一致
#5、可以省略列名，默认所有列，而且列的顺序和表中列的顺序一致
INSERT INTO beauty
VALUES(18,'ZHANGFEIFIE','女',NULL,'119',NULL,NULL);

#二、
/*
语法：
INSERT INTO 表名
SET 列名=值,列名=值,…
*/
INSERT INTO beauty
SET id=19,`name`='LIUTT',phone='999';

#两种方式的比较
#1、方式一支持插入多行,方式二不支持
INSERT INTO beauty
VALUES (23,'tang1','女','1990-4-23','1234',NULL,2)
,(24,'tang2','女','1990-4-23','1234',NULL,2)
,(25,'tang3','女','1990-4-23','1234',NULL,2);
#2、方式一支持子查询，方式二不支持
INSERT INTO beauty(id,`name`,phone)
SELECT 26,'SONG','118';
```



## 修改语句

```mysql
#修改语句
/*
1、修改单表的记录
语法：
UPDATE 表名
SET 列=新值,列=新值,…
WHERE 筛选条件;

2、修改多表的记录
sql92语法：
UPDATE 表1 别名,表2 别名
SET 列=值,…
WHERE 连接条件
AND 筛选条件;
sql99语法：
UPDATE 表1 别名
INNER|LEFT|RIGHT JOIN 表2 别名
ON 连接条件
SET 列=值,…
WHERE 筛选条件;
*/

#修改的单表的记录
/*案例：修改beauty表中首字母为TA字段的电话为55555*/
UPDATE beauty SET phone=55555
WHERE `name` LIKE 'TA%';

#修改多表的记录
/*案例：修改张无忌的女朋友的手机号为114*/
UPDATE boys bo
INNER JOIN beauty b ON bo.id=b.boyfriend_id
SET b.phone='114'
WHERE bo.boyName='张无忌';
```



## 删除语句

```mysql
#删除语句
/*
方式一：DELETE
语法：
1、单表的删除
DELETE FROM 表名 WHERE 筛选条件
2、多表的删除
sql92语法：
DELETE 【表1的别名】,【表2的别名】
FROM 表1 别名,表2 别名
WHERE 连接条件
AND 筛选条件
sql99语法：
DELETE 【表1的别名】,【表2的表名】 
FROM 表1 别名
INNER|LEFT|RIGHT JOIN 表2 别名 ON 连接条件
WHERE 筛选条件 

方式二：TRUNCATE
语法：TRUNCATE TABLE 表名;
*/

#DELETE
#1、单表的删除
/*案例：删除手机号以9结尾的女神信息*/
DELETE FROM beauty WHERE phone LIKE '%9';
#2、多表的删除
/*案例：删除张无忌的女朋友的信息*/
DELETE b
FROM beauty b
INNER JOIN boys bo ON b.boyfriend_id = bo.id
WHERE bo.boyName='张无忌';
/*案例：删除黄晓明的信息以及他女朋友的信息*/
DELETE b,bo
FROM beauty b
INNER JOIN boys bo ON b.boyfriend_id=bo.id
WHERE bo.boyName='黄晓明';

#TRUNCATE
#清空数据，一删全删

#两种方式的区别
/*
1、DELETE可以加WHERE条件，TRUNCATE不能加
2、假如要删除的表中有自增长列，
如果使用DELETE删除后，再插入数据，自增长列的值从断点开始
而TRUNCATE删除后，再插入数据，自增长列的值从1开始
3、TRUNCATE删除，效率要一丢丢
4、TRUNCATE删除没有返回，DELETE删除有返回值
5、TRUNCATE删除不能回滚，DELETE删除可以回滚
*/
```

