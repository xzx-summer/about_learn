# 使用PreparedStatement实现CRUD操作



## 操作和访问数据库

一个数据库连接就是一个Socket连接

在java.sql包中有3个接口分别定义了对数据库的调用的不同方式：

* Statement：用于执行静态SQL语句并返回它所生成结果的对象
* PreparedStatement：SQL语句被预编译并存储在此对象中，可以使用此对象多次高效的执行该语句
* CallableStatement：用于执行SQL存储过程



## 使用Statement操作数据库的弊端

* 存在拼串，操作繁琐

  只要用PreparedStatement取代Statement就可以解决拼串

* 存在sql注入问题

  只要用PreparedStatement取代Statement就可以避免sql注入，因为PreparedStatement有预编译的过程，预先把sql的逻辑确定了



## PreparedStatement的使用

![image-20210716172055448](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210716172113.png)

**增删改：**

增：

```java
 @Test
    public void PreparedStatementUpdateTest() {
        Connection conn = null;
        PreparedStatement ps = null;
        try {
            //获取配置文件中四个关于连接的信息
            InputStream is = PreparedStatementUpdate.class.getClassLoader().getResourceAsStream("jdbc01.properties");
            //InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc01.properties");//两种获取方式都可以
            Properties pros = new Properties();
            pros.load(is);
            String user = pros.getProperty("user");
            String password = pros.getProperty("password");
            String url = pros.getProperty("url");
            String driver = pros.getProperty("driver");
            Class.forName(driver);
            conn = DriverManager.getConnection(url,user,password);

            //预编译sql语句，返回PreparedStatement的实例
            String sql = "insert into customers(name,email,birth) values(?,?,?)";//?:占位符
            ps = conn.prepareStatement(sql);

            //填充占位符，下标从1开始
            ps.setString(1,"哪吒");
            ps.setString(2,"nezha@gmail.com");
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            java.util.Date date = sdf.parse("1000-01-01");//插入数据库后时间为0999-12-31，时区问题
            // 数据库日期时间默认值使用1901年以后的日期
            ps.setDate(3,new java.sql.Date(date.getTime()));

            //执行操作
            ps.execute();

        } catch (Exception e) {
            e.printStackTrace();

        } finally {
            //资源的关闭
            try {
                if(null != ps)
                    ps.close();
            }catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if(null != conn)
                    conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
```

将连接的建立和关闭放到工具类中（单例模式）：

```java
package util;

import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JDBCUtils {

    /**
     * 获取数据库连接
     * @return
     */
    public static Connection getConnection() throws Exception {
        //读取配置文件中的4个基本信息
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc01.properties");
        Properties pros = new Properties();
        pros.load(is);

        String user = pros.getProperty("user");
        String password = pros.getProperty("password");
        String url = pros.getProperty("url");
        String driver = pros.getProperty("driver");

        Class.forName(driver);

        Connection conn = DriverManager.getConnection(url,user,password);
        return conn;
    }

    /**
     * 关闭连接和Statement的操作
     * @param conn
     * @param ps
     */
    public static void closeResource(Connection conn, Statement ps){
        try {
            if(null != ps)
                ps.close();
        }catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if(null != conn)
                conn.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
    }
}

```

修改：

```java
 @Test
    public void testUpdate(){
        Connection conn = null;
        PreparedStatement ps = null;
        try{
            conn = JDBCUtils.getConnection();

            String sql = "update customers set name=? where id=? ";
            ps = conn.prepareStatement(sql);

            ps.setString(1,"莫扎特");
            ps.setInt(2,18);

            ps.execute();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,ps);
        }
    }
```

增删改的操作是基本一样的：1. 获取数据库连接 2. 预编译sql语句，返回PreparedStatement的实例 3. 填充占位符 4. 执行 5. 资源的关闭

通用（不同的增删改和不同的表）的增删改：

```java
 public void update(String sql,Object ...args){
        Connection conn = null;
        PreparedStatement ps = null;
        try{
            conn = JDBCUtils.getConnection();

            ps = conn.prepareStatement(sql);

            for (int i = 0; i < args.length; i++) {
                ps.setObject(i+1,args[i]);
            }
            ps.execute();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn,ps);
        }
    }
```

删除：

```java
 @Test
    public void testCommonUpdate(){
        String sql = "delete from customers where id = ?";
        update(sql,3);
    }
```

**查：**

ORM（object relational mapping) 编程思想：一个数据表对应一个java类，表中的一条记录对应java类的一个对象，表中的一个字段对应java类的一个属性

![image-20210717203156191](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210717203156.png)

在同一个表中通用的查询：

