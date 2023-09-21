1. Websocket是什么

   1. Websocket是一种网络通讯协议
   2. 和HTTP协议的区别
      1. HTTP是一种无状态，请求、响应模型的单向应用层协议，服务器端无法主动给客户端发送消息

2. Websocket使用

   1. 客户端

      1. websocket对象

         1. ```js
            url=ws：//ip:port/url
            var ws = new websocket(url);
            ```

      2. 事件

         1. ```js
            //建立连接回调函数
            ws.onopen
            //客户端接收消息时回调函数
            ws.onmessage
            //发生错误时回调函数
            ws.onerror
            //关闭连接时触发
            ws.onclose
            ```

      3. 方法

         1. ```js
            send()
            ```

   2. 服务端

      1. Endpoint对象

         1. ```java
            编程式
            继承Endpoint，并重写父类方法
                
            注解式
            @ServerEndpoint(value="url")
                
            @OnOpen
            @OnMessage
            @OnError
            @OnClose    
            ```

      2. RemoteEndpoint对象

         1. ```java
            RemoteEndpoint对象向客户端发送消息
            //同步消息发送
            Session.getBasicRemote().sendXXX()
            //异步消息发送
            Session.getAsyncRemote().sendXXX()
            ```

         2. 