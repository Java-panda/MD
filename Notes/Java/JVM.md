1. 引用类型

   1. 强引用

      1. ```java
         Object p1 = new Object();
         System.out.println(p1);//不回收
         System.gc();
         System.out.println(p1);//不回收
         ```

   2. 软引用

      1. ```java
         Object p2 = new Object();
         SoftReference<Object> personSoftReference = new SoftReference<>(p2);
         System.out.println(personSoftReference.get());//不回收
         p2=null;
         System.gc();//,内存够用则不回收,内存不够则回收
         System.out.println(personSoftReference.get());
         ```

   3. 弱引用

      1. ```java
         Object p3 = new Object();
         WeakReference<Object> personWeakReference = new WeakReference<>(p3);
         System.out.println(personWeakReference.get());//不回收
         p3=null;
         System.gc();
         System.out.println(personWeakReference.get());//回收
         ```

   4. 虚引用

      1. ```java
         Object p4 = new Object();
         ReferenceQueue<Object> queue = new ReferenceQueue<>();
         PhantomReference<Object> phantomReference = new PhantomReference<>(p4, queue);
         System.out.println(phantomReference.get());//回收
         p4=null;
         System.gc();
         System.out.println(phantomReference.get());//回收
         ```

2. 内存模型(本地虚拟计算一堆方法)

   1. 方法区 
   2. 堆
      1. 新生代:1
         1. Eden:8
         2. S1(From):1
         3. S2(To):1
      2. 老年代:2
   3. 本地方法栈
   4. 虚拟机栈
      1. 栈帧
         1. 局部变量表
         2. 操作数栈
         3. 方法返回地址
         4. 动态链接
   5. 程序计数器
      1. 记录程序执行地址
   6. 总结
      1. 共享区:堆和方法区
      2. OutOfMemoryError
         1. 线程太多
      3. StackOverflowError
         1. 栈帧过多,递归过深

3. 类加载器
   1. 步骤
      1. 加载
      2. 链接
         1. 验证
         2. 准备
         3. 解析
      3. 初始化
   2. 分类
      1. BootStrapClassLoader
         1. 加载jre/lib
      2. ExtClassLoader
         1. 加载jre/lib/ext
      3. AppClassLoader
         1. 加载自己APP中的类路径下targets
      4. WedbAppClassLoader(Tomcat)
         1. 
   3. 双亲委派
      1. BootStrapClassLoader
      2. ExtClassLoader
      3. AppClassLoader
         1. AppClassLoader加载类时先让ExtClassLoader加载
         2. ExtClassLoader加载类时先让BootStrapClassLoader加载
         3. BootStrapClassLoader加载不到时ExtClassLoader加载
         4. ExtClassLoader加载不到时AppClassLoader加载
         5. 避免重复加载类或者被恶意窜改
      4. Tomcat:

4. GC
   1. 分类
      1. Young GC/Minor GC:新生代回收
      2. Old GC/Major GC:老年代回收
      3. Full GC:整堆回收
   2. 垃圾标记
      1. 引用计数法
         1. 计数被引用次数
         2. 优点:实现简单
         3. 缺点:需要额外空间,时间,无法解决循环引用
      2. 可达性分析
         1. 使用GC Roots进行溯源分析.有那些对象正在被GC Root引用
         2. GC Roots
            1. 正在执行的栈中的局部变量
            2. 方法区中的静态类变量
            3. 方法区中的常量
   3. 具体实现
      1. 标记清除
         1. 判断对象可达后,记录下来
         2. 遍历堆空间,没有记录的对象清除
         3. 优点:实现简单
         4. 缺点:效率不高,内存碎片
      2. 复制算法
         1. 内存分成两块A,B
         2. GC一次后把可达对象复制到某一半,删除另一块全部对象
         3. 优点:减少内存碎片
         4. 缺点:当单次垃圾对象所占比例较小时,需要大量复制,降低效率
      3. 标记整理
         1. 标记可达对象
         2. 整理可达对象到一端连续内存
         3. 清除剩余空间
         4. 相当于标记清除+复制算法的结合
         5. 优点:降低内存碎片
         6. 缺点:效率不高
      4. 

5. JVM调优(参数设置)
   1. -Xms:堆的最小内存大小(1/64M)
   2. -Xmx:堆的最大内存大小(1/4M)