```java
public Customer queryForCustomers(String sql,Object... args){
    Connection conn = null;
    PreparedStatement ps = null;
    ResultSet rs = null;
    try{
        conn = JDBCUtils.getConnection();
        ps = conn.prepareStatement(sql);

        for (int i = 0; i < args.length; i++) {
            ps.setObject(i+1,args[i]);
        }

        rs = ps.executeQuery();

        //获取结果集的元数据（数据的数据）：ResultSetMetaData
        ResultSetMetaData rsmd = rs.getMetaData();
        //通过ResultSetMetaData获取结果集中的列数
        int columCount = rsmd.getColumnCount();

        if(rs.next()){

            Customer cust = new Customer();

            //处理结果集一行数据中的每一个列
            for (int i = 0; i < columCount; i++) {
                //获取列值，下标从1开始
                Object columValue = rs.getObject(i+1);
                //获取每个列的列名
                String columName = rsmd.getColumnName(i+1);

                //给cust对象指定的columName属性，赋值为columValue：通过反射
                Field field = Customer.class.getDeclaredField(columName);
                field.setAccessible(true);
                field.set(cust,columValue);
            }
            return cust;
        }
    } catch (Exception e) {
        e.printStackTrace();
    }finally {
        JDBCUtils.closeResource(conn,ps,rs);
    }
    return null;
}
```

若数据库中列名和java类中的属性名不同，可以通过声明sql时使用类的属性名取别名的方式在ResultSet中进行转化

```java
getColumnName(int index);//获取列的列名
getColumnLabel(int index);//获取列的别名，没有别名是就是列名
```

查询操作的流程：

![image-20210718000410329](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210718000834.png)

在不同表中通用的查询：

结果只有一行

```java
 public <T> T getInstance(Class<T> clazz,String sql,Object... args){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try{
            conn = JDBCUtils.getConnection();
            ps = conn.prepareStatement(sql);

            for (int i = 0; i < args.length; i++) {
                ps.setObject(i+1,args[i]);
            }

            rs = ps.executeQuery();

            //获取结果集的元数据（数据的数据）：ResultSetMetaData
            ResultSetMetaData rsmd = rs.getMetaData();
            //通过ResultSetMetaData获取结果集中的列数
            int columCount = rsmd.getColumnCount();

            if(rs.next()){

                T t = clazz.newInstance();

                //处理结果集一行数据中的每一个列
                for (int i = 0; i < columCount; i++) {
                    //获取列值，下标从1开始
                    Object columValue = rs.getObject(i+1);
                    //获取每个列的列名
                    String columLabel = rsmd.getColumnLabel(i+1);

                    //给t对象指定的columName属性，赋值为columValue：通过反射
                    Field field = clazz.getDeclaredField(columLabel);
                    field.setAccessible(true);
                    field.set(t,columValue);
                }
                return t;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.closeResource(conn,ps,rs);
        }
        return null;
    }

    @Test
    public void testGetInstance(){
        String sql = "select order_id orderId,order_name orderName from `order` where order_id = ?";
        Order order = getInstance(Order.class,sql,1);
        System.out.println(order);
    }
```

结果有多行

```Java
 public <T> List<T> getForList(Class<T> clazz,String sql,Object... args){
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try{
            conn = JDBCUtils.getConnection();
            ps = conn.prepareStatement(sql);

            for (int i = 0; i < args.length; i++) {
                ps.setObject(i+1,args[i]);
            }

            rs = ps.executeQuery();

            //获取结果集的元数据（数据的数据）：ResultSetMetaData
            ResultSetMetaData rsmd = rs.getMetaData();
            //通过ResultSetMetaData获取结果集中的列数
            int columCount = rsmd.getColumnCount();

            ArrayList<T> list = new ArrayList<T>();
            while(rs.next()){

                T t = clazz.newInstance();

                //处理结果集一行数据中的每一个列
                for (int i = 0; i < columCount; i++) {
                    //获取列值，下标从1开始
                    Object columValue = rs.getObject(i+1);
                    //获取每个列的列名
                    String columLabel = rsmd.getColumnLabel(i+1);

                    //给t对象指定的columName属性，赋值为columValue：通过反射
                    Field field = clazz.getDeclaredField(columLabel);
                    field.setAccessible(true);
                    field.set(t,columValue);
                }
                list.add(t);
            }
            return list;
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.closeResource(conn,ps,rs);
        }
        return null;
    }

    @Test
    public void testGetForList(){
        String sql = "select order_id orderId,order_name orderName from `order` where order_id < ?";
        List<Order> list = getForList(Order.class,sql,5);
        list.forEach(System.out::println);
    }
```

**使用PreparedStatement好处：**

1. PreparedStatement操作Blob的数据，而Statement做不到
2. PreparedStatement可以实现更高效的批量操作

**tips：**



* ```java
  ps.execute();//ps是PreparedStatement的实例
  ```

此操作返回的是boolean类型，如果执行的是查询操作，有返回结果，则此方法返回true；如果执行的是增、删、改操作，没有返回结果，则此方法返回false

* ```java
  ps.executeUpdate();
  ```

此操作返回sql语句执行后影响的行数 

