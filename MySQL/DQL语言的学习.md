# DQL语言的学习



## 基础查询

```mysql
#基础查询
/*
语法：
SELECT 查询列表 FROM 表名;

 特点：
1.查询列表可以是：表中的字段、常量值、表达式、函数
2.查询的结果是一个虚拟的表格
*/

USE myemployees;
#1.查询表中的单个字段
SELECT
	last_name 
FROM
	employees;
	
#2.查询表中的多个字段
SELECT
	last_name,
	salary,
	email 
FROM
	employees;
	
#3.查询表中的所有字段
SELECT * FROM employees;/*此时查询顺序不会变，想变化顺序将字段一个一个列出来就会根据列出来的顺序进行查询，例：SELECT `employee_id`,`first_name`,`hiredate` FROM employees;*/
/*注意：``是着重号，也就是键盘1旁边的那个，当字段名与关键字重名时可以使用``将字段名标出来*/

#4.查询常量值
SELECT 100;
SELECT 'john';
/*字符型和日期型的常量值必须用单引号引起来，数值型不需要*/

#5.查询表达式
SELECT 100%98;

#6.查询函数
SELECT VERSION();

#7.起别名
/*
①便于理解
②如果要查询的字段有重名的情况，使用别名可以区分开来
*/
#方式一：使用AS
SELECT 100%98 AS 结果;
SELECT last_name AS 姓,first_name AS 名 FROM employees;
#方式二：使用空格
SELECT last_name 姓,first_name 名 FROM employees;
/*特例 案例：查询salary,显示结果为out put*/
#SELECT salary AS out put FROM employees;-->别名有特殊字符空格，错了
SELECT salary AS "out put" FROM employees;#建议加上双引号，也可以加上单引号

#8.去重
/*案例：查询员工表中涉及到的所有的部门编号*/
SELECT DISTINCT department_id FROM employees;#只能去重一个字段

#9.+号的作用
/*
MySQL中的+号只有一个功能：运算符

SELECT 100+90;#两个操作数都为数值型，则做加法运算
SELECT '123'+90;#其中一方为字符型，试图将字符型数值转换成数值型，如果转换成功则继续做加法运算；
SELECT 'john'+90;#如果转换失败则将字符型数值转换成0
SELECT null+10;#只要其中一方为null，则结果肯定为null
*/
/*案例：查询员工的名和姓连接成一个字段，并显示为 姓名*/
SELECT last_name+first_name AS 姓名 FROM employees;#错误
#使用CONCAT函数
SELECT CONCAT('a','b','c') AS 结果;
SELECT CONCAT(last_name,first_name) AS 姓名 FROM employees; 

#10.IFNULL函数
/*功能：
		判断某字段或表达式是否为NULL，如果为NULL返回指定的值，否则返回原本的值
*/
SELECT IFNULL(commission_pct,0) FROM employees;

#11.ISNULL函数
/*功能：
		判断某字段或表达式是否为NULL，如果是则返回1，否则返回0
*/
SELECT ISNULL(commission_pct),commission_pct FROM employees;

```



## 条件查询

