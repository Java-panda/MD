### C单片机

1. 软件安装(参考网络文档自行解决环境问题)
   1. Keil
   2. Proteus

2. C基础

3. 程序案例

   1. 点亮P10的LED灯

      ```c
      //引入80C52头文件
      #include<reg52.h>
      void main(){
      	while(1){
              //设置P1端口第一位为低电平0
      		P1=0xfe;//11111110B;
      	}
      }
      ```

   2. 

   3. 

   4. 

   5. 

   6. 

   7. 

      ```c
      //引入80C52头文件
      #include<reg52.h>
      void main(){
      	while(1){
              //设置P1端口第一位为低电平0
      		P1=0xfe;//11111110B;
      	}
      }
      ```