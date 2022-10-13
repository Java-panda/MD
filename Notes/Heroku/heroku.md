- Heroku

  - 基本概念

    - Deploy,Manage,Run Application

    - Application:源码,依赖,Procfile构成

    - Procfile:声明应用类型以及启动应用程序的执行指令

    - ```shell
      #<process type>: <command>
      web: java -jar lib/foobar.jar $PORT
      worker:
      c
      ```

    - slug:编译后的可执行应用程序(jar)
    - dynos:运行app的所需容器环境
    - config vars:系统环境变量

  - 常用命令

    - ```shell
      #准备项目,创建Procfile文件(web: java -Dserver.port=$PORT -jar target/thymeleaf-0.0.1-SNAPSHOT.jar)
      准备项目代码
      #初始化git
      git init
      #提交代码到暂存区
      git add .
      #提交代码
      git commit -m "init"
      #登录Heroku
      heroku login
      #创建Heroku APP
      heroku create -a "thymeleafboot"
      #上传代码到远程
      git push heroku master
      #查看日志
      heroku logs -t
      #查看环境变量
      heroku config -a "thymeleafboot"
      #设置环境变量
      heroku config:set KEY=VALUE
      #获取环境变量
      heroku config:get KEY
      #重启
      heroku restart -a "thymeleafboot"
      #启动APP,并且打印出环境变量
      heroku run -a "thymeleafboot" printenv
      #启动APP,并且打印出环境变量
      heroku open -a "thymeleafboot"
      #查看dynas
      heroku ps
      #查看本月使用时间
      heroku ps -a thymeleafboot
      #查看历史发布记录
      heroku releases
      #回滚历史发布记录
      heroku releases:rollback 版本号
      
      
      
      #环境变量
      JAVA_HOME=/app/.jdk
      JDBC_DATABASE_URL=jdbc:mysql://us-cdbr-east-06.cleardb.net/heroku_e90fea40c5e9ca2?password=d778eb7b&reconnect=true&user=b2f0158ef0a735
      JAVA_OPTS=-Xmx300m -Xss512k -XX:CICompilerCount=2 -Dfile.encoding=UTF-8
      PWD=/app
      PORT=17889
      LINES=49
      HOME=/app
      SPRING_DATASOURCE_USERNAME=b2f0158ef0a735
      COLUMNS=199
      SPRING_DATASOURCE_URL=jdbc:mysql://us-cdbr-east-06.cleardb.net/heroku_e90fea40c5e9ca2?password=d778eb7b&reconnect=true&user=b2f0158ef0a735
      CLEARDB_DATABASE_URL=mysql://b2f0158ef0a735:d778eb7b@us-cdbr-east-06.cleardb.net/heroku_e90fea40c5e9ca2?reconnect=true
      JDBC_DATABASE_PASSWORD=d778eb7b
      JDBC_DATABASE_USERNAME=b2f0158ef0a735
      SHLVL=0
      LD_LIBRARY_PATH=/app/.jdk/jre/lib/amd64/server:
      PS1=\[\033[01;34m\]\w\[\033[00m\] \[\033[01;32m\]$ \[\033[00m\]
      SPRING_DATASOURCE_PASSWORD=d778eb7b
      PATH=/app/.heroku/bin:/app/.jdk/bin:/usr/local/bin:/usr/bin:/bin
      DYNO=run.5238
      _=/usr/bin/printenv
      ```

    - 