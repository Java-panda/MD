1. JUC包含什么

   1. 锁
      1. Lock
      2. Condition
      3. synchronized
   2. 并发集合
      1. ConcurrentHashMap
      2. CopyOnWriteArrayList
   3. 原子类
      1. AtomicInteger
      2. AtomicStampedReference
   4. 线程池
      1. Executors

2. 基本概念

   1. 进程:一个程序,一个车间
   2. 线程:一条生产线
   3. 并发:交替进行
   4. 并行:平行进行

3. 多线程状态(男人不玩舔玩偷)

   1. NEW
   2. RUNNABLE
   3. BLOCKED
   4. WAITING
   5. TIMED_WAITING
   6. TERMINATED

4. 并发三大特性(并发原子有序是显而易见的)

   1. 可见性
   2. 有序性
   3. 原子性

5. 线程创建方式

   1. 继承Thread
   2. 实现Runnable
   3. 实现Callable结合FutureTask实现有返回值的线程
   4. 线程池Executors
      1. 创建线程池
         1. Executors.newCachedThreadPool()
      2. 开启任务
         1. execute
      3. 结束线程池
         1. shutdown：所有线程执行完毕
         2. shutdownNow：立马结束interrupt所有线程

6. ThreadLocal

   1. 作用
      1. 线程隔离
      2. 简化传参
         1. 例如把用户信息存储在ThreadLocal中，可以方便后续业务，直接调用工具类获取用户信息
   2. 原理
      1. 每个线程内部维护一个ThreadLocalMap
      2. ThreadLocalMap中的Key是某一个ThreadLocal对象,值是该tl中的副本对象
   3. 和synchronized的区别
      1. ThreadLocal
         1. 空间换时间
         2. 并发度高
      2. synchronized
         1. 时间换空间
         2. 并发度低
   4. 问题
      1. 内存泄漏
         1. 原因:ThreadLocal生命周期可能比Thread短,且没有手动remove
         2. 解决方式
            1. 手动remove移除ThreadLocal中的对象
            2. 使用完ThreadLocal后就结束线程(使用线程池则不太现实)
            3. ThreadLocalMap中的Key使用弱引用,在get值的时候如果为空就会自动remove,

7. synchronized和Lock

   1. | synchronized | Lock                             |
      | ------------ | -------------------------------- |
      | JVM          | JDK                              |
      | 粒度大       | 粒度小                           |
      | 无需手动释放 | 需要手动释放                     |
      | 非公平       | 默认非公平，可以构造函数设置公平 |

8. Condition(await,signal,signalAll)

   1. ```java
      public class AwaitSignalExample {
          private Lock lock = new ReentrantLock();
          private Condition condition = lock.newCondition();
      
          public void before() {
              lock.lock();
              try {
                  System.out.println("before");
                  condition.signalAll();
              } finally {
                  lock.unlock();
              }
          }
      
          public void after() {
              lock.lock();
              try {
                  condition.await();
                  System.out.println("after");
              } catch (InterruptedException e) {
                  e.printStackTrace();
              } finally {
                  lock.unlock();
              }
          }
      }
      ```

9. 内存屏障:内存屏障前后代码不重排序

   1. LOADLOAD
   2. STORESTORE
   3. LOADSTORE
   4. STORELOAD

10. 缓存一致性MESI(没事)

   1. M修改（Modefiy）:该缓存行有效， 数据被修改了，和内存中的数据不一样，数据只存在于本缓存行中
   2. E独享（Exclusive）:该缓存行有效，数据和内存中的数据一致，数据只存在本缓存行中
   3. S共享（Shared）:该缓存行有效，数据和内存中的数据一致，数据同时存在于其他缓存中
   4. I无效（Invalid）:该缓存行数据无效

11. JMM(Java内存模型)

12. JMM指令(独裁用赋存些黄金锁起来)

    1. read:从主内存读取数据
    2. load:加载读取数据到工作内存
    3. use:用于计算
    4. assign:重新赋值结果到工作内存
    5. store:将工作内存的变量值存入主内存
    6. write:将新值写入主内存
    7. lock:加锁主内存变量,变成线程独占
    8. unlock:解锁

13. Volatile(非洲大草原随地可见禁止排便)

14. 可见性

15. (有序性)禁止指令重排

16. 不保证原子性

17. ``` java
       package com.panda.datastructrue.juc;
       
       import java.util.concurrent.TimeUnit;
       
       public class Volatile {
           //volatile保证可见性,线程二修改共享变量后会让线程一可见并结束循环
           private static volatile boolean flag=false;
           public static void main(String[] args) throws Exception {
               new Thread(new Runnable() {
                   @Override
                   public void run() {
                       System.out.println("第一个线程启动");
                       while (!flag){
       
                       }
                       System.out.println("第一个线程结束");
                   }
               }).start();
       
               TimeUnit.SECONDS.sleep(1);
       
               new Thread(new Runnable() {
                   @Override
                   public void run() {
                       System.out.println("第二个线程启动");
                       flag=true;
                       System.out.println("第二个线程结束");
                   }
               }).start();
           }
       }
       
       //输出
       //第一个线程启动
       //第二个线程启动
       //第二个线程结束
       //第一个线程结束
       ```

18. 