1. 基本概念

   1. 进程:一个程序,一个车间
   2. 线程:一条生产线
   3. 并发:交替进行
   4. 并行:平行进行

2. 多线程状态(男人不玩舔玩偷)

   1. NEW
   2. RUNNABLE
   3. BLOCKED
   4. WAITING
   5. TIMED_WAITING
   6. TERMINATED

3. 并发三大特性(并发原子有序是显而易见的)

   1. 可见性
   2. 有序性
   3. 原子性

4. 内存屏障:内存屏障前后代码不重排序

   1. LOADLOAD
   2. STORESTORE
   3. LOADSTORE
   4. STORELOAD

5. 缓存一致性MESI(没事)

   1. M修改（Modefiy）:该缓存行有效， 数据被修改了，和内存中的数据不一样，数据只存在于本缓存行中
   2. E独享（Exclusive）:该缓存行有效，数据和内存中的数据一致，数据只存在本缓存行中
   3. S共享（Shared）:该缓存行有效，数据和内存中的数据一致，数据同时存在于其他缓存中
   4. I无效（Invalid）:该缓存行数据无效

6. JMM(Java内存模型)

7. JMM指令(独裁用赋存些黄金锁起来)

   1. read:从主内存读取数据
   2. load:加载读取数据到工作内存
   3. use:用于计算
   4. assign:重新赋值结果到工作内存
   5. store:将工作内存的变量值存入主内存
   6. write:将新值写入主内存
   7. lock:加锁主内存变量,变成线程独占
   8. unlock:解锁

8.  Volatile(非洲大草原随地可见禁止排便)

   1. 可见性

   2. (有序性)禁止指令重排

   3. 不保证原子性

   4. ``` java
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

   5. 