```mysql
#条件查询
/*
语法：
SELECT 查询列表 FROM 表名 WHERE 筛选条件;
#先查看是否有表，然后筛选，最后查询列表

分类：
1. 按条件表达式筛选
条件运算符：> < = != <> >= <=
2. 按逻辑表达式筛选
逻辑运算符（作用：用于连接条件表达式）：
&& || ！
AND OR NOT 
&&和AND：两个条件都为true，结果为true，反之为false
||和OR：只要有一个条件为true，结果为true，反之为false
！和NOT：如果连接的条件本身为false，结果为true，反之为false
3.模糊查询
LIKE 	
BETWEEN AND
IN
IS NULL/IS NOT NULL 
*/

#1.按条件表达式筛选
SELECT * FROM employees WHERE salary>12000;
SELECT last_name,department_id FROM employees WHERE department_id<>90;

#2.按逻辑表达式筛选
SELECT
	last_name,
	salary,
	commission_pct 
FROM
	employees 
WHERE
	salary >= 10000 
	AND salary <= 20000;
SELECT
	* 
FROM
	employees
WHERE 
	NOT ( department_id >= 90 AND department_id <= 110 ) OR salary > 15000;

#3.模糊查询
/*
LIKE
特点：
①一般和通配符搭配使用，可以判断字符型或数值型
②通配符：%任意多个字符，_任意单个字符

BETWEEN AND
①使用BETWEEM AND 可以提高语句的简洁度
②包含临界值
③两个临界值不能调换顺序

IN
含义：判断某字段的值是否属于IN列表中的某一项
特点：
	①使用IN提高语句简洁度
	②IN列表的值类型必须一致或兼容
	
IS NULL/IS NOT NULL
=或<>不能用于判断NULL值
IS NULL和IS NOT NULL可以判断NULL值
*/
#①LIKE
/*案例：查询员工名中包含字符a的员工信息*/
SELECT * FROM employees WHERE last_name LIKE '%a%';
/*案例：查询员工名中第三个字符为n，第五个字符为l的员工名和工资*/
SELECT last_name,salary FROM employees WHERE last_name LIKE '___n_l%';
/*案例：查询员工名中第二个字符为_的员工名*/
SELECT last_name FROM employees WHERE last_name LIKE '_\_%';#或LIKE '_$_%' ESCAPE '$';
#
#②BETWEEN AND
/*案例：查询员工编号再100到120之间的员工信息*/
SELECT * FROM employees WHERE employee_id BETWEEN 100 AND 120;
#
#③IN
/*案例：查询员工的工种编号是IT_PROG、AD_VP、AD_PRES中的一个员工名和工种编号*/
SELECT last_name,job_id FROM employees WHERE job_id IN('IT_PROG','AD_VP','AD_PRES');
#
#④IS NULL/IS NOT NULL 
/*案例：查询没有奖金的员工名和奖金率*/
SELECT last_name,commission_pct FROM employees WHERE commission_pct IS NULL;
/*案例：查询有奖金的员工名和奖金率*/
SELECT last_name,commission_pct FROM employees WHERE commission_pct IS NOT NULL;

#安全等于:<=>
/*案例：查询没有奖金的员工名和奖金率*/
SELECT last_name,commission_pct FROM employees WHERE commission_pct <=> NULL;
SELECT last_name,commission_pct FROM employees WHERE salary <=> 12000;
/*IS NULL:仅仅可以判断NULL值，可读性较高，建议使用
	<=>	   :既可以判断NULL值，又可以判断普通的数值，可读性较低	*/
	
#经典问题
SELECT * FROM employees;
SELECT * FROM employees WHERE commission_pct LIKE '%%' AND last_name LIKE '%%';
/*二者结果不一样，如果判断的字段有NULL值*/ 
```



## 排序查询

```mysql
#排序查询
/*
引入：对查询的结果进行排序显示

语法：
SELECT 查询列表
FROM 表
[WHRER 筛选条件]
ORDER BY 排序列表 [ASC/DESC]

特点：
	1.ASC代表的是升序，DESC代表的是降序，如果不写默认是升序
	2.ORDER BY子句中可以支持单个字段、多个字段、表达式、函数、别名
	3.ORDER BY子句一般是放在查询语句的最后面，但LIMIT子句除外
*/
/*案例：查询员工信息，要求工资从高到低排序*/
SELECT * FROM employees ORDER BY salary DESC;#从高到低
SELECT * FROM employees ORDER BY salary AS;#从低到高，ASC可以去掉
/*案例：查询部门编号>=90的员工信息，按入职时间的先后进行排序--【添加筛选条件】*/
SELECT * 
FROM employees 
WHERE department_id>=90
ORDER BY hiredate ASC;
/*案例：按年薪的高低显示员工的信息和年薪--【按表达式排序】*/
SELECT *,salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
FROM employees 
ORDER BY salary*12*(1+IFNULL(commission_pct,0)) DESC;
/*案例：按年薪的高低显示员工的信息和年薪--【按别名排序】*/
SELECT *,salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
FROM employees 
ORDER BY 年薪 DESC;
/*案例：按姓名的长度显示员工的姓名和工资--【按函数排序】*/
SELECT LENGTH(last_name) 字节长度,last_name,salary
FROM employees 
ORDER BY LENGTH(last_name) DESC;
/*案例：查询员工信息，要求先按工资排序，再按员工编号排序--【多个字段排序】*/
SELECT *
FROM employees
ORDER BY salary ASC,employee_id DESC;
```



## 常见函数



### 常见函数介绍

```mysql
#常见函数
/*
功能：类似于Java的方法，将一组逻辑语句封装在方法体中，对外暴露方法名

优点：
		1.隐藏了实现细节
		2.提高代码的重用性
		
调用：SELECT 函数名(实参列表) [FROM 表];

特点：
		①叫什么（函数名）
		②干什么（函数功能）
		
分裂：
		1.单行函数，如CONCAT,LENGTH,IFNULL等
		2.分组函数
		功能：做统计使用，又称为统计函数、聚合函数、组函数
	
常见函数：
	字符函数：
	LENGTH
	CONCAT 
	SUBSTR
	INSTR
	TRIM
	UPPER
	LOWER
	LPAD
	RPAD
	REPLACE
	
	数学函数：
	ROUND
	CEIL
	FLOOR
	TRUNCATE
	MOD
	
	日期函数：
	NOW
	CURDATE
	CURTIME
	YEAR
	MONTH
	MONTHNAME
	DAY
	HOUR
	MINUTE
	SECOND
	STR_TO_DATE
	DATE_FORMAT
	
	其他函数：
	VERSION
	DATABASE
	USER
	
	控制函数：
	IF
	CASE
*/
```



