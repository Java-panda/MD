* 环境准备
    * 安装VS Code官网直接下载
        * 安装code runner插件
        * 右键代码文件即可运行代码
    * 安装Node JS官网直接下载
        * 验证安装是否成功
            * windows
                * win+r
                * cmd
                * node -v
        * * 变量/常量
```javascript
/*
入门Hello World
*/

//定义常量
var a="Hello World";

//定义常量
const c ="Hello World";

//输出语句
console.log(a);
gs.print(a);//global
gs.debug(a);//scope 
gs.info(a);//scope
```
* 注释
```javascript
//单行注释

/*
多行注释
*/
```

* 运算符
|+|加法(字符串时是拼接)|var x=1+2|3|
|:----|:----|:----|:----|
|-|减法|var x=2-1|1|
|*|乘法|var x=2*5|10|
|/|除法|var x=4/2|2|
|%|求余|var x=5%2|1|
|++|自增|var i =10;<br>i++;|11|
|--|自减|var i =10;<br>i--|9|

* 赋值运算符
|符号|示例|含义|
|:----|:----|:----|
|=|x=1|把1赋值给x |
|+=|x+=1|x=x+1|
|-=|x-=1|x=x-1|
|*=|x*=2|x=x*2|
|/=|x/=2|x=x/2|
|%=|x%=2|x=x%2|

* 比较符
|==|值相等|1==1|true|
|:----|:----|:----|:----|
|===|值和类型均相等|1=="1"|false|
|!=|不等|1!=2|true|
|!==|值和类型有一个不同就为true|    |    |
|>|大于|2>1|true|
|<|小于|    |    |
|>=|大于等于|    |    |
|<=|小于等于|    |    |

* 逻辑运算符
|&&|与|
|:----|:----|
|\|\||或|
|!|非|

* 变量类型
* 基本类型
```javascript
//undefine类型
var v;

//number类型
var age = 25;

//boolean类型
var isMale= false/true;

//string类型
var name ="panda"; 

//array数组类型(也是一个特殊的object类型)
var arr = [1,2,3,4,5,6,7,8,9];

//object类型
var person = {name:"zhangsan",age:25,gender:true};

//function类型
function add(a,b){
  return a+b;
}
```
* String类型常用方法
|charAt(i)|返回指定索引位置的字符|
|:----|:----|
|indexOf("指定字符")|返回字符串中检索指定字符第一次出现的位置|
|lastIndexOf("指定字符")|返回字符串中检索指定字符最后一次出现的位置|
|replace("旧字符串","新字符串")|替换某个部分|
|split("切割字符")|把字符串分割为子字符串数组|
|substring(start,end)|提取字符串中两个指定的索引号之间的字符(左闭右开)|
|toLowerCase()|转小写|
|toUpperCase()|转大写|
|startsWith("起始字符串")|是否以某个字符串开始的|
|endsWith("结束字符串")|是否以某个字符串结束的|

* 字符串补充
    * 字符串的拼接:"abc"+"def"
    * 字符串的转义:
|\'|单引号|
|:----|:----|
|\"|双引号|
|\\|反斜杠|
|\n|换行|
|\r|回车|
|\t|tab(制表符)|
|\b|退格符|
|\f|换页符|

* 数组类型(Array)
```javascript
//数组定义
var names =[];//空数组
var names = ["张三","李四","王五"];//带值数组
```
* 数组常用操作
|names.length|查看数组的长度|
|:----|:----|
|names[i]|获取数组某个下标的值(i的取值范围从0到names.length-1)|
|names[i]="新的值"|赋值数组某个下标所对应的值|
|names.push("周六")|最后位置加一个值|
|names.pop()|删除最后一个值,并返回该值|
|names.splice(start)<br>names.splice(start, deleteCount)<br>names.splice(start, deleteCount, item1)<br>names.splice(start, deleteCount, item1, item2, itemN)|增删改全能剪刀<br>[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)|
|names.slice()<br>names.slice(start)<br>names.slice(start,end)|截取子段前索引包含,后索引不包含[start,end)<br>[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)|

