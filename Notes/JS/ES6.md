- 字符串拼接

  - ``` js
    var x =123;
    var y =`前缀${x}后缀`;//前缀123后缀
    ```

- 数组化

  - ```js
    var a ="123";
    var arr = [...a];//["1","2","3"]
    ```

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