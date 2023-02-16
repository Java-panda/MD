MQ

- MQ基本概念

  - Message Quene:消息队列
  - 分布式系统通讯方式
    - 远程调用直接通信
    - 第三方中间件通信
  - 优势
    - 引用解耦
    - 异步提速
    - 削峰填谷
  - 劣势
    - 系统可用性降低
    - 系统复杂度增加
      - 消息丢失?
      - 重复消费?
      - 消费顺序?
    - 数据一致性问题
  - 应用前提
    - 生产者无需等待消费者的响应结果,接口返回值为空
    - 允许短暂的数据不一致性
  - 常见产品
    - RabbitMQ
      - 吞吐量:万级
      - 消息延迟:微秒级
      - 特性:并发高,延时低
    - RocketMQ
      - 吞吐量:万级
      - 消息延迟:毫秒级
      - 特性:扩展性强
    - ActiveMQ
      - 吞吐量:万级
      - 消息延迟:毫秒级
      - 特性:成熟度高
    - Kafka
      - 吞吐量:十万级
      - 消息延迟:毫秒级
      - 特性:适用于大数据领域
  - AMQP:(Advance Message Quene Protocol)高级消息队列协议
  - 常用端口
    - 管理控制台:localhost:15672
      - panda
      - Panda5201314
    - 客户端:5672

- 基本原理

  - Producer→Rabbit Broker→Exchange→Queue→Consumer

- 工作类型

  - 简单模式
    - ![Producer -> Queue -> Consuming: send and receive messages from a named queue.](https://www.rabbitmq.com/img/tutorials/python-one.png)
    - 一个生产者+一个队列+一个消费者
  - WorkQueues
    - ![Producer -> Queue -> Consuming: Work Queue used to distribute time-consuming tasks among multiple workers.](https://www.rabbitmq.com/img/tutorials/python-two.png)
    - 一个生产者+一个队列+多个消费者轮流消费
  - PubSub
    - ![Producer -> Queue -> Consuming: deliver a message to multiple consumers. This pattern is known as publish/subscribe](https://www.rabbitmq.com/img/tutorials/python-three.png)
    - 一个生产者+交换机(不写Routing Key)+多个队列+每个队列对应一个消费者
  - Routing
    - ![Producer -> Queue -> Consuming: subscribe to a subset of the messages only.](https://www.rabbitmq.com/img/tutorials/python-four.png)
    - 一个生产者+交换机(根据Routing Key分发到相应队列)+多个队列+每个队列对应一个消费者
  - Topic
    - ![Producer -> Queue -> Consuming: receiving messages based on a pattern (topics).](https://www.rabbitmq.com/img/tutorials/python-five.png)
    - 一个生产者+交换机(根据Routing Key模式匹配分发到相应队列)+多个队列+每个队列对应一个消费者
      - 模式匹配(和正则表达式正好相反)
        - *:匹配1个单词
        - #:匹配0或者多个单词

- 高级特性

  - 消息可靠性投递
    - 生产方
      - 确认回调
        - C→Exchange之间添加RabbitTemplate.setConfirmCallBack
      - 返回回调
        - Exchange→Queue之间添加RabbitTemplate.setReturnCallBack
    - 消费方Consumer ACK
      - 自定义消息监听器
        - basicAck:批量同意
        - basicNack:批量拒绝
        - basicReject:单个拒绝
    - Broker集群高可用
    - 持久化
  - 消费端限流
    - 设置setPrefetchCount数量控制每次拉取消息的个数
    - 设置手动ack
  - TTL:存活时间
    - 单个消息设置
    - 队列整体设置
  - 死信队列
    - 理解
      - 可以看成是java代码里面的finnaly,最终将会被执行的逻辑
    - 死信发生条件
      - 消息队列长度达到限制
      - 消息被拒绝
      - 消息过期
    - 设置方式
      - 创建一个新的死信交换机和一个队列与之匹配
      - 将一个打算加死信队列的队列属性中加入死信交换机名称和死信队列路由名称
  - 延迟队列
    - 使用TTL+死信队列组合完成
  - 日志监控
    - 可以开启日志记录插件,生产环境影响性能慎用
  - 消息可靠性分析和追踪
    - 消息可靠性
      - 消息补偿机制
    - 幂等性保证
      - 
  - 控制台管理

- 应用问题

  - 消息可靠性保证
    - 消息补偿机制
  - 消息幂等性/重复消费
    - 使用乐观锁,添加版本号,每次更新version=version+1

- 集群

  - 单机多结点伪集群
    - Node1.Port(5673,15673)
    - Node1.Port(5674,15674)
    - Node1.Port(5675,15675)
  - 多机多结点集群
    - Node1.Port(5673,15673)
    - Node2.Port(5673,15673)
    - Node3.Port(5673,15673)

  - 问题
    - 集群各节点之间数据不同步
      - 使用镜像队列
    - 结点宕机如何保证可用性
      - haproxy负载均衡