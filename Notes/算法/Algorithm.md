# 算法

1. 算法内功

   1. 算法是基于数据结构的增删改查

   2. 数据结构本质分类
      1. 线形结构（1对1）
         1. 结构分类
            1. 数组

            2. 链表

         2. 特性分类
            1. 栈
            2. 队列

      2. 树形结构（1对n）
         1. 二叉树

         2. 多叉树

      3. 图形结构（m对n）
         1. DFS

         2. BFS

   3. 算法基本构成
      1. 顺序
      2. 循环
      3. 分支
      4. 递归

2. 算法复杂度

   1. 时间复杂度(Time Complexity)
      1. 常数项忽略
      2. 低次项忽略
      3. 系数项忽略
   2. 空间复杂度(Space Complexity)

3. 数据结构的本质

   - 数组
     - 顺序存储
     - 查询快,增删慢
   - 链表
     - 链式存储
     - 查询慢,增删快

4. 算法的本质
   - 以数据结构为基础对数据进行增,删,改,查

5. 经典排序算法

   1. Bubble Sort

      ```java
      /**
       * Bubble Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void bubbleSort(int[] arr){
          for (int i = 0; i < arr.length-1; i++) {
              int temp;
              boolean flag =false;
              for (int j = 0; j < arr.length - 1-i; j++) {
                  if (arr[j]>arr[j+1]){
                      flag=true;
                      swap(arr,j,j+1);
                  }
              }
              debugPrint(arr,i+1);
              if (!flag)break;
          }
      }
      ```

   2. Select Sort

      ```java
      /**
       * Select Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void selectSort(int[] arr){
          for (int i = 0; i < arr.length-1; i++) {
              for (int j = i+1; j < arr.length; j++) {
                  if (arr[i]>arr[j]){
                      swap(arr,i,j);
                  }
              }
          }
      }
      ```

   3. Insert Sort

      ```java
      /**
       * Insert Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void insertSort(int[] arr){
          for (int i = 1 ; i <arr.length ; i++) {
              int current=arr[i];
              int j=i;
              while (current<arr[j-1]&&j>0){
                  arr[j]=arr[j-1];
                  j--;
              }
              if (i!=j)arr[j]=current;
          }
      }
      ```

   4. Shell Sort

      ```java
      /**
       * Shell Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void shellSort(int[] arr){
          debugPrint(arr,0);
          int count=1;
          for (int step = arr.length/2; step >=1 ; step/=2) {
              System.out.println(step);
              for (int i = step; i <arr.length ; i++) {
                  int current=arr[i];
                  int j=i;
                  while (j>=step&&current<arr[j-step]){
                      arr[j]=arr[j-step];
                      j-=step;
                  }
                  if (j!=i)arr[j]=current;
              }
              debugPrint(arr,count++);
          }
      }
      ```

   5. Quick Sort

      ``` java
      /**
       * Quick Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void quickSort(int[] arr,int start,int end){
          if (start>=end)return;
          int left=start;
          int right=end;
          int pointer=arr[left];
          while (left<right){
              while (left<right&&arr[right]>=pointer){
                  right--;
              }
              arr[left]=arr[right];
              while(left<right&&arr[left]<=pointer){
                  left++;
              }
              arr[right]=arr[left];
          }
          arr[left]=pointer;
          quickSort(arr,start,left-1);
          quickSort(arr,left+1,end);
      }
      ```

   6. Merge Sort

      ``` java
      /**
       * Merge Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void mergeSort(int[] arr,int start,int end,int[] memo){
          if (start<end){
              int mid=(start+end)/2;
              mergeSort(arr, start, mid, memo);
              mergeSort(arr,mid+1,end,memo);
              merge(arr,start,mid,end,memo);
          }
      }
      
      public static void merge(int[] arr,int start,int mid,int end,int[] memo){
          int leftStart=start;
          int rightStart=mid+1;
          int memoIndex=0;
          while (leftStart<=mid&&rightStart<=end){
              if (arr[leftStart]<=arr[rightStart]){
                  memo[memoIndex++]=arr[leftStart++];
              }else {
                  memo[memoIndex++]=arr[rightStart++];
              }
          }
          while (leftStart<=mid){
              memo[memoIndex++]=arr[leftStart++];
          }
          while (rightStart<=end){
              memo[memoIndex++]=arr[rightStart++];
          }
      
          memoIndex=0;
          while (start<=end){
              arr[start++]=memo[memoIndex++];
          }
      }
      ```

   7. Radix Sort

      ``` java
      /**
       * Radix Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void radixSort(int[] arr){
          int[][] buckets =new int[10][arr.length];
          int[] bucketIndexs =new int[arr.length];
          Arrays.fill(bucketIndexs,-1);
          int maxNum=arr[0];
          for (int i = 0; i < arr.length; i++) {
              if (arr[i]>maxNum)maxNum=arr[i];
          }
          int maxLength=(maxNum+"").length();
      
          for (int i = 1,n=1; i <= maxLength; i++,n*=10) {
              for (int j = 0; j < arr.length; j++) {
                  int radixNum=arr[j]/n % 10;
                  buckets[radixNum][++bucketIndexs[radixNum]]=arr[j];
              }
      
              int index=0;
              for (int j = 0; j < buckets.length; j++) {
                  for (int k = 0; k <= bucketIndexs[j]; k++) {
                      arr[index++]=buckets[j][k];
                  }
                  bucketIndexs[j]=-1;
              }
          }
      }
      ```

   8. Heap Sort

      ``` java
      /**
       * Heap Sort
       * Time Complexity
       * Space Complexity
       * @param arr sort arr
       */
      public static void heapSort(int[] arr){
          //第一次构建大顶堆,至下而上,从右往左倒序构建大顶堆
          for (int i = arr.length/2-1; i >=0 ; i--) {
              //最后一个非叶子节点开始直到根节点构建大顶堆
              adjustHeap(arr,i,arr.length);
          }
          int temp;
          //循环数组长度-1次将每一次构建成的大顶堆的根节点元素放到相对最后一位
          for (int i = arr.length-1; i >0; i--) {
              temp=arr[i];
              arr[i]=arr[0];
              arr[0]=temp;
              //放完一次根节点到最后之后此时堆中除了根节点以外所有节点均满足大顶堆定义,所以只需要从上到下调整新的大顶堆,即开始节点为0
              adjustHeap(arr,0,i);
          }
      }
      public static void adjustHeap(int[] arr,int nodeIndex,int len){
          int temp =arr[nodeIndex];
          for (int i = 2*nodeIndex+1; i <len ; i=i*2+1) {
              if (i+1<len&&arr[i]<arr[i+1]){
                  i++;
              }
              if (arr[i]>temp){
                  arr[nodeIndex]=arr[i];
                  nodeIndex=i;
              }else {
                  break;
              }
          }
          arr[nodeIndex]= temp;
      }
      ```

