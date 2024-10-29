---
title: PreparedStatement和statement的区别
abbrlink: b3986604
date: 2024-04-02 14:35:57
---

# PreparedStatement和statement的区别

**一、PreparedStatement概述**

* PreparedStatement是statement的子类，它的实例对象可以通过调用Connection.preparedStatement()方法来获得，相对于Statement对象，PreparedStatement可以防止SQL注入。
* 另外Statement会使得数据库频繁编译SQL，有可能会造成数据库缓冲区溢出。PreparedStatement可以对SQL进行预编译，从而提高数据库的执行效率。并且PreparedStatement中对于SQL语句中的参数允许使用占位符的形式进行替换，简化SQL语句的简写。

**二、PreparedStatement与Statement的区别**

1.相同点

* 两者都是用来执行SQL语句的。

2.不同点

* 1.Statement不安全，不能进行预编译，不能使用占位符，容易造成SQL语句拼接错误。
* 2.PreparedStatement是安全的的，可以预编译SQL语句，另外可以使用“？”占位符，从而使得SQL语句更加简洁清晰，还有效率高。

**三、利用PreparedStatement来实现增删该查** 

* 首先还是我们对前面的学生表来进行增删该查。

```
 import java.sql.Connection;
 import java.sql.DriverManager;
 import java.sql.PreparedStatement;
 import java.sql.ResultSet;
 import java.util.ArrayList;
 import java.util.Scanner;
 
 public class Insert1 {
     public static void main(String[] args) throws Exception {
         //1.连接数据库
         Class.forName("com.mysql.jdbc.Driver");
         //2.获取数据库的连接
         String username = "root";
         String password = "123456";
         String url = "jdbc:mysql://localhost:3306/school";
         Connection connection = DriverManager.getConnection(url, url, password);
         //3.编写SQL语句,使用？作为占位符
         Scanner scanner = new Scanner(System.in);
         System.out.println("请输入你要查询的学号");
         String StudentNo = scanner.nextLine();
         System.out.println("请输入你的密码");
         String LoginPwd = scanner.nextLine();
         String sql = "select * from  student where StudentNo=?and LoginPwd =?";
         PreparedStatement statement = connection.prepareStatement(sql);
         statement.setObject(1, StudentNo);
         statement.setObject(2, LoginPwd);
         ResultSet resultSet = statement.executeQuery();
         while (resultSet.next()) {
             System.out.println(resultSet.getObject(1));
             System.out.println(resultSet.getObject(2));
             System.out.println(resultSet.getObject(3));
         }
     }
 }
```

**我们可以看出上面的方法用起来代码显得非常臃肿，因此我们 可以编写一个工具类来实现增删该查，到时候我们只需要传入需要的参数就OK。**

**四、利用编写的工具类来实现增删该查** **1.首先还是先写一个配置文件**

```
 driver = com.mysql.jdbc.Driver
 username = root
 password = 123456
 url = jdbc:mysql://localhost:3306/school?useSSL=true
```

**2.编写工具类** **在这个工具类中可以仔细分析它实现的功能，设计的还是非常巧妙地。**

```
 package org.westos.pojo;
 
 
 import java.io.IOException;
 import java.io.InputStream;
 import java.sql.*;
 import java.util.Properties;
 
     public class JDBCUtils {
         private static String driver = null;
         private static String username = null;
         private static String password = null;
         private static String url = null;
     static {
         try {
             //1、加载配置文件
             InputStream stream = JDBCUtils.class.getClassLoader().getResourceAsStream("database.properties");
             Properties properties = new Properties();
             properties.load(stream);
             //2.加载配置文件
             driver = properties.getProperty("driver");
             password = properties.getProperty("password");
             username = properties.getProperty("username");
             url = properties.getProperty("url");
         } catch (IOException e) {
             e.printStackTrace();
         }
     }
     //获取数据库连接对象
     public static Connection getConnection()throws Exception{
         //加载数据库驱动
         Class.forName(driver);
         Connection connection = DriverManager.getConnection(url,username,password);
         return connection;
     }
     //查询
     public static ResultSet query(Connection connection,PreparedStatement preparedStatement,String sql,ResultSet resultSet,Object[]params)throws Exception{
         preparedStatement = connection.prepareStatement(sql);
         for (int i = 0; i < params.length; i++) {
             preparedStatement.setObject(i+1,params[i]);
         }
          resultSet = preparedStatement.executeQuery();
         return resultSet;
     }
     //更改
     public static int update(Connection connection,PreparedStatement preparedStatement,String sql,Object[]params)throws Exception{
          preparedStatement = connection.prepareStatement(sql);
         for (int i = 0; i < params.length; i++) {
             preparedStatement.setObject(i+1,params[i]);
         }
         int i = preparedStatement.executeUpdate();
         return i;
     }
     //释放资源
     public static <prepareStatement> void closeAll(ResultSet resultSet, Connection connection,PreparedStatement preparedStatement){
 
         if (preparedStatement != null){
             try {
                preparedStatement.close();
             } catch (SQLException e) {
                 e.printStackTrace();
             }
         }
         if (connection != null){
             try {
                 connection.close();
             } catch (SQLException e) {
                 e.printStackTrace();
             }
         }
         if (resultSet != null){
             try {
                 resultSet.close();
             } catch (SQLException e) {
                 e.printStackTrace();
             }
         }
     }
 }
```