### 单行函数

![image-20210201163219489](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210201163219489.png)



#### 字符函数

```mysql
#一、字符函数

#LENGTH 获取参数值的字节个数
SELECT LENGTH('john');
SELECT LENGTH('张三丰hahaha');

SHOW VARIABLES LIKE '%char%'#显示客户端的字符集

#CONCAT 拼接字符串
SELECT CONCAT(last_name,'_',first_name) 姓名 FROM employees;

#UPPER\LOWER
SELECT UPPER('john');
SELECT LOWER('JOHN');
/*实例：将姓变大写，名变小写，然后拼接*/
SELECT CONCAT(UPPER(last_name),LOWER(first_name)) 姓名 FROM employees;

#SUBSTR\SUBSTRING
/*注意：索引从1开始*/
/*【截取从指定索引处后面所有字符】*/
SELECT SUBSTR('李莫愁爱上了陆展元',7) out_put;
/*【截取从指定索引处指定字符长度的字符】*/
SELECT SUBSTR('李莫愁爱上了陆展元',1,3) out_put;
/*案例：姓名中首字符大写，其他字符小写然后用_拼接，显示出来*/
SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),LOWER(SUBSTR(last_name,2))) out_put 
FROM employees;

#INSTR 返回子串第一次出现的索引，如果找不到返回0
SELECT INSTR('杨不悔爱上了殷六侠','殷六侠') AS out_put;

#TRIM 
SELECT LENGTH(TRIM('           张翠山          ')) AS out_put;
SELECT TRIM('A' FROM 'AAAAAAA张AAAAAA翠山AAAAAAAA') AS out_put;

#LPAD 同指定的字符实现左填充指定长度
SELECT LPAD('殷素素',10,'*') AS out_put;
SELECT LPAD('殷素素',2,'*') AS out_put;#结果为殷素

#RPAD 同指定的字符实现右填充指定长度
SELECT RPAD('殷素素',10,'ab') AS out_put;
SELECT RPAD('殷素素',2,'*') AS out_put;#结果为殷素

#REPLACE 替换,字符串内指定字符全部替换
SELECT REPLACE('张无忌爱上了周芷若','周芷若','赵敏') AS out_put;
```



#### 数学函数

```mysql
#二、数学函数

#ROUND 四舍五入
SELECT ROUND(1.65);
SELECT ROUND(1.567,2);

#CEIL 向上取整，返回>=该参数的最小整数
SELECT CEIL(1.00);#结果为1

#FLOOR 向下取整，返回<=该参数的最大整数
SELECT FLOOR(-9.99);

#TRUNCATE 截断
SELECT TRUNCATE(1.69999,1);

#MOD 取余
/* MOD(A,B): A-A/B*B */
SELECT MOD(10,3);#SELECT 10%3;

#RAND 获取随机数，返回0-1之间的小数
```



#### 日期函数

```mysql
#三、日期函数

#NOW 返回当前系统日期+时间
SELECT NOW();

#CURDATE 返回当前系统日期，不包含时间
SELECT CURDATE();

#CURTIME 返回当前时间，不包含日期
SELECT CURTIME();

#获取指定的部分，年、月、日、小时、分钟、秒
#
SELECT YEAR(NOW()) 年;
SELECT YEAR('1998-1-1') 年;
SELECT YEAR(hiredate) 年 FROM employees;
#
SELECT MONTH(NOW()) 月;
SELECT MONTHNAME(NOW()) 月;

#STR_TO_DATE 将字符通过指定的格式转换成日期
SELECT STR_TO_DATE('9-13-1999','%m-%d-%Y');
/*案例：查询入职日期为1992-4-3的员工信息*/
SELECT * FROM employees WHERE hiredate = STR_TO_DATE('4-3 1992','%c-%d %Y');

#DATE_FORMAT 将日期转换成字符
SELECT DATE_FORMAT('2018/6/6','%Y年%m月%d日');
/*案例：查询有奖金的员工名和入职日期（XX月/XX日 XX年）*/
SELECT last_name,DATE_FORMAT(hiredate,'%m月/%d日 %y年') 入职日期
FROM employees
WHERE commission_pct IS NOT NULL;

#DATEDIFF 返回两个日期相差的天数

#MONTHNAME 以英文形式返回月
```

![image-20210201185946512](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210201185946512.png)



#### 其他函数

```mysql
#四、其他函数

SELECT VERSION();#当前数据库服务器的版本

SELECT DATABASE();#当前打开的数据库

SELECT USER();#当前用户

#PASSWORD('字符');返回该字符的密码形式
#MD5('字符');返回该字符的MD5加密形式
```



