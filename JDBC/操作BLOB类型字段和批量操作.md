# 操作BLOB类型字段和批量操作



## MySQL BLOB类型

* BLOB是一个二进制大型对象，是一个可以存储大量数据的容器，能容纳不同大小的数据
* 插入BLOB类型的数据必须使用PreparedStatement，因为BLOB类型的数据无法使用字符串拼接写的
* MySQL的四种BLOB类型，除了在存储最大信息量上不同外，他们是等同的

![image-20210718153050032](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210718153050.png)

* 实际使用中根据需要存入的数据大小定义不同的BLOB类型
* 但如果存储的文件过大，数据库的性能会下降
* 如果在指定了相关的Blob类型以后还报错：`XXX to large`,那么在mysql安装目录下，找`my.ini`文件加上配置参数`max_allowed_packet=16M`，同时注意，修改了`my.ini`文件后，需要重新启动mysql服务



## 操作BLOB类型的字段

**插入BLOB类型的数据：**使用流的方式传输

```java
FileInputStream is = new FileInputStream(new File("girl.jpg"));
ps.setBlob(4,is);
```

**查询BLOB类型的字段：**

两种获取属性的方法

```java
rs.getInt(int index);
rs.getInt(String columnLabel);
```

将BLOB类型的字段下载，以文件的方式保存在本地：

```java
Blob photo = rs.getBlob("photo");
InputStream is = photo.getBinaryStream();
FileOutputStream fos = new FileOutputStream("zhang.jpg");
byte[] buffer = new byte[1024];
int len;
while((len = is.read(buffer)) != -1){
    fos.write(buffer,0,len);
}
```



## 批量插入数据的操作

update、delete本身就具有批量操作的效果

**PreparedStatement如何实现更高效的批量插入？**

![image-20210719144010750](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210719144011.png)

方式二：

DBServer会对预编译语句提供性能优化，语句在DBServer的编译器编译后的执行代码被缓存下来，下次调用时只要时相同的预编译语句就不需要编译；用Statement即使是相同操作但因为数据内容不一样，所以整个语句本身不能匹配，没有缓存语句的意义，这样每执行一次都要对传入的语句编译一次

```java
@Test
public void testInsert1(){
    Connection conn = null;
    PreparedStatement ps = null;
    try {
        long start = System.currentTimeMillis();
        conn = JDBCUtils.getConnection();

        String sql="insert into user(name) values(?)";
        ps = conn.prepareStatement(sql);

        for (int i = 0; i < 20000; i++) {
            ps.setObject(1,"name_"+i);
            ps.execute();
        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为:"+(end-start));//130725
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,ps);
    }
}
```

方式三：

批处理

```java
addBatch();
executeBatch();
clearBatch();
```

mysql服务器默认关闭批处理，需要通过一个参数，让mysql开启批处理的支持：`?rewriteBatchedStatements=true`写在`.properties`配置文件的url后面

```java
@Test
public void testInsert2(){
    Connection conn = null;
    PreparedStatement ps = null;
    try {
        long start = System.currentTimeMillis();
        conn = JDBCUtils.getConnection();

        String sql="insert into user(name) values(?)";
        ps = conn.prepareStatement(sql);

        for (int i = 0; i < 20000; i++) {
            ps.setObject(1,"name_"+i);

            //”攒“ sql
            ps.addBatch();
            if(i%500 == 0){
                //执行batch
                ps.executeBatch();
                //清空batch
                ps.clearBatch();
            }

        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为:"+(end-start));//1580
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,ps);
    }
}
```

此时插入100 0000条数据的时间是：35788

方式四：

设置连接不允许自动提交数据

```java
@Test
public void testInsert3(){
    Connection conn = null;
    PreparedStatement ps = null;
    try {
        long start = System.currentTimeMillis();
        conn = JDBCUtils.getConnection();

        //设置不允许自动提交数据
        conn.setAutoCommit(false);

        String sql="insert into user(name) values(?)";
        ps = conn.prepareStatement(sql);

        for (int i = 0; i < 1000000; i++) {
            ps.setObject(1,"name_"+i);

            //”攒“ sql
            ps.addBatch();
            if(i%500 == 0){
                //执行batch
                ps.executeBatch();
                //清空batch
                ps.clearBatch();
            }

        }

        //在最后统一提交数据
        conn.commit();

        long end = System.currentTimeMillis();
        System.out.println("花费的时间为:"+(end-start));//21424
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,ps);
    }
}
```









