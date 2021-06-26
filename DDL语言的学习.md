# DDL语言的学习

数据定义语言

库和表的管理



一、库的管理

创建、修改、删除

二、表的管理

创建、修改、删除



创建：CREATE

修改：ALTER

删除：DROP



## 库和表的管理



### 库的管理

```mysql
#1、库的创建
/*
语法：
CREATE DATABASE (IF NOT EXISTS) 库名;
*/
/*案例：创建库Books*/
CREATE DATABASE IF NOT EXISTS books;

#2、库的修改
RENAME DATABASE books TO 新库名;#用不了
#更改库的字符集
ALTER DATABASE books CHARACTER SET gbk;

#3、库的删除
DROP DATABASE IF EXISTS books;
```



### 表的管理

```mysql
#1、表的创建
/*
语法：
CREATE TABLE 表名(
		列名 列的类型【(长度) 约束】,
		列名 列的类型【(长度) 约束】,
		列名 列的类型【(长度) 约束】,
		…
		列名 列的类型【(长度) 约束】
)
*/
/*案例：创建表book*/
CREATE TABLE IF NOT EXISTS book(
		id INT,
		bname VARCHAR(20),
		price DOUBLE,
		authorId INT,
		publishDate DATETIME
);

#2、表的修改
/*
语法：
ALTER TABLE 表名 ADD|DROP|MODIFY|CHANGE COLUMN 列名 【列类型 约束】;
*/
/*
①修改列名
ALTER TABLE 表名 CHANGE COLUMN 列名 新列名 列的类型;

②修改列的类型或约束
ALTER TABLE 表名 MODIFY COLUMN 列名 新的列的类型;

③添加新列
ALTER TABLE 表名 ADD COLUMN 新列列名 新列的类型;

④删除列
ALTER TABLE 表名 DROP COLUMN 列名;

⑤修改表名
ALTER TABLE 表名 RENAME TO 新表名
*/

#3、表的删除
/*
语法：
DROP TABLE IF EXISTS 表名;
*/

#通用的写法：
/*
DROP DATABASE IF EXISTS 旧库名;
CREATE DATABASE 新库名;

DROP TABLE IF EXISTS 旧表名;
CREATE TABLE 表名();
*/

#4、表的复制
/*
①仅仅复制表的结构
CREATE TABLE 新表 LIKE 要复制表的表名;

②复制表的结构+数据
CREATE TABLE 新表
SELECT * FROM 要复制表的表名;

③只复制部分数据
CREATE TABLE 新表
SELECT 要复制的列名1,要复制的列名2,…
FROM 要复制表的表名
WHERE 复制行的筛选条件;

④仅仅复制某些字段，不要数据
CREATE TABLE 新表
SELECT 要复制的列名1,要复制的列名2,…
FROM 要复制表的表名
WHERE 1=2;
*/
```



## 常见数据类型介绍

数值型：
​	整型
​	小数：
​			定点数
​			浮点数

