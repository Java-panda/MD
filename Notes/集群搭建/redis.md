一,安装

- windows

- - 获取安装包

  - - github:https://github.com/MicrosoftArchive/redis/releases(.zip)
    - 本地:[‪I:\InstallPackage\Redis-x64-3.0.504.zip](http://‪I:/InstallPackage/Redis-x64-3.0.504.zip)

  - 解压到path

  - cmd进入path

  - - 安装命令：redis-server.exe       --service-install redis.windows.conf --loglevel verbose
    - 启动服务命令：redis-server.exe --service-start
    - 关闭服务命令：redis-server.exe --service-stop

  - 校验

  - - 启动服务:redis-server.exe --service-start

    - 另开一个cmd进入path

    - - 调用服务:redis-cli.exe -h        127.0.0.1 -p 6379

      - - set x "y"
        - get x

  - 若成功读取x则安装成功

- linux

- - 下载获得redis-3.2.5.tar.gz后将它放入我们的Linux目录/opt
  - 解压命令:tar -zxvf      redis-3.2.5.tar.gz
  - 解压完成后进入目录:cd redis-3.2.5执行make(存在gcc)
  - 执行make install
  - 查看默认安装目录：usr/local/bin
  - 修改redis.conf文件将里面的daemonize no 改成 yes，让服务在后台启动
  - 启动命令：执行 redis-server  /…/redis.conf

二,命令

- DB

- - select 0-15 选择0-15号数据库,默认16个
  - dbsize 查看数据库存在的key个数
  - flushdb 清空当前库
  - flushall 清空所有库

- Key

- - keys * 查看当前数据库所有key
  - exists key 查看是否存在key
  - type key 查看key的类型
  - del key 删除某个key
  - expire key seconds 设置key的过期时间
  - ttl key 查看key的剩余过期时间

三,数据类型

- string

- - 最基本的数据类型

  - 二进制安全

  - 一个string最大512m

  - 常用命令

  - - set key value 添加kv键值对
    - setex key seconds       value 设置kv的同时设置过期时间
    - getset key value 获取并重新设置
    - get key
    - append key value 添加value到原始值的后面
    - strlen key 查询value的长度
    - setnx key value 当且仅当key不存在的时候添加kv
    - incr 自增仅限数字类型的string
    - decr 自减仅限数字类型的string
    - incrby/decrby key       step 自定义步长自增自减
    - setrange key start       end 复写某一个范围
    - getrange key start       end 获取某一个范围
    - mset key1 value1 key2 value2…
    - mget key1 key2
    - msetnx key1 value1 key2       value2…

- set

- list

- hash

- zset

四,事务(类似于同步代码快)

队列嵌套队列实现命令的事务效果

不具有原子性

执行失败后继续向后执行

编译器出现错误全不执行

运行期出现错误除错误部分,其他全部执行

multi{

指令集

(discard取消队列化)

}exec

解释:系统将指令集以队列的形式加入到一个指令集队列,然后再将该队列放入外层队列,外层队列决定指令的执行顺序