6. 查找算法

   1. 线性查找
   2. 二分查找
   3. 插值查找
   4. 斐波那契查找

7. 树

   1. 基本用语

      1. 节点
      2. 根节点
      3. 父节点
      4. 子节点
      5. 叶子节点(没有子节点的节点)
      6. 节点的权(节点值)
      7. 路径(从root 节点找到该节点的路线)
      8. 层
      9. 子树
      10. 树的高度(最大层数)
      11. 森林:多颗子树构成森林

   2. 二叉树

      1. 二叉树
         1. 每个节点最多只有两个子节点的树称为二叉树
            1. 性质
               1. 二叉树的第i层最多只有2的n-1个结点
      2. 满二叉树
         1. 所有叶子结点都在最后一层
            1. 性质
               1. 总节点数$=2^n-1$
      3. 完全二叉树
         1. 从上到下,从左到右节点排列不中断
            1. 性质
               1. 具有n个结点的完全全二叉树的数的高=[log2(n)]+1
               2. 具有n个结点的完全全二叉树,第i个结点编号设为j,则如果,左右子结点都存在的话满足父结点=[i/2],左结点=2i(2i<n),右节点=2i+1(2i+1<n)
      4. 二叉树的遍历
         1. 前序遍历(父→左→右)
         2. 中序遍历(左→父→右)
         3. 后序遍历(左→右→父)
      5. 常见操作
         1. 查找节点
         2. 删除节点

   3. 顺序存储二叉树(只考虑完全二叉树)

      ​	定义:按照数组结构存储,按照二叉树的结构遍历

      1. 特点
         1. 第n个节点的左子节点$=2n+1$
         2. 第n个节点的右子节点$=2n+2$
         3. 第n个节点的父节点$=\frac{n-1}{2}$
         4. n为数组索引下标

   4. 线索化二叉树

      1. n个节点的二叉树中,会有n+1个空指针域,若将某种遍历顺序下该节点的前驱和后继节点存放在空指针域中,则实现线索化二叉树,空指针域加入的前驱或者后继称之为线索

   5. B树

      1. B树是一种多叉树
      2. B树的值存在叶子节点和非叶子节点
      3. B树的子节点都在最后一层

   6. B+树

      1. B+树的数据均存放在叶子节点
      2. 聚簇索引
      3. 叶子节点之间有兄弟指针

   7. B*树

      1. 在B+树的基础上增加非叶子节点之间的兄弟指针

