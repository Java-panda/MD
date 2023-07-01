1. 安装

2. 基本操作

   1. ​	登录

      1. ```bash
         #以sysdba的身份登录sys，密码sys
         sys as sysdba
         ```

   2. 创建用户

      1. ```bash
         #创建root用户，密码是root
         CREATE USER "root" IDENTIFIED BY "root";
         ```

   3. 切换用户

      1. ```bash
         connect root
         ```

3. Oracle使用JDBC

   1. ```java
      // JDBC 连接信息
      String jdbcUrl = "jdbc:oracle:thin:@localhost:1521:orcl";
      String username = "root";
      String password = "root";
      
      // JDBC 连接对象
      Connection connection = null;
      PreparedStatement preparedStatement = null;
      ResultSet resultSet = null;
      try {
          // 加载驱动程序
          Class.forName("oracle.jdbc.driver.OracleDriver");
      
          // 建立连接
          connection = DriverManager.getConnection(jdbcUrl, username, password);
      
          // 创建SQL
          String sql = "select * from contacts where contact_id=?";
      
          // 预编译sql
          preparedStatement = connection.prepareStatement(sql);
          preparedStatement.setInt(1,208);
      
          // 执行查询
          resultSet = preparedStatement.executeQuery();
      
          // 处理结果集
          ResultSetMetaData metaData = resultSet.getMetaData();
          int columnCount = metaData.getColumnCount();
          while (resultSet.next()){
              for (int i = 1; i <= columnCount; i++) {
                  System.out.println(resultSet.getString(i));
              }
          }
      } catch (ClassNotFoundException e) {
          e.printStackTrace();
      } catch (SQLException e) {
          e.printStackTrace();
      } finally {
          // 关闭资源
          connection.close();
          preparedStatement.close();
          resultSet.close();
      }
      ```

   2. 

