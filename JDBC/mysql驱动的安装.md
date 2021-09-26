## 安装

1. 进入

https://dev.mysql.com/downloads/

![image-20210405140120950](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210405140120950.png)

2. 选择自己要安装的语言

![image-20210405140212203](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210405140212203.png)

3. 选择版本和平台

![image-20210405140356858](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210405140356858.png)

4. 然后就是下载啦，然后解压就可以用！

![image-20210405140558071](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210405140558071.png)

![image-20210405140710685](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210405140710685.png)



## 使用

在eclipse中：

1. 在工程里new$\rightarrow$Folder$\rightarrow$取名字为lib
2. 把jar包复制到lib目录下
3. 在jar包那里点右键$\rightarrow$Build Path$\rightarrow$Add to Build Path

可以啦！

![image-20210405141831109](C:\Users\15812\AppData\Roaming\Typora\typora-user-images\image-20210405141831109.png)



在IDEA中：

还不知道



## 使用Maven配置

https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.22

```Maven
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.22</version>
</dependency>

```