**3.通过工具类来进行查询**

```
 package org.westos.pojo;
 
 import org.jboss.arquillian.test.spi.annotation.TestScoped;
 import org.junit.Test;
 
 import java.sql.Connection;
 import java.sql.PreparedStatement;
 import java.sql.ResultSet;
 import java.util.ArrayList;
 import java.util.Scanner;
 
 public class sqlScentence {
     @Test
     public void query() throws Exception {
         Connection connection = null;
         PreparedStatement preparedStatement = null;
         ArrayList<Object> objects = new ArrayList<>();
         ResultSet resultSet = null;
         String  StudentNo = null;
         String password = null;
         String sql = null;
         Scanner scanner = new Scanner(System.in);
         System.out.println("请输入你要查询的学号");
         StudentNo  = scanner.nextLine();
         System.out.println("请输入你的密码");
          password = scanner.nextLine();
         connection = JDBCUtils.getConnection();
           sql = "select * from  student where StudentNo=?and LoginPwd =?";
           objects.add(StudentNo);
           objects.add(password);
         Object[] array = objects.toArray();
         ResultSet query = JDBCUtils.query(connection, preparedStatement, sql, resultSet, array);
         while (query.next()){
             System.out.println(query.getObject(1));
             System.out.println(query.getObject(2));
             System.out.println(query.getObject(3));
         }
     }
     @Test
     public void insert ()throws Exception{
         Connection connection = null;
         PreparedStatement preparedStatement = null;
         ArrayList<Object> objects = new ArrayList<>();
         connection = JDBCUtils.getConnection();
         objects.add(1018);
         objects.add(1234567);
         objects.add("赵文君");
         objects.add(2);
         objects.add(4);
         objects.add("28282828282");
         objects.add("重庆");
         objects.add("1986-12-11 00:00:00");
         objects.add("test1@bdqn.cn");
         objects.add("450323198612314232");
         Object[]  array = objects.toArray();
         String sql = "insert into student (StudentNo,LoginPwd,StudentName,Sex,GradeId,Phone,Address,BornDate,Email,IdentityCard)values(?,?,?,?,?,?,?,?,?,?)";
         int update = JDBCUtils.update(connection, preparedStatement, sql, array);
         if (update>0){
             System.out.println(666666666);
         }
     }
     @Test
     public void update()throws Exception{
         Connection connection = null;
         PreparedStatement preparedStatement = null;
         ArrayList<Object> objects = new ArrayList<>();
         objects.add(1000);
         objects.add(2);
         Object[] array = objects.toArray();
          connection = JDBCUtils.getConnection();
         String sql = "update  result set StudentResult =100 where StudentNo = ? and SubjectNo= ? ";
         int update = JDBCUtils.update(connection, preparedStatement, sql, array);
         if (update>0){
             System.out.println(666666);
         }
     }
     @Test
     public void delete()throws Exception{
         Connection connection = null;
         PreparedStatement preparedStatement = null;
         ArrayList<Object> objects = new ArrayList<>();
         connection = JDBCUtils.getConnection();
         objects.add(1000);
         Object[] array = objects.toArray();
         String sql = "delete from student where StudentNo=?";
         int i = JDBCUtils.update(connection, preparedStatement, sql, array);
         if (i>0){
             System.out.println(67899);
         }
     }
 }
```
