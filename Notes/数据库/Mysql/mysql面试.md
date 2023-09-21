1. 一张表，里面有ID自增主键，当insert了17条记录之后，删除了第15,16,17条记录，再把Mysql重启，再insert一条记录，这条记录的ID是18还是15 ？ 

   8以下

   ​	Innodb:15(存在内存)

   ​	Myisam:18(存在文件)

   8以上

   ​	Innodb:18(存在重做日志)

   ​	Myisam:18(存在文件)


1. 事务

   1. ACID

      1. Atomicity:原子性
      2. Consistency:一致性
      3. Isolation:隔离性
      4. Durability:持久性

   2. 事务的开启和结束

      ``` sql
      -- 开启事务
      begin;
      
      -- 回滚事务
      rollback;
      
      -- 提交事务
      commit;
      ```

   3. 并发事务问题

      1. 脏写
         1. A,B事务都在更新数据,但是B事务不知道A已经更新,而直接更新覆盖了A的更新
      2. 脏读
         1. 事务B读取了事务A的修改但未提交的数据
      3. 不可重复读
         1. 同一个事务内,多次读取同一个查询语句,结果不相同
      4. 幻读
         1. 事务B读取到了事务A新提交的数据,读多了

   4. 事务的隔离级别

      1. Read Uncommitted:读未提交
         1. A事务未提交B事务也可以读取到变化
         2. 问题:脏读,不可重复读,幻读
      2. Read Comimitted:读已提交
         1. A事务提交之后,B事务才可以读取到变化
         2. 问题:不可重复读,幻读
         3. 原理:MVCC每次读取最新的Read View版本
      3. Repeatable Read:可重复读()
         1. A事务提交之后,B事务读取到的仍然为第一次读到的结果
         2. 问题:幻读,脏写
         3. 原理:MVCC每次读取第一次的Read View版本
      4. Serializable:可串行化
         1. A事务未结束前,B事务一直阻塞等待
         2. 原理:相当于查询语句后面加上lock in share mode

   5. 设置事务隔离级别

      1. set tx_isolation='Read-Uncommitted','Read-Comimitted','Repeatable-Read','Serializable',

2. InnoDB和Myisam存储文件后缀格式

   1. InnoDB
      1. ibd

      2. frm（8.0废弃）

   2. Myisam
      1. frm（8.0sdi）

      2. myd

      3. myi

3. MVCC(multi-version-concurrent-control)多版本并发控制

   1. 当前读
   2. 快照读

4. Mysql锁

   1. 场景
      1. 读-读:无需加锁
      2. 读-写:MVCC(Innodb)
      3. 读-读:加锁同步
   2. 种类
      1. 表锁
         1. 开销小,加锁快,粒度大,并发性小,锁冲突高
      2. 行锁
         1. 开销大,加锁慢,粒度小,并发性大,锁冲突低
      3. 共享锁
      4. 排它锁
      5. 乐观锁
         1. 添加一个Version记录版本.每次更新带上Version,如果Version匹配则直接更新,否则更新失败
      6. 悲观锁

5. Mysql 索引类型

   1. Index:普通索引(可以重复)
   2. primary key:主键索引(唯一,非空)
   3. unique:唯一索引(唯一,可空)
   4. composite index:组合索引(组合唯一,第一个索引时才可生效)
   5. fulltext:全文索引(char,varchar,text的关键字搜索)

6. Mysql索引失效场景

   ``` sql
   CREATE TABLE `user` (
     `id` int NOT NULL AUTO_INCREMENT,
     `code` varchar(20) COLLATE utf8mb4_bin DEFAULT NULL,
     `age` int DEFAULT '0',
     `name` varchar(30) COLLATE utf8mb4_bin DEFAULT NULL,
     `height` int DEFAULT '0',
     `address` varchar(30) COLLATE utf8mb4_bin DEFAULT NULL,
     PRIMARY KEY (`id`),
     KEY `idx_code_age_name` (`code`,`age`,`name`),
     KEY `idx_height` (`height`)
   ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin
   
   INSERT INTO sue.user (id, code, age, name, height) VALUES (1, '101', 21, '周星驰', 175,'香港');
   INSERT INTO sue.user (id, code, age, name, height) VALUES (2, '102', 18, '周杰伦', 173,'台湾');
   INSERT INTO sue.user (id, code, age, name, height) VALUES (3, '103', 23, '苏三', 174,'成都');
   ```

   1. 不满足最左匹配原则

      1. select * from user where age=21

   2. 使用了select *

      1. 不遵循最左前缀原则但是满足索引覆盖

   3. 索引列上有计算

      1. select * from user where id+1=2

   4. 索引列上有函数

      1. select * from user where 

   5. 字段类型不同

      1. 123错写成'123'索引不会失效
      2. '123'错写成123索引会失效

   6. 模糊匹配左端有%

   7. 列对比

      1. select * from user where id=height

   8. 使用or关键字

      1. 使用or关键字时候要保证or其前后字段都要是索引字段

   9. Not in/Not Exists

      1. in(走索引)
      2. exists(走索引)
      3. not in(主键走,普通索引不走)
      4. not exists(不走索引)
      5. between and(走索引)

   10. Order By

       1. 不适用Where条件或者limit
       2. 对不同索引进行排序
       3. 不满足最左前缀原则
       4. 多个索引字段排序升降不同

7. SQL执行计划

   1. 使用方法
      1. Explain +Select语句
   2. select_type
      1. Simple:简单查询没有子查询
      2. Primary:复杂查询中的主干查询
      3. Subquery:From前的子查询
      4. Derived:From后的子查询
      5. Union:Union后的查询
   3. type
      1. System:系统级别
      2. Const:常数级别
      3. eq_ref:唯一索引
      4. ref:非唯一索引
      5. range:范围查找
      6. index:覆盖索引
      7. all:全表扫描
   4. Extra
      1. Useing Index:索引覆盖
      2. Useing Where:包含Where条件,同时查询的列没有被索引覆盖
      3. Using index condition:查询的列没有完全被索引覆盖
      4. Using temporary:产生临时表,一般需要进行优化
      5. Using filesort:需要外部排序,一般需要优化
      6. Select tables optimized away:对索引列使用了某些函数

8. SQL优化

   1. 什么是慢查询

      1. 查询时间长，超过阈值

   2. 导致慢查询的可能原因

      1. 子查询，连接查询嵌套过多
      2. 没有设计好合适的索引，导致全表扫描
      3. 本身数据量太大，考虑分库分表
      4. 表结构设计不合理，字段重复，冗余过多

   3. 开启慢查询日志

      1. ```bash
         # 开启慢查询日志
         slow_query_log = 1
         # 慢查询日志文件的路径和名称
         slow_query_log_file = D:/mysql-8.0.33-winx64/log/slow-query.log
         # 定义查询执行时间超过多少秒才被认为是慢查询（可以根据实际需求进行调整）
         long_query_time = 2
         ```

      2. 