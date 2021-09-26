# Apache-DBUtils实现CRUD操作



## Apache-DBUtils简介

commons-dbutils是Apache组织提供的一个开源JDBC工具类库，封装了针对于数据库的增删改查操作

导入jar包：

```xml
<dependency>
    <groupId>commons-dbutils</groupId>
    <artifactId>commons-dbutils</artifactId>
    <version>1.6</version>
</dependency>
```



## QueryRunner类

提供数据库操作的一系列重载的update()和query()操作

update():

```java
@Test
public void testInsert(){
    Connection conn = null;
    try {
        QueryRunner runner = new QueryRunner();
        conn = DruidTest.getConnection();
        String sql = "insert into customers(name,email,birth) values(?,?,?)";
        int insertCount = runner.update(conn,sql,"菜菜子","caicai@126.com","1988-07-23");
        System.out.println(insertCount);
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,null);
    }
}
```

查询：

BeanHandler是ResultSetHandler接口的实现类，用于封装表中的一条记录

BeanListHandler是ResultSetHandler接口的实现类，用于封装表中的多条记录构成的集合

```java
@Test
public void testQuery1() {
    Connection conn = null;
    try {
        QueryRunner runner = new QueryRunner();
        conn = DruidTest.getConnection();
        String sql = "select id,`name`,email,birth from customers where id =?";
        BeanHandler<Customer> handler = new BeanHandler<>(Customer.class);
        Customer customer = runner.query(conn,sql,handler,12);
        System.out.println(customer);
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,null);
    }
}
@Test
public void testQuerry2() {
    Connection conn = null;
    try {
        QueryRunner runner = new QueryRunner();
        conn = DruidTest.getConnection();
        String sql = "select id,`name`,email,birth from customers where id <?";
        BeanListHandler<Customer> handler = new BeanListHandler<>(Customer.class);
        List<Customer> list = runner.query(conn,sql,handler,12);
        list.forEach(System.out::println);
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,null);
    }
}
```

MapHandler是ResultSetHandler接口的实现类，对应表中的一条记录，将字段及相应字段的值作为map中的key和value

```java
MapHandler handler = new MapHandler();
Map<String,Object> map = runner.query(conn,sql,handler,12);
```

MapListHandler是ResultSetHandler接口的实现类，对应表中的多条记录，将字段及相应字段的值作为map中的key和value，将这些map添加到List中

```java
MapListHandler handler = new MapListHandler();
List<Map<String,Object>> list = runner.query(conn,sql,handler,12);
```

ScalarHandler用于查询特殊值

```java
@Test
public void testQuerry3() {
    Connection conn = null;
    try {
        QueryRunner runner = new QueryRunner();
        conn = DruidTest.getConnection();
        String sql = "select count(*) from customers";
        //第一种
        //ScalarHandler<Long> handler = new ScalarHandler<>();
        //Long count = runner.query(conn,sql,handler);
        //第二种
        ScalarHandler handler = new ScalarHandler();
        Long count = (Long) runner.query(conn,sql,handler);
        System.out.println(count);
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        JDBCUtils.closeResource(conn,null);
    }
}
```

如果有比较特殊的需求，可以自定义ResultSetHandler的实现类

 

## DbUtils类关闭资源的操作

```java
public static void closeResource(Connection conn, Statement ps, ResultSet rs){
    try {
        DbUtils.close(conn);
    } catch (SQLException throwables) {
        throwables.printStackTrace();
    }
    try {
        DbUtils.close(ps);
    } catch (SQLException throwables) {
        throwables.printStackTrace();
    }
    try {
        DbUtils.close(rs);
    } catch (SQLException throwables) {
        throwables.printStackTrace();
    }
    //或
    //DbUtils.closeQuietly(conn);
    //DbUtils.closeQuietly(ps);
    //DbUtils.closeQuietly(rs);
}
```