#### 流程控制函数

```mysql
#五、流程控制函数

#IF函数：类似于if else的效果
SELECT IF(10>5,'大','小');
SELECT last_name,commission_pct,IF(commission_pct IS NULL,'没奖金','有奖金') 备注
FROM employees;

#CASE函数的使用一：类似于switch case的效果
/*
	CASE 要判断的表达式
	WHEN 常量1 THEN 要显示的值1或语句1;
	…
	ELSE 要显示的值n或语句n;
	END
*/
/*案例：查询员工的工资，要求

部门号=30，显示工资为1.1倍
部门号=40，显示工资为1.2倍
部门号=50，显示工资为1.3倍
其他部门，显示的工资为原工资
*/
SELECT salary 原始工资,department_id,
CASE department_id
WHEN 30 THEN salary*1.1#值后面不用加分号，加分号表达式就结束了
WHEN 40 THEN salary*1.2
WHEN 50 THEN salary*1.3
ELSE salary
END AS 新工资
FROM employees;

#CASE函数的使用二：类似于多重IF
/*
	CASE 
	WHEN 条件1 THEN 要显示的值1或语句1;
	WHEN 条件2 THEN 要显示的值2或语句2;
	…
	ELSE 要显示的值n或语句n;
	END
*/
/*案例：查询员工的工资的情况

如果工资>20000，显示A级别
如果工资>15000，显示B级别
如果工资>10000，显示C级别
否则显示D级别
*/
SELECT salary,
CASE 
WHEN salary>20000 THEN 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END AS 工资级别
FROM employees;
```



## 分组函数

```mysql
#分组函数
/*
功能：用做统计使用，又称为聚合函数或统计函数或组函数

分类：
SUM 求和、AVG 平均值、MAX 最大值、MIN 最小值、COUNT 计算个数

特点：
1、SUM,AVG一般用于处理数值型
	 MAX,MIN,COUNT可以处理任何类型
2、以上分组函数都忽略NULL值
3、可以和DISTINCT搭配实现去重的运算
4、COUNT函数的单独介绍
	一般使用COUNT(*)用作统计行数
5、和分组函数一同查询的字段要求是GROUP BY后的字段
*/

#简单的使用
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT MIN(salary) FROM employees;
SELECT MAX(salary) FROM employees;
SELECT COUNT(salary) FROM employees; 
SELECT SUM(salary) 和,AVG(salary) 平均 FROM employees;

#参数支持哪些类型
SELECT SUM(last_name),AVG(last_name) FROM employees;
SELECT SUM(hiredate),AVG(hiredate) FROM employees;
#
SELECT MAX(last_name),MIN(last_name) FROM employees;
SELECT MAX(hiredate),MIN(hiredate) FROM employees;
#
SELECT COUNT(commission_pct) FROM employees;
SELECT COUNT(last_name) FROM employees;

#忽略NULL值
SELECT SUM(commission_pct),AVG(commission_pct),SUM(commission_pct)/35,SUM(commission_pct)/107 FROM employees;#NULL加上任何值为NULL
#
SELECT MAX(commission_pct),MIN(commission_pct) FROM employees;
#
SELECT COUNT(commission_pct) FROM employees;

#和DISTINCT搭配
SELECT SUM(DISTINCT salary),SUM(salary) FROM employees;
#
SELECT COUNT(DISTINCT salary),COUNT(salary) FROM employees;

#COUNT函数的详细介绍
SELECT COUNT(salary) FROM employees;
SELECT COUNT(*) FROM employees;#求行数
SELECT COUNT(1) FROM employees;#求行数,COUNT中加一个常量值，表示在表又加了一列常量
/*效率：
MYISAM存储引擎下，COUNT(*)的效率高
INNODB存储引擎下，COUNT(*)和COUNT(1)的效率差不多，比COUNT(字段)要高一些
*/

#和分组函数一同查询的字段有限制
/*是错误的：SELECT SUM(salary),department_id FROM employees;*/

#DATEDIFE
SELECT DATEDIFF('2017-10-1','2017-9-29');#得前一个日期减后一个日期相差的天数
/*案例：查询员工表中的最大入职时间和最小入职时间的相差天数，记为DIFFRENCE*/
SELECT DATEDIFF(MAX(hiredate),MIN(hiredate)) DIFFRENCE
FROM employees;
```



## 分组查询

![image-20210202162249731](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210202162249731.png)

