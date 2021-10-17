### Hive

1. 环境搭建

2. 配置文件

   ```xml
   <?xml version="1.0"?>
   <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
   <configuration>
   	<!-- jdbc 连接的 URL -->
   	<property>
   		<name>javax.jdo.option.ConnectionURL</name>
   		<value>jdbc:mysql://hadoop102:3306/metastore?useSSL=false</value>
   	</property>
   	<!-- jdbc 连接的 Driver-->
   	<property>
   		<name>javax.jdo.option.ConnectionDriverName</name>
   		<value>com.mysql.jdbc.Driver</value>
   	</property>
   	<!-- jdbc 连接的 username-->
   	<property>
   		<name>javax.jdo.option.ConnectionUserName</name>
   		<value>root</value>
   	</property>
   	<!-- jdbc 连接的 password -->
   	<property>
   		<name>javax.jdo.option.ConnectionPassword</name>
   		<value>root</value>
   	</property>
   	<!-- Hive 元数据存储版本的验证 -->
   	<property>
   		<name>hive.metastore.schema.verification</name>
   		<value>false</value>
   	</property>
   	<!--元数据存储授权-->
   	<property>
   		<name>hive.metastore.event.db.notification.api.auth</name>
   		<value>false</value>
   	</property>
   	<!-- Hive 默认在 HDFS 的工作目录 -->
   	<property>
   		<name>hive.metastore.warehouse.dir</name>
   		<value>/user/hive/warehouse</value>
   	</property>
   	<!-- 指定存储元数据要连接的地址 -->
   	<property>
   		<name>hive.metastore.uris</name>
   		<value>thrift://hadoop102:9083</value>
   	</property>
   	<!-- 指定 hiveserver2 连接的 host -->
   	<property>
   		<name>hive.server2.thrift.bind.host</name>
   		<value>hadoop102</value>
   	</property>
   	<!-- 指定 hiveserver2 连接的端口号 -->
   	<property>
   		<name>hive.server2.thrift.port</name>
   		<value>10000</value>
   	</property>
   	<!-- 打印表头 -->
   	<property>
   		<name>hive.cli.print.header</name>
   		<value>true</value>
   	</property>
   	<!-- 打印库名 -->
   	<property>
   		<name>hive.cli.print.current.db</name>
   		<value>true</value>
   	</property>
   </configuration>
   
   ```

   

3. 基本命令

   1. hive -help

      ```sh
      #hiveservice.sh脚本服务启动
      hiveservice.sh start/status/stop/restart
      
      #登录退出客户端
      hive
      quit/exit/ctrl+c
      
      #查看帮助
      hive -help
      
      #直接不登录查询
      hive -e "select * from test"
      
      #执行xxx.sql文件查询
      hive -f xxx.sql
      
      #查看HDFS文件目录
      dfs -ls /;
      
      #进入到当前用户的根目录 /root 或/home/username
      #查看历史命令文件
      cat .hivehistory
      ```

4. 数据类型
   1. 基本数据类型
      1. TINYINT
      2. SMALINT
      3. INT
      4. BIGINT
      5. BOOLEAN
      6. FLOAT
      7. DOUBLE
      8. STRING
      9. TIMESTAMP
      10. BINARY
   2. 集合数据类型
      1. STRUCT
      2. MAP
      3. ARRAY