8. 图

   1. 基本概念

      1. 顶点：vertex

      2. 边：edge

      3. 路径：点到点对应边的连线

      4. 有向(AB≠BA)/无向(AB=BA)

      5. 权值：边的长度

   2. 表示方法

      1. 邻接矩阵

         ![](I:\Jpanda\MD\MD\Notes\算法\images\image-20220919230136033.png)

      2. 邻接表

         ![](I:\Jpanda\MD\MD\Notes\算法\images\image-20220919230512646.png)

   3. 图的遍历

      1. DFS(深度优先)
      2. BFS(广度优先)

   4. 图的常用算法

      1. 普利姆算法(最小生成树)
      2. 克鲁斯卡尔算法
      3. 迪杰斯特拉算法
      4. 弗洛伊德算法

9. 算法模板

   1. 回溯模板

   ```java
   // 枚举所有
   void backTrace(int[] nums,List<Integer> paths){
       // 结束条件
       if (paths.size()== nums.length){
           rets.add(new ArrayList<>(paths));
           return;
       }
       for (int i = 0; i < nums.length; i++) {
           // 剪支
           if (paths.contains(nums[i])){
               continue;
           }
           // 选择路径
           paths.add(nums[i]);
           // DFS
           backTrace(nums,paths);
           // 回溯
           paths.remove(paths.size()-1);
       }
   }
   
   // 枚举第一个满足条件的
   boolean backTrace(int[] nums,List<Integer> paths){
       // 结束条件
       if (paths.size()== nums.length){
           rets.add(new ArrayList<>(paths));
           return true;
       }
       for (int i = 0; i < nums.length; i++) {
           // 剪支
           if (paths.contains(nums[i])){
               continue;
           }
           // 选择路径
           paths.add(nums[i]);
           // DFS
           if(backTrace(nums,paths)){
               return true;
           };
           // 回溯
           paths.remove(paths.size()-1);
       }
       return false;
   }
   ```

   - 典型题型

     - 子集

       - 结束条件：没有
       - 路径列表：index控制选择范围

     - 排列

       - 结束条件：选择列表选完
       - 剪支：保证没有用过
       - 路径列表：全体

     - 组合

       - 结束条件：选择了k个元素时
       - 路径列表：index控制选择范围

     - ```
       1.组合问题中如果是排列问题则应该是用contains保证不重用，不使用index定位，即AB!=BA
       2.如果是组合问题则应该使用定向index来保证顺序，即AB=BA
       3.子集问题本质相当于所有组合问题的全集，也就是k从0-n的全部合集
       ```

     - N皇后

       - 结束条件：第n层皇后摆放结束
       - 剪支：保证该位置是否可以放皇后(左上，上，右上均不能有棋子)
       - 路径列表：棋盘的每一行的1-n列可以任意选

     - 数独

       - 结束条件：第9层数字摆放结束
       - 剪支：保证该位置是否可以放该数字(横，竖，九宫格内不能包含相同数字)
       - 路径列表：1-9

     - 马踏棋盘

       - 结束条件：跳了N*N次
       - 剪枝：尽可能选择下一步对应的走法数量最少的分枝
       - 路径列表：马走日字对应的八个方向

     - ```
       1.N皇后和数独问题本质均为暴力枚举，因而，如果只想获取一组答案，则一般需要将回溯递归体返回值定义为boolean类型如若想枚举所有可能，则使用void返回类型不加限制
       2.本质原理使用boolean后，相当于dfs中找到一个树枝以后不断向上返回，最终结束，若不返回，则会回溯整棵树
       ```
   2. 滑动窗口模板

      1. 模板打油诗

         ```
         滑动窗口
         
         左右指针从零跑，目标窗口要定好。
         外环右针向右进，窗口直到大小尽。
         内环左针随后追，移出窗口最小堆。
         中间变量适当加，最后求出指针差。
         ```

      2. ```java
         int[] nums;
         int left=0;
         int right=0;
         target
         window
         while(right<nums.length){
             rightValue = nums[right];
             window.put(rightValue);
             right++;
             while(left<right && window要收缩){
         	    leftValue = nums[left];
             	window.remove(leftValue);
             	left++;
             }
         }
         ```

         

