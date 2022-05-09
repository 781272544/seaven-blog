---
title: JDBC-官方定义的一套操作所有关系型数据库的接口
date: 2022-04-29 16:13:27
tags: ["java","JDBC"]
categories: ["java"]
---

JDBC：官方定义的一套操作所有关系型数据库的接口。由各个数据库厂商去实现这个接口，提供数据库驱动jar包。

* 导入驱动jar包

* 注册驱动

  ```java
  Class.forName("com.mysql.jdbc.Driver");
  ```

* 获取数据库连接对象 connection

  ```java
  String url = "jdbc:mysql://localhost:3306/test";
  String user = "root";
  String password = "root";
  Connection conn = DriverManager.getConnection(url, user, password);
  ```


* 定义SQL

  ```java
  String sql = "select * from emp where salary>100";
  String sql2 = "update emp set name='luffy' where id=2";

  String sql3 = "insert into emp (id,name,salary) values (?,?,?)";
  String sql4 = "select * from emp where id=?";
  ```

* 获取执行SQL语句的对象 Statement

  ```java
  Statement st = conn.createStatement();
  ```

  ```java
  //PreparedStatement对带？的进行预编译
  PreparedStatement pst = conn.prepareStatement(sql3);
  PreparedStatement pst2 = conn.prepareStatement(sql4);
  ```

* 执行SQL

  ```java
  //executeQuery用于执行select操作
  ResultSet rs = st.executeQuery(sql);
  //executeUpdate用于执行insert、delete、update操作
  int result = st.executeUpdate(sql2);
  ```

  ```java
  //使用PreparedStatement
  pst.setInt(1,id);//id为已定义的变量
  pst.setString(2,name);
  pst.setDouble(3,salary);
  int result = pst.executeUpdate();

  //
  pst2.setInt(1,id);
  ResultSet rs = pst2.executeQuery();
  ```

  ​

* 处理结果

  ```java
  while(rs.next){
      
  }
  ```

* 释放资源

  ```java
  st.close();
  conn.close();
  ```

  ​
