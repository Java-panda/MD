1. Mysql底层重要构成

   1. 连接池组件
   2. 管理服务和工具组件
   3. SQL接口组件
   4. 查询分析器组件
   5. 优化器组件 
   6. 缓存组件
   7. 插拔式存储引擎
   8. 物理文件

2. 主要引擎的区别

   - | Feature                                | MyISAM       | Memory           | InnoDB       | Archive      | NDB          |
     | :------------------------------------- | :----------- | :--------------- | :----------- | :----------- | :----------- |
     | B-tree indexes                         | Yes          | Yes              | Yes          | No           | No           |
     | Backup/point-in-time recovery (note 1) | Yes          | Yes              | Yes          | Yes          | Yes          |
     | Cluster database support               | No           | No               | No           | No           | Yes          |
     | Clustered indexes                      | No           | No               | Yes          | No           | No           |
     | Compressed data                        | Yes (note 2) | No               | Yes          | Yes          | No           |
     | Data caches                            | No           | N/A              | Yes          | No           | Yes          |
     | Encrypted data                         | Yes (note 3) | Yes (note 3)     | Yes (note 4) | Yes (note 3) | Yes (note 3) |
     | Foreign key support                    | No           | No               | Yes          | No           | Yes (note 5) |
     | Full-text search indexes               | Yes          | No               | Yes (note 6) | No           | No           |
     | Geospatial data type support           | Yes          | No               | Yes          | Yes          | Yes          |
     | Geospatial indexing support            | Yes          | No               | Yes (note 7) | No           | No           |
     | Hash indexes                           | No           | Yes              | No (note 8)  | No           | Yes          |
     | Index caches                           | Yes          | N/A              | Yes          | No           | Yes          |
     | Locking granularity                    | Table        | Table            | Row          | Row          | Row          |
     | MVCC                                   | No           | No               | Yes          | No           | No           |
     | Replication support (note 1)           | Yes          | Limited (note 9) | Yes          | Yes          | Yes          |
     | Storage limits                         | 256TB        | RAM              | 64TB         | None         | 384EB        |
     | T-tree indexes                         | No           | No               | No           | No           | Yes          |
     | Transactions                           | No           | No               | Yes          | No           | Yes          |
     | Update statistics for data dictionary  | Yes          | Yes              | Yes          | Yes          | Yes          |

   1. Notes:
      1. Implemented in the server, rather than in the storage engine.
      2. Compressed MyISAM tables are supported only when using the compressed row format. Tables using the compressed row format with MyISAM are read only.
      3. Implemented in the server via encryption functions.
      4. Implemented in the server via encryption functions; In MySQL 5.7 and later, data-at-rest encryption is supported.
      5. Support for foreign keys is available in MySQL Cluster NDB 7.3 and later.
      6. Support for FULLTEXT indexes is available in MySQL 5.6 and later.
      7. Support for geospatial indexing is available in MySQL 5.7 and later.
      8. InnoDB utilizes hash indexes internally for its Adaptive Hash Index feature.
      9. See the discussion later in this section.

