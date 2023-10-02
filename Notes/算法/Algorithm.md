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
         外环右针向右进，直到窗口大小尽。
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
   3. 动态规划模板

      1. 动态规划的本质就是已知数列首项a1以及an和an-1的递推公式，求an的过程，而如何设计一个合理的an便是设计合理状态的过程，而求解动态规划的本质就是暴力迭代求数列an值的过程
      2. 动态规划的三大要素
         1. 状态：an的含义
         2. 状态转移方程：an和an-1的递推公式
         3. 初始值：a0

10. 算法技巧

    1. 二分查找

       1. [搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

          1. 方法

             1. 暴力法：首先否定暴力方法不可取，题中给出条件有序，那么一定考虑二分查找

             2. 二维数组一维数组化后二分查找

                1. 因为行有序，列有序，则矩阵全有序，将mn的矩阵看成一个长度为mn的一维数组，然后使用最朴素的二分查找模型

                2. 一维数组nums[i]在矩阵中的对应位置

                   1. row:i / matrix[0].length

                   2. col:i % matrix[0].length

                3. 核心逻辑代码

                   ```java
                   int left=0;
                   int right=matrix.length*matrix[0].length-1;
                   while(left<=right){
                       int mid =(left+right)>>1;
                       int cur =matrix[mid / matrix[0].length][mid % matrix[0].length];
                       if(cur==target){
                           return true;
                       }else if(cur>target){
                           right=--mid;
                       }else{
                           left=++mid;
                       }
                   }
                   
                   return false;
                   ```

             3. 行列两次二分查找

                1. 先二分查找找到第一列中小于等于目标值target的最大值对应的行索引

                2. 然后二分查找对应整行的数据

                3. 核心逻辑代码

                   ```java
                   int targetRow =getLowIndex(matrix,target);
                   //如果出现矩阵第一个都比目标值大的情况，后续无需继续查找
                   if(targetRow==-1){
                       return false;
                   }
                   int left=0;
                   int right=matrix[0].length-1;
                   while(left<=right){
                       int mid =(left+right)>>1;
                       if(matrix[targetRow][mid]==target){
                           return true;
                       }else if(matrix[targetRow][mid]>target){
                           right=--mid;
                       }else{
                           left=++mid;
                       }
                   }
                   return false;
                   
                   
                   //这个方法需要当作模板记住！！！
                   public int getLowIndex(int[][] matrix, int target) {
                       int left=0;
                       int right=matrix.length-1;
                       int ret =-1;
                       while(left<=right){
                           int mid =(left+right)>>1;
                           if(matrix[mid][0]==target){
                               return mid;
                           }else if(matrix[mid][0]>target){
                               //如果想找到大于等于目标值的第一个值请在这里
                               //ret=mid;
                               right=--mid;
                           }else{
                               //核心逻辑在这里
                               ret=mid;
                               left=++mid;
                           }
                       }
                   
                       return ret;
                   }
                   
                   ```

                   

       2. [搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

          1. 方法

             1. 每行二分查找效率较低
             2. 逐行排除法，选择左下角->右上角的元素逐一对比(右上角->左下角方法类似)
                1. 如果matrix(row,col)<target:col++
                2. 如果matrix(row,col)>target:row--

          2. 核心逻辑代码

             ```java
             //当matrix[row][col]>target则matrix[row][col]的第row行全不符合排除
             //当matrix[row][col]<target则matrix[row][col]的第col列全不符合排除
             int row =matrix.length-1;
             int col =0;
             while(row>=0 && col<matrix[0].length){
                 if(matrix[row][col]==target){
                     return true;
                 }else if (matrix[row][col]>target){
                     row--;
                 }else{
                     col++;
                 }
             }
             return false;
             ```

             

    2. 数组

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

       3. 轮转

          1. [轮转数组](https://leetcode.cn/problems/rotate-array/)

          2. 方法

             1. 暴力法
                1. 显然暴力法可以每次移动一位，移动k次时间复杂度T(n)

             2. 数组单个元素旋转
                1. 考虑使用临时数组将后面k个元素存起来，然后前面len-k个元素从后往前分别移动距离k，然后将临时数组复制回即可

             3. 轮转（摩根定律AB=-（-A并-B））

          3. 核心逻辑代码

             ```java
             //方法二.整体后移k个位置，但是需要额外k的空间
             int len =nums.length;
             k=k % len;
             if(k==0){
                 return;
             }
             int[] temp =new int[k];
             //先将后面k个元素临时存起来
             for(int i=len-k;i<len;i++){
                 temp[i+k-len]=nums[i];
             }
             //将前面len-k个元素，从后往前分别移动k距离
             for(int i=len-k-1;i>=0;i--){
                 nums[i+k]=nums[i];
             }
             
             //最后将临时存起来的temp复制到nums的最前面即可
             for(int i=0;i<temp.length;i++){
                 nums[i]=temp[i];
             }
             
             //方法三
             int len =nums.length;
             k=k % len;
             if(k==0){
                 return;
             }
             //反转前len-k个元素
             reverse(nums,0,len-k-1);
             //反转后k个元素
             reverse(nums,len-k,len-1);
             //反转len个元素
             reverse(nums,0,len-1);
             ```

             

       4. 合并

          1. 以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

          2. 考虑将原数组按照start升序排序

          3. 数组两两比较记为ab和cd，已知排完序后c>=a

          4. 分类讨论

             1. 如果d<=b那么合并后结果为ab
             2. 如果c<=b同时d>b那么合并后为ad
             3. 如果c>b那么合并后ab，cd不变

          5. 由上面的分析可以知道，4.1和4.2中会进行合并，合并后的结果和下一个再次进行比较合并，因此不确定是否可以加入结果集，但是4.3中的ab一定不会再次发生合并，因而可以加入结果集，所以得出结论，只有在4.3的情况下像结果中加入ab，直到所有循环结束，此时最后一个cd需要加入结果集

          6. 核心逻辑代码

             ```java
             //排序
             Arrays.sort(intervals,(a,b)->{
                 return a[0]!=b[0]?a[0]-b[0]:a[1]-b[1];
             });
             List<int[]> list = new ArrayList<>();
             int i=0;
             for(i=0;i<intervals.length-1;i++){
                 //如果d<=b那么合并后结果为ab
                 if(intervals[i+1][1]<=intervals[i][1]){
                     //每次将下一次的比较对象移到i+1的位置
                     intervals[i+1][0]=intervals[i][0];
                     intervals[i+1][1]=intervals[i][1];
                 }else if(intervals[i+1][1]>intervals[i][1] && intervals[i+1][0] <=intervals[i][1]){
                     //如果c<=b同时d>b那么合并后为ad
                     intervals[i+1][0]=intervals[i][0];
                 }else{//如果c>b那么合并后ab，cd不变
                     list.add(intervals[i]);
                 }
             }
             //最后一个cd需要加入结果集
             list.add(intervals[i]);
             return list.toArray(int[][]::new);
             
             
             
             //可以借鉴一下使用Stack的写法
             //方法二.栈
             Arrays.sort(intervals,(a,b)->{
                 return a[0]!=b[0]?a[0]-b[0]:a[1]-b[1];
             });
             Stack<int[]> stack = new Stack<>();
             stack.push(intervals[0]);
             
             for(int i=1;i<intervals.length;i++){
                 //a=pop[0] b=pop[1]
                 int[] pop =stack.pop();
             
                 //c=intervals[i][0] d=intervals[i][1]
                 //d<=b
                 if(intervals[i][1]<=pop[1]){
                     stack.push(pop);
                 }else if(intervals[i][1]>pop[1] && intervals[i][0] <=pop[1]){//c<=b && d>b
                     pop[1]=intervals[i][1];
                     stack.push(pop);
                 }else{//c>b
                     stack.push(pop);
                     stack.push(intervals[i]);
                 }
             }
             return stack.stream().toArray(int[][]::new);
             ```

             

    3. 双指针

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

          1. 两数之和/三数之和

             1. 如果不需要返回索引下标则可以直接排序后，前后夹逼双指针

             2. 核心逻辑代码

                ```java
                int target;
                int[] nums;
                int second = nums.length-1;
                for(int first=0;first<second;first++){
                    //避免重复结果集
                    if(first!=0 && nums[first]==nums[first-1]){
                        continue;
                    }
                    while(first<second && nums[first]+nums[second]>target){
                        second--;
                    }
                    if(first<second && nums[first]+nums[second]==target){
                        //加入结果集
                    }
                }
                
                
                //升华
                //这是夹逼双指针的基础模板，务必熟练写出
                ```
          2. 盛最多水的容器

             1. 本质在于求数组中任意两个元素和对应值作为高之间围成的区域能盛放的最大水量

             2. 最大水量=(right-left)**Min*(nums[left],nums[right])

             3. 判断左右指针递进的逻辑为率先移动值较小的一方，不然的话，假若先移动较大值一方，那么接下来无论如何移动都不会使得新的面积大于当前面积，因为该策略一定是错的，那么反之则一定是对的

             4. 核心逻辑代码

                ```java
                int[] nums
                int maxArea=0;
                //定义左右指针
                int left=0;
                int right=nums.length-1;
                while(left<right){
                    //面积(盛水量)计算公式
                	int area = (right-left)*Math.Min(nums[left],nums[right]);
                	maxArea =Math.max(maxArea,area);
                	//指针递进逻辑，更新较小者的指针递进
                	if(nums[left]<nums[right]){
                		left++;
                	}else{
                		right--;
                	}
                }
                ```

                

       3. 滑动窗口

          1. 找到字符串中所有字母异位词

             1. 直接套前文打油诗模板

             2. 核心逻辑代码

                ```java
                List<Integer> rets = new ArrayList<>();
                 //左右指针从零跑
                 int left =0;
                 int right =0;
                 //目标窗口要定好
                 Map<Character,Integer> target = new HashMap<>();
                 Map<Character,Integer> window = new HashMap<>();
                 for(int i=0;i<p.length();i++){
                     target.put(p.charAt(i),target.getOrDefault(p.charAt(i),0)+1);
                 }
                
                 int coverSize =0;
                 //外环右针向前进
                 while(right<s.length()){
                     char rightValue = s.charAt(right);
                     window.put(rightValue,window.getOrDefault(rightValue,0)+1);
                     right++;
                     if(window.get(rightValue).equals(target.get(rightValue))){
                         coverSize++;
                     }
                     //直到窗口大小尽
                     while(left<right && window.get(rightValue)>target.getOrDefault(rightValue,0)){
                         //内环左针随后追
                         char leftValue = s.charAt(left);
                         if(window.get(leftValue).equals(target.get(leftValue))){
                             coverSize--;
                         }
                         //移除窗口最小堆
                         window.put(leftValue,window.getOrDefault(leftValue,0)-1);
                         left++;
                     }
                
                     //中间变量适当加
                     if(coverSize == target.size()){
                         //最终求出指针差
                         rets.add(left);
                     }
                 }
                 return rets;
                
                //这是滑动窗口的基础模板，应当熟练掌握
                ```

                

    4. 链表

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
                 root =root.next;
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

             

       2. 正序复制或者逆序复制一个链表并保持原链表结构不变

          1. 正序复制

             ```java
             public static ListNode copy(ListNode root) {
                 //虚拟头节点，用于得到新的头节点
                 ListNode dummy=new ListNode(0);
                 //尾结点
                 ListNode tail =dummy;
             
                 while(root!=null){
                     //复制一个新的同值结点
                     ListNode node =new ListNode(root.val);
                     //把新节点加到尾部的next
                     tail.next=node;
                     //更新尾结点
                     tail=tail.next;
                     //下一个节点
                     root=root.next;
                 }
                 //返回虚拟结点的下一个节点
                 return dummy.next;
             }
             ```

             

          2. 逆序复制

             ```java
             public static ListNode reverseCopy(ListNode root) {
                 //新链表
                 ListNode tail=null;
                 
                 while(root!=null){
                     //复制一个新的同值结点
                     ListNode node =new ListNode(root.val);
                     //新节点的next更新为之前的新链表
                     node.next=tail;
                     //更新新链表
                     tail=node;
                     //下一个
                     root=root.next;
                 }
                 return tail;
             }
             ```

             

       3. Floyd判圈算法：判断一个链表是否有环，环的起始结点，环的长度，起点到环起点的距离

          1. 基本原理

             1. 设置快慢指针slow和fast同时从原点开始，slow指针每次走一步，fast每次走2步，当二者第一次相遇后，将fast重新放回原点，则下一次相遇时就是相遇的结点
             2. ![image-20231001140324352](C:\Users\liuji\AppData\Roaming\Typora\typora-user-images\image-20231001143204414.png)

          2. 判断是否有环

             ```java
             public boolean haveLoop(ListNode head){
                 ListNode slow =head;
                 ListNode fast =head;
                 while(fast!=null && fast.next!=null){
                     slow=slow.next;
                     fast=fast.next.next;
                     if(slow==fast){
                         return true;
                     }
                 }
                 return false;
             }
             ```

          3. 环的起点

             ```java
             public ListNode startNode(ListNode head){
                 ListNode slow =head;
                 ListNode fast =head;
                 //记录是否有环
                 boolean haveLoop =false;
                 while(fast!=null && fast.next!=null){
                     slow=slow.next;
                     fast=fast.next.next;
                     if(slow==fast){
                         //遇到环直接重置fast
                         fast=head;
                         haveLoop =true;
                         break;
                     }
                 }
                 if(!haveLoop){
                     return null;
                 }
                 while(fast!=slow){
                     slow=slow.next;
                     fast=fast.next;
                 }
                 return fast;
             }
             ```

          4. 环的长度

             ```java
             public int lenToLoopNode(ListNode head){
             	//同3中的逻辑但是相遇后fast不重置原点而是继续跑一圈计算长度
                 //...
                 //用一个变量记录长度
                 int len =1;
                 while((fast=fast.next)!=slow){
                     len++;
                 }
                 return len;
             }
             ```

          5. 原点到环入口的距离

             ```java
             public int lenToLoopNode(ListNode head){
             	//同3中的逻辑
                 //...
                 //重置fast到原点后，用一个变量记录长度
                 int len =1;
                 while(fast!=slow){
                     len++;
                     slow=slow.next;
                     fast=fast.next;
                 }
                 return len;
             }
             ```

       4. [合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

          1. 参考数组的归并合并，写法和原理类似

          2. 核心逻辑代码

             ```java
             public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
                 ListNode dummy =new ListNode(0);
                 ListNode tail =dummy;
                 while(list1!=null && list2!=null){
                     if(list1.val<list2.val){
                         ListNode next=list1.next;
                         list1.next=null;
                         //上面两行完全可以省略，因为下面的单个链表最后整体追加的时候
                         //会覆盖掉原本tail后面多余的链表
                         tail.next=list1;
                         tail=tail.next;
                         list1=next;
                     }else{
                         ListNode next=list2.next;
                         list2.next=null;
                         //上面两行完全可以省略，因为下面的单个链表最后整体追加的时候
                         //会覆盖掉原本tail后面多余的链表
                         tail.next=list2;
                         tail=tail.next;
                         list2=next;
                     }
                 }
                 //链表合并完不需要一个一个合并了，直接剩下部分整体追加即可
                 //同时下面两个if一定会有一个被执行，所以会覆盖掉原本tail后面多余的链表(直呼巧妙)
                 if(list1!=null){
                     tail.next=list1;
                 }
                 if(list2!=null){
                     tail.next=list2;
                 }
             
                 return dummy.next;
             }
             
             //升华
             //此题可以巧妙地构造递归函数，子路如下
             public ListNode dfs(ListNode node1,ListNode node2){
                 if(node1==null){
                     return node2;
                 }else if(node2==null){
              		return node1;       
                 }else if(node1.val<node2.val){
                     node1.next=dfs(node1.next,node2);
                 }else{
                     node2.next=dfs(node1,node2.next);
                 }
             }
             ```

       5. [两数相加](https://leetcode.cn/problems/add-two-numbers/)

          1. 这题大模型为上一题的链表的归并

          2. 题目中已知条件为逆序，这是一个简化条件，因为我们计算的时候一定是从个位开始，那么正好可以两链表互相加

          3. 注意点

             1. 需要合理的设定出进位carry和mod余数变量
             2. 其次考虑某一个链表没有更多元素了（归并模型）
             3. 最后需要注意最后carry是否不为0，这样会多进一位

          4. 核心逻辑代码

             ```java
             public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
                 int carry =0;
                 int mod =0;
                 //链表常用虚拟头结点技巧
                 ListNode dummy = new ListNode(0);
                 ListNode tail = dummy;
                 //归并模型
                 while(l1!=null && l2!=null){
                     mod=(l1.val+l2.val+carry) % 10;
                     carry =(l1.val+l2.val+carry) / 10;
                     ListNode node = new ListNode(mod);
                     tail.next=node;
                     tail=tail.next;
                     l1=l1.next;
                     l2=l2.next;
                 }
                 //不同于链表合并，这里要考虑999这种连续进位的情况，因而需要遍历每一位
                 while(l1!=null){
                     mod=(l1.val+carry) % 10;
                     carry =(l1.val+carry) / 10;
                     ListNode node = new ListNode(mod);
                     tail.next=node;
                     tail=tail.next;
                     l1=l1.next;
                 }
                 //不同于链表合并，这里要考虑999这种连续进位的情况，因而需要遍历每一位
                 while(l2!=null){
                     mod=(l2.val+carry) % 10;
                     carry =(l2.val+carry) / 10;
                     ListNode node = new ListNode(mod);
                     tail.next=node;
                     tail=tail.next;
                     l2=l2.next;
                 }
             	
                 //最后两个链表都相加结束后可能会发生进位，需要额外添加一个结点
                 if(carry!=0){
                     ListNode node = new ListNode(carry);
                     tail.next=node;
                 }
                 return dummy.next;
             }
             ```

       6. [删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

          1. 暴力法

             1. 先遍历得到链表长度len，删除倒数第n个就是删除正数第len+1-n，也就是找到第len-n个结点node
             2. node.next=node.next.next

          2. 递归法

             1. 题目需要删除倒数，那么显然使用后序遍历可以拿到对应的第n个结点序号
             2. node.next=node.next.next
             3. 递归函数每一次返回倒数第几个的数值

          3. 核心逻辑代码

             ```java
             public ListNode removeNthFromEnd(ListNode head, int n) {
                 //删除倒数第N个意味着删除正数第len+1-N个结点
                 int len =0;
                 ListNode hd =head;
                 //求len
                 while(hd!=null){
                     len++;
                     hd=hd.next;
                 }
             
                 n=len+1-n;
                 len=0;
                 hd =head;
                 while(hd!=null && len <n){
                     len++;
                     //如果删的是第一个则直接返回。可以添加虚拟头节点，避免，具体尝试
                     if(n==1){
                         return hd.next;
                     }
                     //找到第len-n个结点
                     if(len==n-1){
                         hd.next=hd.next.next;
                         break;
                     }
                     hd=hd.next;
                 }
                 return head;
             }
             
             
             
             //递归法
             public ListNode removeNthFromEnd(ListNode head, int n) {
                //一般判断方式
                 
                //if(afterTrverse(head,n)==n){
                //    return head.next;
                //}
                //return head;
                 
                //添加虚拟头节点方式 
                ListNode dummy = new ListNode(0);
                dummy.next=head;
                afterTrverse(dummy,n) 
                return dummy.next; 
             }
             public int afterTrverse(ListNode head, int out) {
                 if(head==null){
                     return 0;
                 }
                 int num = afterTrverse(head.next,out);
             
                 if(out==num){
                     head.next=head.next.next;
                 }
                 return num+1;
             }
             
             
             //升华
             //本题常规法比较容易想到，但是具体细节需要注意如果删除倒数最后一个的时候的细节操作
             //对于删除倒数最后一个的时候的细节操作添加虚拟节点有着不错的效果，需要思考和掌握
             ```

             

    5. 队列

       1. 优先队列

          1. 滑动窗口最大值：给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

          2. 首先想到定长为k的滑动窗口，每次循环比较k个元素的最大值，这样时间复杂度nk，尝试后超时

          3. 借助优先队列的大顶堆排序特性，逐个加入nums中的元素，最大值一定会在堆顶

          4. 使用{nums[i],i}作为整体放入堆中，当加入第k+1个元素时，如果对顶是滑动窗口左边界以外的元素，则不断出堆

          5. 核心逻辑代码

             ```java
             //注意，新的数组大小应当nums.length-k+1，可以找下规律简单推理
             int[] rets = new int[nums.length-k+1];
             //定义按值大顶堆排序，如果值相等则按照索引靠后的优先，这也可以避免频繁的移除左边界以外的最大值
             PriorityQueue<int[]> priorityQueue = new PriorityQueue<>((a,b)->{
                 if(a[0]!=b[0]){
                     return b[0]-a[0];
                 }else{
                     return b[1]-a[1];
                 }
             });
             //前k-1个元素无需任何操作直接加入队列中
             for (int i = 0; i < k-1; i++) {
                 priorityQueue.offer(new int[]{nums[i],i});
             }
             //第k个元素开始，每加一个元素均要判断当前堆顶最大值是否超出左边界，超出则出堆
             for (int i = k-1; i < nums.length ; i++) {
                 //第一个存值，第二个存索引
                 priorityQueue.offer(new int[]{nums[i],i});
                 //判断是否越过左边界，只需要算出左边界x，x满足i-x+1=k推出x=i+1-k
                 while(priorityQueue.peek()[1]<(i+1-k)){
                     //出堆
                     priorityQueue.poll();
                 }
                 //逐个加入最大值到结果集，注意索引表达式应该同上为左边界x
                 rets[i+1-k]=priorityQueue.peek()[0];
             }
             return rets;
             
             
             //升华
             //熟悉PriorityQueue构造函数中的Comparator自定义比较器写法
             //顺便学习掌握PriorityQueue的基本用法offer,peek,pop,poll
             //此题索引边界值较多，做题时适当假设值同时熟练掌握等差项数=(ai-ak)/d+1计算公式
             ```

             

    6. 栈

       1. 单调栈

          1. 接雨水的单调栈

             1. 若要能储水，则必须产生凹槽，对应函数图像即产生向上转折

             2. 从左到右遍历数组，将nums[i]的索引逐个加入Stack中，如果Stack为空或者当前值<=栈顶元素，则继续加入栈，直到出现第一个比栈顶元素达的元素（加数组索引的原因是不仅可以获取索引值也能获取索引对应的数组值，后续面积计算需要计算索引差值，因而保留索引）

             3. 然后判断栈元素个数>=2的时候才会产生凹槽，否则一定是直接单调递增，那么当前元素左边的均应该弹出栈

             4. 弹出栈顶top，同时判断栈此时是否为空，为空说明一直单调递增，那么因该弹出前面的元素，不为空则查看栈元素作为left

             5. 逐个消除凹槽，并计算对应的面积值，S+=(i-left-1)*(Min(nums[i],nums[left])-nums[top])

             6. 核心代码逻辑

                ```java
                int[] nums;
                int area=0;
                Stack<Integer> stack = new Stack<>();
                for(int i=0;i<nums.length;i++){
                    //当出现凹槽向上，则循环计算前面所有符合凹槽的点的储水量
                    while(!stack.isEmpty() && nums[i]>nums[stack.peek()]){
                        //拿到栈顶元素索引，作为即将计算面积的点
                        Integer top = stack.pop();
                        //如果等于空说明栈中只有一个元素，那么此时nums[i]>nums[stack.peek()]则说明栈单调递增，不合要求
                        if(!stack.isEmpty()){
                            area += (i-left-1)*(Min(nums[i],nums[left])-nums[top])
                        }
                    }
                    stack.push(i);
                }
                
                //栈的作用真的很神奇，但是栈的操作原理有真的很抽象，多做，多理解，多感悟吧
                ```

                

    7. 二叉树

    8. 位运算

       1. 位运算和集合

          1. 定义：一个非负整数集合S={m，n，k...}可以表示成以下公式,用集合中的每个元素作为2的幂指数，如{0，2，3}等价于2^0+2^2+2^3=13

             
             $$
             f(S) = \sum_{i \in S} 2^i
             $$
             

          2. 集合与集合的关系

             | 术语       | 集合       | 位运算            | 举例                                 | 举例                                        |
             | ---------- | ---------- | ----------------- | ------------------------------------ | ------------------------------------------- |
             | 交集       | A∩B        | a&b               | {0, 2, 3} ∩ {0, 1, 2} = {0, 2}       | 1101 & 0111 = 0101                          |
             | 并集       | A∪B        | a\|b              | {0, 2, 3} ∪ {0, 1, 2} = {0, 1, 2, 3} | 1101 \| 0111 = 1111                         |
             | 对称差     | A Δ B      | a^b               | {0, 2, 3} Δ {0, 1, 2} = {1, 3}       | 1101 ^ 0111 = 1010                          |
             | 差         | A∖B        | a&~b              | {0, 2, 3} ∖ {1, 2} = {0, 3}          | 1101 & 1001 = 1001                          |
             | 差（子集） | A∖B（A⊆B） | a^b               | {0, 2, 3} ∖ {0, 2} = {3}             | 1101 ^ 0101 = 1000                          |
             | 包含于     | A⊆B        | a&b=a<br />a\|b=b | {0, 2} ⊆ {0, 2, 3}                   | 0101 & 1101 = 0101<br />0101 \| 1101 = 1101 |

          3. 集合与元素的关系

             | 术语                     | 集合           | 位运算                   | 举例                                 | 举例                                 |
             | ------------------------ | -------------- | ------------------------ | ------------------------------------ | ------------------------------------ |
             | 空集                     | ∅              | 0                        | ∅                                    | 0                                    |
             | 单元素集合               | {i}            | 1 << i                   | {2}                                  | 1 << 2                               |
             | 全集                     | U={0,1,2,⋯n-1} | (1 << n) - 1             | {0,1,2,3}                            | (1 << 4) - 1                         |
             | 补集                     | ∁S = U∖S       | ∼S 或 ((1 << n) - 1) ⊕ S | ∼{0,2,3} 或 ((1 << 4) - 1) ⊕ {0,2,3} | ∼{0,2,3} 或 ((1 << 4) - 1) ⊕ {0,2,3} |
             | 属于                     | i∈S            | (s >> i) & 1 = 1         | 2∈{0,2,3}                            | (1101 >> 2) & 1 = 1                  |
             | 不属于                   | i∉S            | (s >> i) & 1 = 0         | 1∉{0,2,3}                            | (1101 >> 1) & 1 = 0                  |
             | 添加元素                 | S∪{i}          | s\|(1 << i)              | {0,3} ∪ {2}                          | 1001 ∣ (1 << 2)                      |
             | 删除元素                 | S∖{i}          | s & ∼(1 << i)            | {0,2,3} ∖ {2}                        | 1101 & ∼(1 << 2)                     |
             | 删除元素（一定在集合中） | S∖{i}（i∈S）   | s ⊕ (1 << i)             | {0,2,3} ∖ {2}                        | 1101 ⊕ (1 << 2)                      |
             | 删除最小元素             |                | s & (s - 1)              |                                      |                                      |

          4. 遍历集合

             ```java
             for (int i = 0; i < n; i++) {
                 if (((s >> i) & 1) == 1) { // i 在 s 中
                     // 处理 i 的逻辑
                 }
             }
             ```

          5. 枚举集合

             ```java
             for (int s = 0; s < (1 << n); s++) {
                 // 处理 s 的逻辑
             }
             ```

          6. Java自带位运算API

             | 术语                                   | Java                                 |
             | -------------------------------------- | ------------------------------------ |
             | 集合大小（元素个数）                   | Integer.bitcount(s)                  |
             | 二进制长度（减一得到集合中的最大元素） | 32 - Integer.numberOfLeadingZeros(s) |
             | 集合中的最小元素                       | Integer.numberOfTrailingZeros(s)     |

          7. 基本巩固（求全排列）

             ```java
             //此题应用到了遍历集合和枚举集合，基础模板应当熟记于心
             public static List<List<String>> quanPaiLie(String[] strs){
                 //String[] strs =new String[]{"A", "B", "C"}
                 List<List<String>> rets =new ArrayList<>();
                 //计算长度n
                 int n = strs.length;
                 //枚举集合所有子集
                 for (int i = 0; i < (1<<n); i++) {
                     List<String> ret= new ArrayList<String>();
                     //遍历每个子集中元素的所有索引
                     for (int j = 0; j < n; j++) {
                         //子集中含有某个元素
                         if (((i>>j)&1)==1){
                             ret.add(strs[j]);
                         }
                     }
                     rets.add(ret);
                 }
                 return rets;
             };
             ```

             

    9. 图

    10. 动态规划

       1. 基础递推

          1. 接雨水的动态规划法

             1. 某个点的储水量由该点左边的最大值和右边的最大值的相对较小（短板）决定

             2. 上述求得的较小的短板值-该点的高度，如果结果>0，则该值就是储水量，如果<0则说明此处无法盛水，即储水量为0

             3. 某点储水量=Max(Min(Max(height[0]~height[i-1]),Max(height[i+1]~height[height.length-1]))-height[i],0)

             4. 核心逻辑代码

                ```java
                
                int[] nums;
                int[] leftMax = new int[nums.length];
                //动态规划求每个点对应左边的最大值列表
                leftMax[0]=0;//数列base值
                for(int left=1;left<nums.length;left++){
                    //数列递推公式
                    leftMax[i]=Math.max(leftMax[i-1],nums[i-1]);
                }
                int[] rightMax = new int[nums.length];
                rightMax[nums.length-1]=0;//数列base值
                //动态规划求每个点对应右边的最大值列表
                for(int right=nums.length-1;right>=0;right--){
                    //数列递推公式
                    rightMax[right]=Math.max(rightMax[right+1],nums[right+1]);
                }
                
                //储水量
                int sum =0;
                for(int i=1;i<nums.length;i++){
                	sum+=Math.max(Math.min(leftMax[i],rightMax[i])-nums[i],0);
                }
                ```

          2. 最大子数组和

             1. 给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

             2. 首先想到暴力枚举，那么显然需要n3的时间复杂度，显然不可取

             3. 其次如果考虑前缀和计算出数组的前缀和的话，会发现显然还需要n2的时间复杂度，也不尽人意

             4. 最后如果考虑动态规划则问题得到巧妙地解决

                1. 状态定义dp[i]：以nums[i]结尾的子数组的最大和值

                2. 状态转移方程：dp[i]=dp[i-1]<=0?nums[i]:nums[i]+dp[i-1]

                3. 初始状态：dp[0]=nums[0]

             5. 核心逻辑代码

                ```java
                dp[0]=nums[0];
                //最大连续子数组和值
                int maxSum=dp[0];
                for(int i=1;i<nums.length;i++){
                    dp[i]=dp[i-1]<=0?nums[i]:nums[i]+dp[i-1];
                	maxSum=Math.max(maxSum,dp[i]);
                }
                return maxSum;
                
                //优化
                //由题目可知,每个dp[i]元素都只是在不断前进的过程中被立刻使用,不存在回表反复查询,故可以使用双变量交替,降低空间复杂度
                int before=nums[0];
                int maxSum=before;
                for(int i=1;i<nums.length;i++){
                    before=before>0?before+nums[i]:nums[i];
                    maxSum =Math.max(maxSum,before);
                }
                return maxSum;
                
                //升华
                //本题旨在理解动态规划做题过程，同时掌握解题基本流程和套路
                ```

          3. [除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

             1. 题目要求不使用除法和O(n)时间复杂度，不要多想，朝着dp思考

             2. 其次，题目强调任意元素的全部前缀元素和后缀的乘积都在32位整数范围内则朝着前缀积和后缀积的方向思考

             3. 动态规划要素

                1. 状态:beforeSum[i]/afterSum[i]分别代表nums[i]元素的前缀积和后缀积

                2. 状态转移方程:
                   1. beforeSum[i]=beforeSum[i-1]*nums[i-1]

                   2. afterSum[i]=afterSum[i+1]*afterSum[i+1]

                3. 初始状态:beforeSum[0]=1,afterSum[nums.length-1]=1

             4. 核心逻辑代码

                ```java
                //常规方法T(n)+S(n)
                //定义状态
                int len =nums.length;
                int[] answer = new int[len];
                int[] beforeSum = new int[len];
                int[] afterSum = new int[len];
                //初始值
                beforeSum[0]=1;
                afterSum[len-1]=1;
                for(int i=1;i<len;i++){
                    //基本动态规划递推
                    beforeSum[i]=beforeSum[i-1]*nums[i-1];
                    //i-0=len-1-x => x=len-1-i
                    afterSum[len-1-i]=afterSum[len-i]*nums[len-i];
                }
                for(int i=0;i<len;i++){
                    answer[i]=beforeSum[i]*afterSum[i];
                }
                
                return answer;
                
                //进阶方法T(n)+S(1)
                //定义状态
                int len =nums.length;
                int[] answer = new int[len];
                //将原先的beforeSum和afterSum分部乘到answer中，因而初始化1
                Arrays.fill(answer,1);
                int beforeSum = 1;
                int afterSum = 1;
                //初始值
                for(int i=0;i<len;i++){
                    //基本动态规划递推
                    beforeSum=beforeSum*(i==0?1:nums[i-1]);
                    afterSum=afterSum*(i==0?1:nums[len-i]);
                    answer[i]*=beforeSum;
                    answer[len-i-1]*=afterSum;
                }
                
                return answer;
                ```

                

       2. 前缀和

          1. 和为K的子数组

             1. 基本想法使用双指针枚举所有组合[i-j]，然后计算sum[i-j],但是这样时间复杂度n3，显然不是最佳方法

             2. 考虑sum[i-j]=sum[j]-dum[i-1]满足前缀和，则可以先用dp求出sum[i]=sum[i-1]+nums[i-1]

             3. 然后再次使用双指针枚举，此时求和计算省去o(n),因而时间复杂度变为n2

             4. 此时可以看出还符合两数之和的模型，只不过这里是两数之差，稍微变形即可

             5. 核心逻辑代码

                ```java
                // 前缀和+哈希(类似两数之和的策略) O(n)+O(n)
                int[] sum =new int[nums.length+1];
                int count=0;
                for(int i=1;i<sum.length;i++){
                    //前缀和动态规划
                    sum[i]=sum[i-1]+nums[i-1];
                }
                Map<Integer,Integer> map = new HashMap<>();
                for(int i=0;i<sum.length;i++){
                    //参考两数字和，只不过两数之和这里应该是k-sum[i]
                    if(map.containsKey(sum[i]-k)){
                        count+=map.get(sum[i]-k);
                    }
                    //两数之和求得是元素索引，同时保证答案唯一，所以此处value存的是索引
                    //但是本体求的是满足条件的所有个数，因而将key进行计数，从而得到全部个数
                    map.put(sum[i],map.getOrDefault(sum[i],0)+1);
                }
                
                return count;
                ```

                