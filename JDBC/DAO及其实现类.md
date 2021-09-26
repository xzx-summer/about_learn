# DAO及其实现类

**BaseDao**

封装了针对于数据表的通用的操作，不能创建它的对象，声明为抽象类

1. 没有返回值或返回被影响的行数的update方法，用于增删改和DDL

2. 返回一行查询数据的getInstance方法

3. 返回多行查询数据的getForList方法

4. 返回查询特殊值的getValue方法

   ```java
   public <E> E getValue(Connection conn, String sql, Object... args){
       PreparedStatement ps = null;
       ResultSet rs = null;
       try {
           ps = conn.prepareStatement(sql);
   
           for (int i = 0; i < args.length; i++) {
               ps.setObject(i+1,args[i]);
           }
   
           rs = ps.executeQuery();
           if(rs.next()){
               return (E)rs.getObject(1);
           }
       } catch (SQLException throwables) {
           throwables.printStackTrace();
       } finally {
           JDBCUtils.closeResource(null,ps,rs);
       }
       return null;
   }
   ```

count(*)返回类型是long

**Dao接口**

规范针对于具体表的操作

**DaoImpl实现类**

继承BaseDao和对应的接口

**DaoImpl的单元测试**

IDEA中用Ctrl+Shift+T

Eclipse中右击Impl类的文件$\rightarrow$New$\rightarrow$JUnit Test Case，默认下一步，选择要测试的类和方法

**优化**

在每个DaoImpl中已经知道对哪个表进行操作了，传入的类是固定的，所以考虑去掉

![image-20210720225050886](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210720225050.png)

![image-20210720225007051](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210720225007.png)

![image-20210720225151773](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210720225151.png)

![image-20210720225251324](https://raw.githubusercontent.com/xzx-summer/image/main/img/20210720225258.png)

再想办法获取Impl类父类的泛型给父类的clazz实例化

```java
public class BaseDao<T> {

    private Class<T> clazz = null;

    {
        //获取当前BaseDao的子类继承父类中的泛型
        Type genericSuperclass = this.getClass().getGenericSuperclass();
        ParameterizedType parameterizedType = (ParameterizedType) genericSuperclass;
        
        Type[] types = parameterizedType.getActualTypeArguments();//获取了父类的泛型参数
        clazz = (Class<T>) types[0];//泛型的第一个参数
    }

    public T getInstance(Connection conn,String sql,Object... args){
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
            JDBCUtils.closeResource(null,ps,rs);
        }
        return null;
    }

    public <E> E getValue(Connection conn, String sql, Object... args){
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            ps = conn.prepareStatement(sql);

            for (int i = 0; i < args.length; i++) {
                ps.setObject(i+1,args[i]);
            }

            rs = ps.executeQuery();
            if(rs.next()){
                return (E)rs.getObject(1);
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.closeResource(null,ps,rs);
        }
        return null;
    }

}
```