# 初始MySQL



## MySQL产品的介绍

优点：

1. 成本低：开放源代码，一般可以免费试用
2. 性能高：执行很快，移植性也好
3. 简单：体积小，很容易安装和使用

DBMS分为两类：

* 基于共享文件系统的DBMS（Access）
* 基于客户机——服务器的DBMS（MySQL，Oracle，SqlServer）



## MySQL产品的安装

卸载：

电脑管家；电脑卸载，记得MySQL文件夹和ProgramData文件夹，清理注册表

![image-20201221190321545](https://raw.githubusercontent.com/xzx-summer/image/main/img/image-20201221190321545.png)

安装：

属于c/s架构的软件，一般来讲安装服务端

按这个上面装，MySQL8.0的下载

 https://www.bilibili.com/read/cv5652639?share_medium=android&share_source=qq&bbid=XYD1BC7D04A9750F9183D8460C6B61B91EC7F&ts=1608198134867



## MySQL服务的启动和停止

1. 计算机-右击管理-服务中找到MySQL,右键点击选择启动或停止
2. cmd以管理员身份运行，net start MySQL 启动服务；net stop MySQL 停止服务，MySQL是服务名



## MySQL服务的登录和退出

登录：

1. 自带的客户端，Command Line Client，只适用于root用户
2. 使用cmd，mysql -h localhost -P 3306 -u root -p，mysql -h主机名 -P端口号 -u用户名 -p密码
3. 使用cmd，mysql -u root -proot，mysql -u用户名 -p密码

退出：exit或Ctrl+c



## MySQL的常见命令和语法规范

常见命令：

1. 查看当前所有的数据库

```mysql
show databases;
```

2. 打开指定的库

```mysql
use 库名;
```

3. 查看当前库的所有表

```mysql
show tables;
```

4. 查看其它库的所有表

```mysql
show tables from 库名;
```

5. 创建表

```mysql
create table 表名(
    列名 列类型,
    列名 列类型,
    ...
);
```

6. 查看表结构

```mysql
desc 表名;
```

7. 查看服务器的版本

方式一：登录到mysql服务端

```mysql
select version();
```

方式二：没有登录到mysql服务端

mysql --version或mysql --V



语法规范：

1. 不区分大小写，但建议关键字大写，表名、列名小写

2. 每条命令最好用分号结尾

3. 每条命令根据需要，可以进行缩进或换行

4. 注释

   ​	单行注释：#注释文字

   ​	单行注释：-- 注释文字

   ​	多行注释：/*注释文字 */

   

## 图形化用户界面客户端的安装和介绍

注意注册时一定要**断网**！！

https://www.bilibili.com/read/cv5803970

完成！芜湖！记得把永久许可证拍下来，感觉会有用。。。。

## 四种SQL语言的介绍

DQL（Data Query Language）:主要是select

DML（Data Manipulation Language）:主要是增删改

DDL（Data Define Language）：主要是库和表

TCL（Transaction Control Language）：事物控制

