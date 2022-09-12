- 传统JDBC

  - ``` java
    try {
        // 0.获取连接信息
        InputStream resourceAsStream = PandaApplication.class.getClassLoader().getResourceAsStream("jdbc.properties");
        Properties props = new Properties();
        props.load(resourceAsStream);
        String driver = props.getProperty("driver");
        String url = props.getProperty("url");
        String username = props.getProperty("username");
        String password = props.getProperty("password");
    
        // 1.导包JDBC
        // 2.注册驱动
        Class.forName(driver);//System.setProperty("jdbc.drivers", driver);
        // 3.获取连接
        Connection connection = DriverManager.getConnection(url, username, password);
        // 4.编写SQL
        String sql="select * from employee where id=2";
        // 5.预编译SQL
        PreparedStatement ps = connection.prepareStatement(sql);
        // 6.执行SQL
        ResultSet rets = ps.executeQuery(sql);
        // 7.遍历结果集
        while (rets.next()) {
            System.out.println(rets);
        }
        // 8.关闭连接
        rets.close();
        ps.close();
        connection.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
    ```

- Mybatis
  - 持久层框架,只关注SQL,是一种ORM
  - 主配置文件
  - Mapper配置文件
  - 执行方式

- Mybatis中的设计模式
  - SqlSessionFactoryBuilder:建造者模式
  - SqlSessionFactory:工厂模式,单例模式

- XML映射文件

  - select
  - insert
  - update
  - delete
  - 参数
    - #{id}:sql占位符,相当于?,不存在sql注入问题
    - ${id}:变量代换,可以动态修改查询字段,或者查询表明,有sql注入风险

  