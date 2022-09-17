# 算法

1. 算法复杂度

   1. 时间复杂度(Time Complexity)
      1. 常数项忽略
      2. 低次项忽略
      3. 系数项忽略
   2. 空间复杂度(Space Complexity)

2. 数据结构的本质

   - 数组
     - 顺序存储
     - 查询快,增删慢
   - 链表
     - 链式存储
     - 查询慢,增删快

3. 算法的本质
   - 以数据结构为基础对数据进行增,删,改,查

4. 经典排序算法

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

5. 查找算法

   1. 线性查找
   2. 二分查找
   3. 插值查找
   4. 斐波那契查找

6. 树

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

7. 算法模板

   1. 回溯算法

      解决决策树遍历问题

      三大要素

      1. 路径:已经做出的选择
      2. 选择列表:接下来可做的选择
      3. 结束条件:到达决策树叶子节点

      模板

      ``` java
      /**
       *回溯算法基本模板
       *
       */
      //最终路线结果集
      private  static List<List<Integer>> resultList= new ArrayList<List<Integer>>();
      public static void backTrace(int[] choices,List<Integer> route){
          // 结束条件
          if (route.size()==choices.length){
              resultList.add(new ArrayList<Integer>(route));
              return;
          }
          // 遍历选择列表
          for (Integer choice:choices){
              // 做选择(剪枝)
              route.add(choice);
              // 回溯
              backTrace(choices,route);
              // 撤销选择
              route.remove(route.size()-1);
          }
      }
      ```
      
      123