![image-20210220152720909](https://github.com/xzx-summer/image.git/img/image-20210220152720909.png)

![image-20210220154638947](https://github.com/xzx-summer/image.git/img/image-20210220154638947.png)

字符型：
​	较短的文本：char、varchar
​	较长的文本：text、blob（较长的二进制数据）

![image-20210220172139355](https://github.com/xzx-summer/image.git/img/image-20210220172139355.png)

![image-20210220172825345](https://github.com/xzx-summer/image.git/img/image-20210220172825345.png)

![image-20210220172917661](https://github.com/xzx-summer/image.git/img/image-20210220172917661.png)

![image-20210220172944100](https://github.com/xzx-summer/image.git/img/image-20210220172944100.png)

![image-20210220211730056](https://github.com/xzx-summer/image.git/img/image-20210220211730056.png)

日期型：

![image-20210220212127593](https://github.com/xzx-summer/image.git/img/image-20210220212127593.png)

![image-20210220212949972](https://github.com/xzx-summer/image.git/img/image-20210220212949972.png)

```mysql
#整型
/*
分类：
TINYINT、SMALLINT、MEDIUMINT、INT/INTEGER、BIGINT

特点：
①如果不设置无符号还是有符号，默认是有符号，如果像设置无符号，需要添加UNSIGNED关键字
②如果插入的数值超出了整形的范围，会报out if range异常，并插入临界值
③如果不设置长度，会有默认的长度
长度代表了显示的最大宽度，如果不够会用0在左边填充，但必须搭配ZEROFILL（只能用于无符号数）使用！
*/
#如何设置无符号和有符号
CREATE TABLE tab_int(
		t1 INT,
		t2 INT UNSIGNED
);

#小数
/*
分类：
1、浮点型
FLOAT(M,D)
DOUBLE(M,D)
2、定点型
DEC(M,D)
DECIMAL(M,D)

特点：
①M：整数部分+小数部位
D：小数部位
如果超过范围，则插入临界值
②M和D都可以省略
如果是DECIMAL，则M默认为10，D默认为0
如果是FLOAT和DOUBLE，则会根据插入的数值的精度来决定精度
③定点型的精确度较高，如果要求插入数值的精度较高如货币运算等则考虑使用
*/

#原则
/*
所选择的类型越简单越好，能保存数值的类型越小越好
*/

#字符型
/*
较短的文本：CHAR、VARCHAR
较长的文本：TEXT、BLOB（较大的二进制）
其他：BINARY和VARBINARY用于保存较短的二进制
	ENUM用于保存枚举
	SET用于保存集合

特点：
		写法	M的意思	特点	空间的耗费	效率
CHAR	CHAR(M)	最大的字符数，可以省略，默认为1	固定长度的字符	比较耗费	高
VARCHAR	VARCHAR(M)	最大的字符数，不可以省略	可变长度的字符	比较节省	低
*/

#日期型
/*
分类：
DATE 只保存日期
TIME 只保存时间
YEAR 只保存年
DATETIME 保存日期+时间
TIMESTAMP 保存日期+时间

特点：
		字节	范围	时区等的影响
DATETIME	8	1000-9999	不受
TIMESTAMP	4	1970-2038	受
*/
```



## 常见约束

```mysql
#常见约束
/*
含义：一种限制，用于限制表中的数据，为了保证表中数据的准确和可靠性

分类：六大约束
		NOT NULL:非空，用于保证该字段的值不能为空，如姓名、学号等
		DEFAULT:默认，用于保证该字段有默认值，如性别
		PRIMARY KEY:主键，用于保证该字段的值具有唯一性，并且非空，如学号、工号等
		UNIQUE:唯一，用于保证该字段的值具有唯一性，可以为空，如座位号
		CHECK:检查约束【mysql中不支持】，如年龄、性别
		FOREIGN KEY:外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值，意思是在从表添加外键约束，用于引用主表中某列的值，如学生表的专业编号、员工表的部门编号、员工表的工种编号
		
添加约束的时机：
	1、创建表时
	2、修改表时
	
约束的添加分类：
	列级约束：
		位置在列的后面，六大约束语法上都支持，但外键约束没有效果，不可以起约束名
	表级约束：
		位置在所有列的下面，除了非空、默认其他的都支持，可以起约束名，但主键没有效果
		
主键和唯一的对比：
		保证唯一性	是否允许为空	一个表中可以有多少个	是否允许组合
主键	      √			×	          至多有一个	      √，但不推荐
唯一		  √ 		√		      可以有多个	      √，但不推荐

外键：
	1、要求在从表设置外键关系
	2、从表的外键列的类型和主表的关联列的类型要求一致或兼容，名称无要求
	3、主表的关联列必须是一个KEY（一般是主键、唯一）
	4、插入数据时，先插入主表，再插入从表；删除数据时，先删除从表，再删除主表
*/

#一、创建表时添加约束
#添加列级约束
/*
语法：
直接在字段名和类型后面追加约束类型即可
只支持：默认、非空、主键、唯一
*/
CREATE students;
USE students;
CREATE TABLE stuinfo(
	id INT PRIMARY KEY,#主键
	stuName VARCHAR(20) NOT NULL,#非空
	gender CHAR(1) CHECK(gender = '男' or gender = '女'),#检查
	seat INT UNIQUE,#唯一
	age INT DEFAULT 18,#默认
	majorId INT REFERENCES major(id)#外键
);
CREATE TABLE major(
	id INT PRIMARY KEY,
	majorName VARCHAR(20)
);
SHOW INDEX FROM stuinfo;#查看表中所有的索引，包括主键、外键、唯一
#添加表级约束
/*
语法：在各个字段的最下面
【CONSTRAINT 约束名】 约束类型(字段名)
*/
DROP TABLE IF EXISTS stuinfo;
CREATE TABLE stuinfo(
	id INT,
	seat INT,
	gender CHAR(1),
	majorId INT,
	
	CONSTRAINT pk PRIMARY KEY(id),#主键
	CONSTRAINT uq UNIQUE(seat),#唯一
	CONSTRAINT ck CHECK(gender = '男' OR gender = '女'),#检查
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorId) REFERENCES major(id)#外键
);
#通用的写法：
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT PRIMARY KEY,
	stuname VARCHAR(20) NOT NULL,
	sex CHAR(1),
	age INT DEFAULT 18,
	seat INT UNIQUE,
	majorid INT,
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)
);

#二、修改表时添加约束
/*
1、添加列级约束
ALTER TABLE 表名 MODIFY COLUMN 字段名 字段类型 新约束;
2、添加表级约束
ALTER TABLE 表名 ADD 【CONSTRAINT 约束名】 约束类型(字段名) 【外键的引用】;
*/
DROP TABLE IF EXISTS stuinfo;
CREATE TABLE stuinfo(
	id INT,
	stuName VARCHAR(20),
	gender CHAR(1) CHECK,
	seat INT,
	age INT,
	majorId INT
);
#添加非空约束
ALTER TABLE stuinfo MODIFY COLUMN stuName VARCHAR(20) NOT NULL;
#添加默认约束
ALTER TABLE stuinfo MODIFY COLUMN age INT DEFAULT 18;
#添加主键
#①列级约束
ALTER TABLE stuinfo MODIFY COLUMN id INT PRIMARY KEY;
#②表级约束
ALTER TABLE stuinfo ADD PRIMARY KEY(id);
#添加唯一
#①列级约束
ALTER TABLE stuinfo MODIFY COLUMN seat INT UNIQUE;
#②表级约束
ALTER TABLE stuinfo ADD UNIQUE(seat);
#添加外键
ALTER TABLE stuinfo ADD CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorId) REFERENCES major(id);

#三、修改表时删除约束
#删除非空约束
ALTER TABLE stuinfo MODIFY COLUMN stuName VARCHAR(20) NULL;
#删除默认约束
ALTER TABLE stuinfo MODIFY COLUMN age INT;
#删除主键
ALTER TABLE stuinfo DROP PRIMARY KEY;
#删除唯一
ALTER TABLE stuinfo DROP INDEX seat;
#删除外键
ALTER TABLE stuinfo DROP FOREIGN KEY fk_stuinfo_major;
```



## 标识列

```mysql
#标识列
/*
又称为自增长列
含义：可以不用手动的插入值，系统提供默认的序列值

特点：
1、标识列必须和主键搭配吗？不一定，但要求是一个KEY
2、一个表中至多有一个标识列
3、标识列的类型只能是数值型
4、标识列可以通过SET AUTO_INCREMENT_INCREMENT=3;设置步长；可以通过手动插入值，设置起始值
*/

#一、创建表时设置标识列
CREATE TABLE tab_identity(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20)
);
INSERT INTO tab_identity VALUES(NULL,'JJJJJ');
SET AUTO_INCREMENT_INCREMENT=3;#设置自增长列每次自增多少

#二、修改表时设置标识列
ALTER TABLE tab_identity MODIFY COLUMN id INT PRIMARY KEY AUTO_INCREMENT;

#三、修改表时删除标识列
ALTER TABLE tab_identity MODIFY COLUMN id INT;
```



