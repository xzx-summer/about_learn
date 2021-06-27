# TCL语言的学习

Transaction Control Language 事务控制语言



## 事务和事务处理

事物：一个或一组sql语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行

![image-20210303170938199](https://raw.githubusercontent.com/xzx-summer/image/main/img/img/image-20210303170938199.png)

```mysql
#事务处理
/*
事务的创建：
隐式事物：事务没有明显的开启和结束的标记，如INSERT、UPDATE、DELETE语句
显式事物：事务具有明显的开启和结束的标记，前提是必须先设置自动提交功能为禁用[SET AUTOCOMMIT=0;]
	步骤1：开启事务
	SET AUTOCOMMIT=0;
	START TRANSACTION;#可选的
	步骤2：编写事务中的sql语句（SELECT,INSERT,UPDATE,DELETE）
	语句1;
	语句2;
	…
	步骤3：结束事务
	COMMIT;#提交事务
	ROLLBACK;#回滚事务
*/

#演示事务的使用步骤
#开启事务
SET AUTOCOMMIT=0;
START TRANSACTION;
#编写一组事务的语句
UPDATE account SET balance=500 WHERE username='zzz';
UPDATE account SET balance=1500 WHERE username='ccc';
#结束事务
COMMIT;


```



![image-20210303171507172.png](https://raw.githubusercontent.com/xzx-summer/image/main/img/img/image-20210303171507172.png)

![image-20210304150000511](https://raw.githubusercontent.com/xzx-summer/image/main/img/img/image-20210304150000511.png)

![image-20210304145912400](https://raw.githubusercontent.com/xzx-summer/image/main/img/img/image-20210304145912400.png)

![image-20210304152405097](https://raw.githubusercontent.com/xzx-summer/image/main/img/img/image-20210304152405097.png)

事务的隔离级别：

|                  | 脏读 | 不可重复读 | 幻读 |
| ---------------- | ---- | ---------- | ---- |
| read uncommitted | √    | √          | √    |
| read committed   | ×    | √          | √    |
| repeatable read  | ×    | ×          | √    |
| serializable     | ×    | ×          | ×    |

mysql中默认第三个隔离级别repeatable read

oracle中默认第二个隔离级别read committed

查看隔离级别：

```mysql
select @@tx_isolation;
```

设置隔离级别：

```mysql
set session|global transaction isolation level 隔离级别;
```

savepoint 节点名;#设置保存点

```mysql
/*savepoint 节点名;#设置保存点

*/

#演示savepoint的使用
SET AUTOCOMMIT=0;
START TRANSACTION;
DELETE FROM account WHERE id=25;
SAVEPOINT a;#设置保存点
DELETE FROM account WHERE id=28;
ROLLBACK TO a;#回滚到保存点
```