3. InnoDB架构

   1. 后台线程

      1. Master Thread
         1. 主线程主要负责将缓冲池的数据异步刷新到磁盘
      2. IO Thread
         1. insert buffer thread(默认1个)
         2. log thread(默认1个)
         3. read thread(innodb_read_io_threads默认4个)
         4. write thread(innodb_write_io_threads默认4个)
      3. Purge Thread
         1. 事务提交后,回收undo页
         2. innodb_purge_threads默认四个
      4. Page Cleaner THread
         1. 负责脏页的刷新操作
         2. innodb_page_cleaners默认1个

   2. 内存(缓冲区)

      1. 缓冲池

         1. Innodb基于磁盘存储,按页管理数据,因为磁盘IO大幅度降低性能,因而采用缓冲池技术提高整体性能,本质就是开辟一块内存空间拿来临时存放数据,数据读写时先在缓冲池中进行,通过checkpoint机制将数据刷新回磁盘,缓冲池大小直接决定数据库整体性能[innodb_buffer_pool_size默认128M],同时可以通过设置[innodb_buffer_pool_instances默认1]来设置缓冲池的个数

         2. 构成

            ![](C:\Users\77023\Desktop\MD笔记\MD\Notes\数据库\Mysql\image-20220830201952919-16618598500921.png)

            1. 索引页
            2. 数据页
            3. 插入缓冲
            4. 自适应哈希索引
            5. 锁信息
            6. 数据字典信息

         3. LRU List/Free List/Flush List

            1. LRU(Latest Recent Used):最新最近使用算法
               1. Innodb根据数据使用频率进行排序,当缓冲区无法放入新数据时先释放LRU尾端数据然后新数据插入,Innodb采用特殊LRU算法,并非将最新数据插入最后而是Midpoint位置[innodb_old_blocks_pct默认37],列表的倒数3/8处(我猜应该是1/e),如果直接放在首部遇到某些sql查询量太大会导致缓冲区原有高频数据被刷掉.
            2. Free List
               1. 还未被占用的空闲页,当LUR需要新的页的时候,从Free List中取得,如果Free List用完,则根据LRU算法,清空尾列,当old页变为新的页称之为page made young
            3. Flush List
               1. LRU中的页被修改后,称之为脏页(dirty page),此时缓冲区的数据和磁盘数据不一致,通过Checkpoint机制刷回磁盘

         4. InnoDB状态

            1. 使用show engine innodb status查看引擎状态(1页=16K)

            2. | 参数名             | 含义                                                        |
               | ------------------ | ----------------------------------------------------------- |
               | Buffer pool size   | 缓冲区页数                                                  |
               | Free buffers       | Free List页数                                               |
               | Database pages     | LRU LIst页数                                                |
               | Old database pages | LRU中Old页数[innodb_old_blocks_pct*LRU]                     |
               | Modified db pages  | Flush List页数(脏页页数)                                    |
               | Pages made young   | 从Old移入New的页数                                          |
               | not young          | innodb_old_blocks_time导致未能从Old移入New的页数            |
               | unzip_LRU          | 1,2,4,8等不足16K的压缩页数,是LRU的子集,采用伙伴算法分配内存 |
               | seg size           | 插入缓冲区页数                                              |

      2. 重做日志缓冲

         1. 重做日志放入这个缓冲区,每一秒钟刷新到日志文件,大小可以由[innodb_log_buffer_size默认16M]设置
         2. 刷新机制
            1. Master Thread每秒将重做日志缓冲到日志
            2. 事务提交时
            3. 重做日志缓冲区空间不足1/2时

      3. 额外内存池

         1. 存放一些其他对象的额外缓冲区

   3. Checkpoint机制

      1. 为了解决脏页从缓冲池强制刷新到磁盘

      2. 如果从缓冲池刷新数据到磁盘出现宕机,则会导致数据丢失,因而采用Write ahead log机制,先写重做日志,再修改页,出现问题通过重做日志恢复数据,保证了持久性(Durability)

      3. 作用

         1. 降低缓冲池的数据量,缩短宕机后数据的恢复时间
         2. 缓冲池不够用时,刷新脏页到磁盘
         3. 重做日志不可用时,刷新脏页到磁盘

      4. 分类

         1. Sharp Checkpoint:数据库关闭时时一次性刷新所有脏页到磁盘,即[innodb_fast_shutdown默认1]

         2. Fuzzy Checkpoint:数据库运行时一次性刷新则会影响可用性,因而部分刷新

            1. Master Thread Checkpoint:主线程每隔一段时间刷新Flush List脏页

            2. FLUSH_LRU_LIST Checkpoint:刷新LRU中尾部脏页到磁盘

            3. Async/Sync Flush Checkpoint:重做日志不可用时刷新

               ``` 
               待刷日志=重做日志-刷回日志
               异步阈值=0.75*重做日志缓冲区大小
               同步阈值=0.90*重做日志缓冲区大小
               
               1.待刷日志<异步阈值	此时不刷新
               2.异步阈值<待刷日志<同步阈值	刷新到 待刷日志<异步阈值
               3.待刷日志>同步阈值	刷新到 待刷日志<异步阈值
               
               第三种情况一般不会发生,除非重做日志缓冲区设置太小,或者大量插入数据
               ```

            4. Dirty Page too much Checkpoint:脏页太多刷新,[innodb_max_dirty_pages_pct默认90],当超过该比例时强制刷新

   4. Master Thread工作机制

      1. 由多个Loop组成

         1. Loop:主循环

         2. Background Loop:后台循环

         3. Flush Loop:刷新循环

         4. Suspend Loop:暂停循环

            ```java
            void master_thread() {
            goto loop:
            loop:
                for(int i = 0; i<10; i++){
                    thread_sleep(1) // sleep 1 second 
                    do log buffer flush to disk 
                    if ( last_one_second_ios < 5% innodb_io_capacity )
                        do merge 5% innodb_io_capacity insert buffer
                    if( buf_get_modified_ratio_pct > innodb_max_dirty_pages_pct )
                        do buffer pool flush 100% innodb_io_capacity dirty page 
                    if( no user activity )
                        goto backgroud loop
                }
                if ( last_ten_second_ios < innodb_io_capacity )
                    do buffer pool flush 100% innodb_io_capacity dirty page
                do merge 5% innodb_io_capacity insert buffer
                do log buffer flush to disk 
                do full purge
            
                if ( buf_get_modified_ratio_pct > 70% )
                    do buffer pool flush 100% innodb_io_capacity dirty page 
                else
                    do buffer pool flush 10% innodb_io_capacity dirty page 
                goto loop 
                    
                background loop:
                do full purge
                do merge 100% innodb_io_capacity insert buffer
                if not idle:
                    goto loop: 
                else:
                    goto flush loop
            
                flush loop:
                    do buffer pool flush 100% innodb_io_capacity dirty page 
                    if(buf_get_modified_ratio_pct> innodb_max_dirty_pages_pct )
                        goto flush loop 
                goto suspend loop 
                        
                suspend loop: 
                suspend_thread() 
                waiting event 
                goto loop:
            }
            ```

   5. Innodb关键特性

      1. 插入缓冲(Insert Buffer)
         1. 
      2. 两次写(Double Write)
      3. 自适应哈希索引(Adaptive Hash Index)
      4. 异步IO(Async IO)
      5. 刷新邻接页(Flush Neighbor Page)
      6. 