* 对象类型Object()
```javascript
//对象定义
person ={};//空对象
person ={
  name:"张三",
  age:25,
  isMale:true
};

/*
对象常用操作
*/

//获取对象某个属性的值
person.name;//获取某个属性的值方法一
person["name"];获取某个属性的值方法二

//赋值对象某个属性的值
person.name="新的值";//设置某个属性的值方法一
person["name"]="新的值";//设置某个属性的值方法二

```
* 对象常用操作
|person.name|获取某个属性的值方法一|
|:----|:----|
|person["name"]|获取某个属性的值方法二|
|person.name="新的值"|设置某个属性的值方法一|
|person["name"]="新的值"|设置某个属性的值方法二|
|    |    |

* 逻辑流程
```javascript
//条件语句if
var num=1;
if(num>0){
  console.log("正数");
}else if(num<0){
  console.log("负数");
}else{
  console.log("0");
}

//switch语句
var x=0;
switch(x){
  case 0:
    console.log("星期日");
    break;
  case 1:
    console.log("星期1");
    break;
  case 2:
    console.log("星期2");
    break;
  case 3:
    console.log("星期3");
    break;
  case 4:
    console.log("星期4");
    break;
  case 5:
    console.log("星期5");
    break;
  case 6:
    console.log("星期6");
    break;
  default:
    console.log("不是正常的星期数");
}

//三元运算符
var num=1;
num=num>0?"正数":"负数";

//fori循环
var arr=["a","b","c"];
for(var i=0;i<arr.length;i++){
    console.log(arr[i]);
}

//for in循环(作用于对象类型时获取Key值)
var person={name:"张三",age:25,isMale:true};
for(x in person){
    console.log(x);//name,age,isMale
    console.log(person[x]);//张三,25,true
}

//for in循环(作用于数组类型时获取数组索引值)
var arr=["a","b","c"];
for(x in arr){
    console.log(x);//0,1,2
    console.log(arr[x]);//a,b,c
}

//while循环
var i=1;
var sum=0;
while(i<=100){
    sum+=i;
    i++;
}
console.log(sum);

//do while循环
var i=1;
var sum=0;
do{
    sum+=i;
    i++;
}while(i<=100);
console.log(sum);

//break:用于结束循环
//continue:用于跳过循环

//数组遍历的lambda表达式(了解,不做要求)
var arr=["a","b","c"];
//遍历每一个不会停
arr.forEach((item,index)=>{
    console.log(item,index);
})
//遍历,return就会停止
arr.some((item,index)=>{
    console.log(item,index);
})
//过滤返回true的元素
arr.filter((item,index)=>{
    return item ==='b'
})
//全部满足返回true,否则false
arr.every((item,index)=>{
    return item.length ===1
})
//filter,map,reduce合用
var brr = [
    {status:true,count:1,price:10},
    {status:false,count:2,price:5},
    {status:false,count:4,price:2.5},
    {status:true,count:5,price:2}
]
brr.filter((item,index)=>item.status).map((item,index)=>item.count*item.price).reduce((s,t)=>s+=t,0)//20
```

* * 方法类型function()
```javascript
//方法的定义(方法命名规则小驼峰)
function functionName(param1,param2,...){
  //具体代码逻辑
  
  //如果需要返回值可以加上return语句
  return "返回值";
}

//方法的调用
functionName("第一个参数","第二个参数",...);
```
* 日期对象Date
|getDate()|从 Date 对象返回一个月中的某一天 (1 ~ 31)。|
|:----|:----|
|getDay()|从 Date 对象返回一周中的某一天 (0 ~ 6)。|
|getFullYear()|从 Date 对象以四位数字返回年份。|
|getHours()|返回 Date 对象的小时 (0 ~ 23)。|
|getMilliseconds()|返回 Date 对象的毫秒(0 ~ 999)。|
|getMinutes()|返回 Date 对象的分钟 (0 ~ 59)。|
|getMonth()|从 Date 对象返回月份 (0 ~ 11)。|
|getSeconds()|返回 Date 对象的秒数 (0 ~ 59)。|
|getTime()|返回 1970 年 1 月 1 日至今的毫秒数。|

日期用法

```javascript
var d =new Date();
console.log("getFullYear:"+d.getFullYear());
console.log("getMonth:"+(d.getMonth()+1));
console.log("getDate:"+d.getDate());
console.log("getDay:"+d.getDay());
console.log("getHours:"+d.getHours());
console.log("getMinutes:"+d.getMinutes());
console.log("getSeconds:"+d.getSeconds());
console.log("getMilliseconds:"+d.getMilliseconds());
console.log("getTime:"+d.getTime());
```
* 练习
99乘法表

冒泡排序