```mysql
#分组查询
/*
引入：查询每个部门的平均工资
SELECT AVG(salary) FROM employees;

语法：
	SELECT 分组函数,列（要求出现在GROUP BY的后面）
	FROM 表
	[WHERE 筛选条件]
	GROUP BY 分组的列表
	[ORDER BY 子句]
注意：查询列表必须特殊，要求是分组函数和GROUP BY后出现的字段

特点：
		1、分组查询中的筛选条件分为两类
					数据源		位置		关键字
		分组前筛选	原始表		GROUP BY子句的前面	WHERE
		分组后筛选	分组后的结果表	GROUP BY子句的后面	HAVING
		①分组函数做条件肯定是放在HAVING子句中
		②能用分组前筛选的，就优先考虑使用分组前筛选（考虑到性能的问题）
		2、GROUP BY子句支持单个字段分组，多个字段分组（多个字段之间用逗号隔开没有顺序要求），表达式或函数（用的较少）
		3、也可以添加排序（排序放在整个分组查询的最后）
*/

#简单使用
/*案例：查询每个工种的最高工资*/
SELECT MAX(salary),job_id
FROM employees
GROUP BY job_id;
/*案例：查询每个位置上的部门个数*/
SELECT COUNT(*),location_id
FROM departments
GROUP BY location_id;

#添加分组前的筛选条件
/*案例：查询邮箱中包含a字符的，每个部门的平均工资*/
SELECT AVG(salary),department_id
FROM employees
WHERE email LIKE '%a%'
GROUP BY department_id
/*案例：查询有奖金的每个领导手下员工的最高工资*/
SELECT MAX(salary),manager_id
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY manager_id;

#添加分组后的筛选条件
/*案例：查询哪个部门的员工个数>2*/
#①查询每个部门的员工个数
SELECT COUNT(*),department_id
FROM employees
GROUP BY department_id;
#②根据①的结果进行筛选，查询哪个部门的员工个数>2
SELECT COUNT(*),department_id
FROM employees
GROUP BY department_id
HAVING COUNT(*)>2;
/*案例：查询每个工种有奖金的员工的最高工资>12000的工种编号和最高工资*/
#①查询每个工种有奖金的员工的最高工资
#②根据①结果继续筛选，最高工资>12000
SELECT MAX(salary),job_id
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY job_id
HAVING MAX(salary)>12000;
/*案例：查询领导编号>102的每个领导手下的最低工资>5000的领导编号是哪个，以及其最低工资*/
SELECT MIN(salary),manager_id
FROM employees
WHERE manager_id>102
GROUP BY manager_id
HAVING MIN(salary)>5000;

#按表达式或函数分组
/*案例：按员工姓名的长度分组，查询每一组的员工个数，筛选员工个数>5的有哪些*/
SELECT COUNT(*),LENGTH(last_name) len_name
FROM employees
GROUP BY LENGTH(last_name)
HAVING COUNT(*)>5;

#按多个字段分组
/*案例：查询每个部门每个工种的员工的平均工资*/
SELECT AVG(salary),department_id,job_id
FROM employees
GROUP BY department_id,job_id;#顺序可以变

#添加排序
/*案例：查询每个部门每个工种的员工的平均工资，并且按平均工资的高低显示*/
SELECT AVG(salary),department_id,job_id
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id,job_id;
HAVING AVG(salary)>5000
ORDER BY AVG(salary) DESC;
```



## 连接查询

![image-20210202204227699](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210202204227699.png)

```mysql
#连接查询
/*
含义：又称多表查询，当查询的字段来自于多个表时，就会用到连接查询

笛卡尔乘积现象：表1有m行，表2有n行，结果=m*n行
发生原因：没有有效的连接条件
如何避免：添加有效的连接条件
SELECT name,boyName FROM boys,beauty
WHERE beauty.boyfriend_id=boys.id;

分类：
			按年代分类：
				sql92标准：仅仅支持内连接
				sql99标准【推荐】：支持内连接+外连接（左外和右外）+交叉连接
			按功能分类：
				内连接：
					等值连接
					非等值连接
					自连接
				外连接：
					左外连接
					右外连接
					全外连接
				交叉连接
*/
```



### sql92标准

