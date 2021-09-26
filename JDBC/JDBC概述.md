# JDBC概述



## 数据的持久化

**持久化**：把数据保存到可掉电式存储设备中以供之后使用。大多数情况下，特别是企业级应用，数据持久化意味着将内存中的数据保存到硬盘上加以“固化”，而持久化的实现过程大多通过各种关系数据库来完成

持久化的主要应用是将内存中的数据存储在关系型数据库中，当然也可以存储在磁盘文件、XML数据文件中



## Java中的数据存储技术

在Java中，数据库存取技术可分为：

1. JDBC直接访问数据库

 	2. JDO技术
 	3. 第三方O/R工具，如Hibernate，Mybatis等

JDBC是Java访问数据库的基石，JDO、Hibernate、Mybatis等只是更好的封装了JDBC



## JDBC介绍

JDBC是一个独立于特定数据库管理系统，通用的SQL数据库存取和操作的公共接口，定义了用来访问数据库的标准Java类库（java.sql，javax.sql），使用这些类库可以以一种标准的方法，方便的访问数据库资源

JDBC为访问不同数据库提供了一种统一的路径，为开发者屏蔽了一些细节问题

JDBC的目标是使java程序员使用JDBC可以连接任何提供了JDBC驱动程序的数据库系统，这样就使得程序员无需对特定的数据库的系统的特点有过多的了解，从而大大简化和加快了开发过程

![image-20210404174652818](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210627192527.png)



## JDBC体系结构

JDBC接口（API）包括两个层次：

* 面向应用的API：Java API，抽象接口，供应用程序开发人员使用（连接数据库，执行SQL语句，获得结果）
* 面向数据库的API：Java Driver API，供开发商开发数据库驱动程序使用



## JDBC程序填写步骤

![image-20210405124559507](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210627192658.png)









