1. 安装Docker

   1. 卸载既存的docker

      1. ```
         sudo yum remove docker \
         docker-client \
         docker-client-latest \
         docker-common \
         docker-latest \
         docker-latest-logrotate \
         docker-logrotate \
         docker-engine
         ```

         

   2. 安装必须的依赖

      1. ```
         sudo yum install -y yum-utils \
         device-mapper-persistent-data \
         lvm2
         ```

   3. 设置 docker repo 的 yum 位置

      1. ```
         sudo yum-config-manager \
         --add-repo \
         https://download.docker.com/linux/centos/docker-ce.repo
         ```

   4. 安装 docker，以及 docker-cli

      1. ```
         sudo yum install docker-ce docker-ce-cli containerd.io
         ```

   5. 启动docker

      1. ```
         sudo systemctl start docker
         ```

   6. 设置开机自启

      1. ```
         sudo systemctl enable docker
         ```

2. 安装Mysql

   1. 下载镜像

      1. ```
         #默认下载最新
         docker pull mysql
         ```

   2. 启动实例

      1. ```
         docker run -p 3306:3306 --name mysql \
         -v /mydata/mysql/log:/var/log/mysql \
         -v /mydata/mysql/data:/var/lib/mysql \
         -v /mydata/mysql/conf:/etc/mysql/conf.d \
         -e MYSQL_ROOT_PASSWORD=root \
         -d mysql
         ```

   3. 自定义配置文件

      1. vi /mydata/mysql/conf/my.cnf

      2. ```
         [client]
         default-character-set=utf8
         [mysql]
         default-character-set=utf8
         [mysqld]
         init_connect='SET collation_connection = utf8_unicode_ci' init_connect='SET NAMES utf8' character-set-server=utf8
         collation-server=utf8_unicode_ci
         skip-character-set-client-handshake
         skip-name-resolve
         ```

   4. docker连接mysql客户端

      1. ```
         docker exec -it mysql mysql -uroot -proot
         ```

   5. 设置mysql自启动

      1. ```
         docker update mysql --restart=always
         ```

         

3. 安装Redis

   1. 下载Redis

      1. ```
         docker pull redis
         ```

   2. 启动redis实例

      1. mkdir -p /mydata/redis/conf

      2. touch /mydata/redis/conf/redis.conf

      3. ```
         docker run -p 6379:6379 --name redis -v /mydata/redis/data:/data \
         -v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
         -d redis redis-server /etc/redis/redis.conf
         ```

   3. docker连接redis客户端

      1. ```
         docker exec -it redis redis-cli
         ```

         

   4. redis自启动

      1. ```
         docker update redis --restart=always
         ```

4. 安装Zookeeper

   1. 下载Zookeeper

      1. ```bash
         docker pull zookeeper
         ```

   2. 启动zookeeper

      1. ```
         #创建数据存储目录
         mkdir -p /usr/local/zookeeper/data
         
         docker run -d -e TZ="Asia/Shanghai" -p 2181:2181 -v /usr/local/zookeeper/data:/data --name zookeeper --restart always zookeeper
         ```
   3. 连接zookeeper客户端
   
      1. ```bash
         docker run -it --rm --link zookeeper:zookeeper zookeeper zkCli.sh -server zookeeper
         ```
   
      2. 