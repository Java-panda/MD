- Mongo和传统关系型数据库的对比

    | database    | database    | 数据库                              |
    | ----------- | ----------- | ----------------------------------- |
    | table       | collection  | 数据库表/集合                       |
    | row         | document    | 数据记录行/文档                     |
    | column      | field       | 数据字段/域                         |
    | index       | index       | 索引                                |
    | table joins |             | 表连接,MongoDB不支持                |
    | primary key | primary key | 主键,MongoDB自动将_id字段设置为主键 |

- Mongo语法

    - ```sql
        db.集合.save()
        db.集合.remove()
        db.集合.update()
        db.集合.find()
        ```

    - 