10. 算法技巧

    1. 数组

       1. 如何知道一个无序数组中能找到的最大连续的序列包含的元素个数

          1. 如果没有时间复杂度要求则可以先排序，转为下一个问题

          2. 如果元素个数不是特别巨大，则可以使用Set集合将其去重，然后依次遍历该set集合中的每个元素，找到每一个序列左端点，然后while步进1步的方式确定该序列的最大长度，满足左端点的元素e一定满足条件e-1不在当前set集合中

          3. 核心代码逻辑

             ```java
             //所求答案
             int ret =1;
             for(Integer e : set){
                  //存在比e还靠左的端点，直接下一位
             	if(set.contains(e-1)){
             		continue;
             	}
             	//当前子序列的最大个数
             	int count =1;
                 //步进循环求得最大连续个数
             	while(set.contains(e+1)){
             		count++;
             		e++;
             	}
                 ret = Math.max(ret,count);
             }
             ```

       2. 如何知道一个有序数组中能找到的最大连续的序列包含的元素个数

          1. 有序的状态下，我们只需要使用连续两位数字进行递进比较，类似于冒泡排序的内层循环nums[i]和nums[i-1]进行比较

          2. 如果相等，则跳过进行下一位比较，如果nums[i]=nums[i-1]+1则有效长度+1，否则重置计数器，进入下一位比较

          3. 核心代码逻辑

             ```java
             int ret =1;
             
             int count =1;
             for(int i=1;i<nums.length;i++){
                 //如果相等，则跳过进行下一位比较
                 if(nums[i]==nums[i-1]){
                     continue;
                 }else if(nums[i]==nums[i-1]+1){
                     //有效长度+1
                     count++;
                     ret = Math.max(ret,count);
                 }else{
                     //重置计数器
                     count =1;
                 }
             }
             
             //升华
             
             /**
             *此题使用的基本模型可以定义为nums[i]和nums[i-1]的连续等差比较模型，即判断连续两个元素的关系
             *如：
             *冒泡排序模型比较两个元素大小关系
             *本例比较两个元素的等量关系
             */
             ```

             

    2. 双指针

       1. 快慢指针

          1. 将数组中的所有某个元素A移动到数组最后，其他元素相对位置不变

             1. 相当于从所有元素中顺序找到所有不是A的元素，并用一个slow指针随时指向当前能够放入的位置，再用一个快指针fast遍历整个数组

             2. 核心逻辑代码
    
                ```java
                //注意slow指针指向下一个可以存放非A元素的索引，fast指针负责遍历整个数组
                int slow = 0;
                for(int fast =0;fast <nums.length;fast++){
                    //寻找下一个非A元素
                    if(nums[fast]==A){
                        continue;
                    }else{
                        //找到非A元素，即和slow交换元素位置
                        swap(nums,slow,fast);
                        //slow指向下一个可以存放非A元素的索引
                        slow++;
                    }
                }
                
                
                //升华
                //如果把所有A元素移到最后呢，其他条件不变，则只需要快慢指针从nums.length-1倒序处理即可	
                ```
             
    
       2. 前后夹逼指针
    
          1. 三数之和
    
    3. 链表
    
       1. 反转一个链表
    
          1. 反转链表的模型本质可以理解为将A→B→C转化为C→B→A，因为任何一个链表我们可以抽象结构为，前驱→当前结点→后驱
    
          2. 然后使用基本的链表增删模型操作即可
    
          3. 核心逻辑代码
    
             ```java
             //初始状态理解为A为null，B为根节点root，C为根节点后驱
             Node head=null;
             while(root!=null){
                 //当前节点后驱临时存起来
                 Node tail = root.next;
                 //更新当前结点后驱为前驱结点
                 root.next = head;
                 //更新新的前驱结点
                 head = root;
                 //更新新的根节点
                 root =next;
             }
             
             
             //升华
             /**
             *链表增删查改基本操作
             *1.A，B之间增加结点C，先将B临时当作tempB保存起来，然后A.next指向C，C.next指向tempB
             *2.A，C之间删除结点B，先将C临时当作tempC保存起来，然后A.next指向tempC
             *3.获取A，B结点中的A结点，并去掉其next结点，先将B结点保存起来，然后让A.next指向null
             *4.修改A结点的值，遍历找到A结点然后A.val=新值
             */
             ```
    
             
    
    4. 栈
    
    5. 二叉树
    
    6. 图
    
    7. 动态规划