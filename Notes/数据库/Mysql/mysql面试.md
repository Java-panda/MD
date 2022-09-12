1. 一张表，里面有ID自增主键，当insert了17条记录之后，删除了第15,16,17条记录，再把Mysql重启，再insert一条记录，这条记录的ID是18还是15 ？ 

   8以下

   ​	Innodb:15(存在内存)

   ​	Myisam:18(存在文件)

   8以上

   ​	Innodb:18(存在重做日志)

   ​	Myisam:18(存在文件)

2. 事务

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

   3. 

   4. 

3. MVCC(multi-version-concurrent-control)多版本并发控制

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

5. Mysql索引失效场景

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

       