```mysql
#一、sql92标准

#等值连接
/*
①多表等值连接的结果为多表的交集部分
②n表连接，至少需要n-1个连接条件
③多表的顺序没有要求
④一般需要为表起别名
⑤可以搭配前面介绍的所有子句使用，比如排序、分组、筛选
*/
/*案例：查询女神名对应的男神名*/
SELECT name,boyName 
FROM boys,beauty
WHERE beauty.boyfriend_id=boys.id;
/*案例：查询员工名和对应的部门名*/
SELECT last_name,department_name
FROM employees,departments
WHERE employees.department_id = departments.department_id;
#
#为表起别名
/*
①提高语句的简洁度
②区分多个重名的字段

注意：如果为表起了别名，则查询的字段就不能使用原来的表名去限定
*/
/*案例：查询员工名、工种号、工种名*/
SELECT last_name,e.job_id,job_title
FROM employees e,jobs j
WHERE e.`job_id` = j.`job_id`;
#
#两个表的顺序是否可以调换-->可以
SELECT last_name,e.job_id,job_title
FROM job j,employees e
WHERE e.`job_id` = j.`job_id`;
#
#可以加筛选
/*案例：查询有奖金的员工名、部门名*/
SELECT last_name,department_name,commission_pct
FROM employees e,departments d
WHERE e.`department_id`= d.`department_id`
AND e.`commission_pct` IS NOT NULL;
/*案例：查询城市名中第二个字符为o的部门名和城市名*/
SELECT department_name,city
FROM departments d, locations l
WHERE d.location_id = l.location_id;
AND city LIKE '_o%';
#
#可以加分组
/*案例：查询每个城市的部门个数*/
SELECT COUNT(*) 个数,city
FROM departments d,locations l
WHERE d.location_id = l.location_id
GROUP BY city;
/*案例：查询有奖金的每个部门的部门名和部门的领导编号和该部门的最低工资*/
SELECT department_name,d.`manager_id`,MIN(salary)
FROM departments d,employees e
WHERE d.department_id = e.department_id
AND commission_pct IS NOT NULL
GROUP BY department_name,d.`manager_id`;
#
#可以加排序
/*案例：查询每个工种的工种名和员工个数，并且按员工个数降序*/
SELECT job_title,COUNT(*)
FROM employees e,jobs j
WHERE e.job_id = j.job_id
GROUP BY job_title
ORDER BY COUNT(*) DESC;
#
#可以实现三表连接
/*案例：查询员工名、部门名和所在的城市*/
SELECT last_name,department_name,city
FROM employees e,departments d,locations l
WHERE e.department_id = d.department_id
AND d.location_id = l.location_id;#可以继续向下加条件

#非等值连接
/*案例：查询员工的工资和工资级别*/
SELECT salary,grade_level
FROM employees e,job_grades g
WHERE salary BETWEEN g.`lowest_sal` AND g.`highest_sal`;

#自连接
/*案例：查询员工名和上级的名称*/
SELECT e.employee_id,e.last_name,m.employee_id,m.last_name
FROM employees e,employees m
WHERE e.manager_id = m.employee_id;
```



### sql99标准

```mysql
#二、sql99语法
/*
语法：
	SELECT 查询列表
	FROM 表1 别名 【连接类型】
	JOIN 表2 别名 
	ON 连接条件
	[WHERE 筛选条件]

内连接（*）:INNER
外连接
	左外（*）:LEFT [OUTER]
	右外（*）:RIGHT [OUTER]
	全外:FULL [OUTER]
交叉连接:CROSS
*/

#内连接
/*
语法：
	SELECT 查询列表
	FROM 表1 别名
	INNER JOIN 表2 别名 
	ON 连接条件

分类：
等值
非等值
自连接

特点：
①添加排序、分组、筛选
②INNER可以省略
③筛选条件放在WHERE后面，连接条件放在ON后面，提高分离性，便于阅读
④INNER JOIN连接和sql92语法中的等值连接效果是一样的，都是查询多表的交集
⑤表的顺序可以调换
⑥n表连接至少需要n-1个连接条件
*/
#
#等值连接
/*案例：查询员工名、部门名【表可以调换位置】*/
SELECT last_name,department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
/*案例：查询名字中包含e的员工名和工种名【添加筛选】*/
SELECT last_name,job_title
FROM employees e
INNER JOIN jobs j
ON E.job_id = j.job_id
WHERE e.last_name LIKE '%e%';
/*案例：查询部门个数>3的城市名和部门个数【添加分组+筛选】*/
SELECT city,COUNT(*) 部门个数
FROM departments d
INNER JOIN locations l
ON d.location_id=l.location_id
GROUP BY city
HAVING COUNT(*)>3; 
/*案例：查询哪个部门的员工个数>3的部门名和员工个数，并按个数降序【添加排序】*/
SELECT department_name,COUNT(*)
FROM employees e
INNER JOIN departments d
ON e.department_id=d.department_id
GROUP BY department_name
HAVING COUNT(*)>3
ORDER BY COUNT(*) DESC; 
/*案例：查询员工名，部门名，工种名，并按部门名降序【多表连接】*/
SELECT last_name,department_name,job_title
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
INNER JOIN jobs j ON e.job_id=j.job_id
ORDER BY department_name DESC; 
#
#非等值连接
/*案例：查询员工的工资级别*/
SELECT salary,grade_level
FROM employees e
JOIN job_grades g
ON e.salary BETWEEN g.lowest_sal AND g.highest_sal;
/*案例：查询每个工资级别的个数>20的个数，并且按工资级别降序*/
SELECT COUNT(*),grade_level
FROM employees e
JOIN job_grades g
ON e.salary BETWEEN g.lowest_sal AND g.highest_sal
GROUP BY grade_level
HAVING COUNT(*)>20
ORDER BY grade_level DESC;
#
#自连接
/*案例：查询员工的名字、上级的名字*/
SELECT e.last_name,m.last_name
FROM employees e
JOIN employees m
ON e.manager_id=m.employee_id;
/*案例：查询姓名中包含字符k的员工的名字、上级的名字*/
SELECT e.last_name,m.last_name
FROM employees e
JOIN employees m
ON e.manager_id=m.employee_id
WHERE e.last_name LIKE '%k%';

#外连接
/*
引入：查询男朋友不在男生表的女神名

应用场景：用于查询一个表中有，另一个表没有的记录

特点：
1、外连接的查询结果为主表中的所有记录
	如果从表中有和它匹配的，则显示匹配的值
	如果从表中没有和它匹配的，则显示NULL
	外连接查询结果=内连接结果+主表中有而从表没有的记录
2、左外连接，LEFT左边的是主表
	右外连接，RIGHT JOIN右边的是主表
3、左外和右外交换两个表的顺序可以实现同样的效果
4、全外连接=内连接的结果+表1中有但表2中没有的+表2中有但表1没有的
*/
/*引入：查询男朋友不在男生表的女神名*/
SELECT b.name
FROM beauty b
LEFT OUTER JOIN boys bo
ON b.boyfriend_id = bo.id
WHERE bo.id IS NULL;
/*案例：查询哪个部门没有员工*/
#左外
SELECT d.*,e.employee_id
FROM departments d
LEFT OUTER JOIN employees e
ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
#右外
SELECT d.*,e.employee_id
FROM employees e
RIGHT OUTER JOIN departments d
ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
#
#全外连接
SELECT b.*,bo.*
FROM beauty b
FULL OUTER JOIN boys bo
ON b.boyfriend_id = bo.id;

#交叉连接 99语法的笛卡尔乘积
SELECT b.*,bo.*
FROM beauty b
CROSS JOIN boys bo;
```



