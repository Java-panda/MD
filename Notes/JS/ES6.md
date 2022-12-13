- 变量声明

  - ``` js
    //全局变量
    var a = 1;
    //局部变量
    let b = 2;
    //常量,必须赋值且不能修改
    const c = 3;
    ```

- 解构赋值

  - ```js
    //数组
    let [a,b,c] =[1,2,3];
    //对象
    let user = {name: 'Helen', age: 18}
    let { name, age } =  user //注意：结构的变量必须是user中的属性
    ```

  - 

- 模板字符串

  - ``` js
    var x =123;
    var y =`前缀${x}后缀`;//前缀123后缀
    ```

- 数组化

  - ```js
    var a ="123";
    var arr = [...a];//["1","2","3"]
    ```

- 箭头函数

  - ```js
    (p)=>{
      console.log(p85*);  
    }
    ```

  - 

- 防止空指针

  - ```js
    var person ={
        "name":"lj"
    }
    person?.name;//"lj"
    var person =null;
    person?.name;//undefined
    ```

- 默认值

  - ```js
    var a= 12;
    var b = a || 20;//b=boolean(a)?a:20
    
    var a= 12;
    var b = a ?? 20;//b=null?20:a
    ```

  - 

​	