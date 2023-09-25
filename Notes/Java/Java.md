1. 浅克隆/深克隆
   1. 浅克隆
      1. 只复制外壳,内层元素为指向同一对象的地址
      2. 修改取其中一个内层对象数据,另一个也会随着跟着变动
   2. 深克隆
      1. 内层外层全部重新new,相互不影响
2. Comparable/Comparator
   1. 二者均为接口，都只有一个函数，因而都满足函数式编程
   2. Comparable通常为泛型类的默认排序规则，如果加了Comparator则为动态实际排序规则
   3. 比较方法名不同
      1. Comparable<T>
         1. compareTo(T target)
      2. Comparator<T>
         1. compare(T source,T target)