### 总结连接查询

sql92和sql99比较：
功能：sql99支持的较多
可读性：sql99实现连接条件和筛选条件的分离，可读性较高

![image-20210205211239094](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210205211239094.png)

![image-20210205211436287](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210205211436287.png)



## 子查询

```mysql
#子查询
/*
含义：
出现在其他语句中的SELECT语句，称为子查询或内查询
外面的语句可以是INSERT,UPDATE,DELETE,SELECT等，一般SELECT作为外面语句较多
外部的查询语句，则称为主查询或外查询，

分类：
	按子查询出现的位置：
		SELECT后面：
			仅仅支持标量子查询
		FROM后面：
			支持表子查询
		WHERE或HAVING后面：
			标量子查询（单行）
			列子查询（多行）
			行子查询
		EXISTS后面（相关子查询）：
			表子查询
	按结果集的行列数不同：
		标量子查询（结果集只有一行一列）
		列子查询（结果集只有一列多行）
		行子查询（结果集有一行多列）
		表子查询（结果集一般为多行多列）
*/

#WHERE或HAVING后面
/*
1、标量子查询（单行子查询）
2、列子查询（多行子查询）
3、行子查询（多列多行）

特点：
①子查询放在小括号内
②子查询一般放在条件的右侧
③标量子查询，一般搭配着单行操作符使用> < >= <= = <>
 列子查询，一般搭配着多行操作符使用IN ANY/SOME ALL
④子查询的执行优先于主查询执行，主查询的条件用到了子查询的结果
*/
#
#标量子查询
/*案例：谁的工资比Abel高？*/
#①查询Abel的工资
SELECT salary
FROM employees
WHERE last_name = 'Abel'
#②查询员工的信息，满足salary>①结果
SELECT *
FROM employees
WHERE salary > (
	SELECT salary
	FROM employees
	WHERE last_name = 'Abel'
);
/*案例：返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资*/
SELECT  last_name,job_id,salary
FROM employees
WHERE job_id = (
	SELECT job_id
	FROM employees
	WHERE employee_id = 141
) AND salary > (
	SELECT salary
	FROM employees
	WHERE employee_id = 143
);
/*案例：返回公司工资最少的员工的last_name,job_id和salary*/
SELECT
	last_name,
	job_id,
	salary 
FROM
	employees 
WHERE
	salary = (
	SELECT
		MIN( salary ) 
	FROM
	employees 
	);
/*案例：查询最低工资大于50号部门最低工资的部门id和其最低工资*/
SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary)>(
	SELECT MIN(salary)
	FROM employees
	WHERE department_id = 50
);
#非法使用标量子查询
/*
子查询结果不是一行一列的都是非法标量子查询
*/
SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary)>(
	SELECT salary #
	FROM employees
	WHERE department_id = 250 #
);
#
#列子查询（多行子查询）
/*案例：返回location_id是1400或1700的部门中所有员工姓名*/
SELECT last_name
FROM employees 
WHERE department_id IN(
	SELECT DISTINCT department_id
	FROM departments
	WHERE location_id IN(1400,1700)
);
/*案例：返回其它工种中比job_id为'IT_PROG'工种任一工资低的员工的员工号、姓名、job_id以及salary*/
SELECT last_name,employee_id,job_id,salary
FROM employees
WHERE salary < ANY(
		SELECT DISTINCT salary
		FROM employees
		WHERE job_id = 'IT_PROG'
) AND job_id <> 'IT_PROG';
/*案例：返回其它工种中比job_id为'IT_PROG'工种所有工资低的员工的员工号、姓名、job_id以及salary*/
SELECT last_name,employee_id,job_id,salary
FROM employees
WHERE salary < ALL(
		SELECT DISTINCT salary
		FROM employees
		WHERE job_id = 'IT_PROG'
) AND job_id <> 'IT_PROG';
#
#行子查询（结果集一行多列或多行多列）
/*案例：查询员工编号最小并且工资最高的员工信息*/
SELECT *
FROM employees
WHERE (employee_id,salary) = (
	SELECT MIN(employee_id),MAX(salary)
	FROM employees
);

#SELECT后面
/*
只支持标量子查询
*/
/*案例：查询每个部门的员工个数*/
SELECT d.*,(
	SELECT COUNT(*)
	FROM employees e
	WHERE e.department_id = d.department_id
) 个数
FROM departments d; 
/*案例：查询员工号=102的部门名*/
SELECT (
	SELECT department_name
	FROM departments d
	INNER JOIN employees e
	ON d.department_id = e.department_id
	WHERE e.employee_id = 102
) 部门名;

#FROM后面
/*
将子查询结果充当一张表，要求必须起别名
*/
/*案例：查询每个部门的平均工资的工资等级*/
SELECT  ag_dep.*,g.grade_level
FROM (
	SELECT AVG(salary) ag,department_id
	FROM employees
	GROUP BY department_id
) ag_dep
INNER JOIN job_grades g
ON ag_dep.ag BETWEEN lowest_sal AND highest_sal;

#EXISTS后面（相关子查询）
/*
语法：
EXISTS(完整的查询语句)
结果：
1或0
*/
/*案例：查询有员工的部门名*/
SELECT department_name
FROM departments d
WHERE EXISTS(
	SELECT *
	FROM employees e
	WHERE d.department_id = e.department_id
);
/*案例：查询没有女朋友的男神信息*/
SELECT bo.*
FROM boys bo
WHERE NOT EXISTS(
	SELECT boyfriend_id
	FROM beauty b
	WHERE bo.id = b.boyfriend_id
);
```



![image-20210216145326821](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210216145326821.png)

## 分页查询

```mysql
#分页查询
/*
应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求
	
语法：
	SELECT 查询列表
	FROM 表
	【JOIN
	ON
	WHERE 
	GROUP BY
	HAVING
	ORDER BY】
	LIMIT offset,size;#offset为要显示条目的起始索引（起始索引从0开始）；size为要显示的条目个数
	
特点：
	①LIMIT语句放在查询语句的最后
	②公式：
	要显示的页数 page，每页的条目数 size
	SELECT 查询列表
	FROM 表
	LIMIT (page-1)*size,size;
*/
/*案例：查询前五条员工信息*/
SELECT *
FROM employees
LIMIT 0,5;
SELECT *
FROM employees
LIMIT 5;
/*案例：查询第11条至第25条*/
SELECT *
FROM employees
LIMIT 10,15;
/*案例：有奖金的员工信息，并且工资较高的前10名显示出来*/
SELECT *
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY salary DESC
LIMIT 10;
```



## union联合查询

```mysql
#联合查询
/*
UNION 联合 合并：将多条查询语句的结果合并成一个结果

语法：
查询语句1
UNION
查询语句2
UNION
…

应用场景：
要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时

特点：
1、要求多条查询语句的查询列数是一致的！
2、要求多条查询语句的查询的每一列的类型和顺序最好一致
3、UNION关键字默认去重，如果使用UNION ALL 可以包含重复项

*/
/*案例：查询部门编号>90或邮箱包含a的员工信息*/
SELECT * FROM employees WHERE email LIKE '%a%'
UNION 
SELECT * FROM employees WHERE department_id>90;
/*案例：查询中国用户中男性的信息以及外国用户中年男性的用户信息*/
SELECT id,cname,csex FROM t_ca WHERE csex='男'
UNION #自动去重，如果不想去重使用UNION ALL
SELECT t_id,tName,tGender FROM t_ua WHERE tGender='male';
```

