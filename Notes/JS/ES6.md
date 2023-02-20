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

- 模块导入/导出

  - Node默认支持CommonJS

    - 配置Node支持ES6
      - package.json中加入*"type"*:"module"

    - 配置Node支持ES6后如果还需使用CommonJS则需要把js文件后缀改为cjs

    CommonJS

    - ```js
      //导入模块
      const m = require('path')
      
      //导出模块
      moudule.exports={
          属性名:属性值,
          方法名:function(){},
          方法名:()=>{}
      }
      ```

  - ES6

    - ```js
      //导入模块
      import 默认,{按需} from "path"
      
      //导出模块
      export 按需导出
      export default{
          属性名:属性值,
          方法名:function(){},
          方法名:()=>{}
      }
      
      //导入并直接执行一个js
      import from "path"
      ```

- Promise

  - 链式执行

    - ```js
      import fs from "then-fs"
      fs.readFile("src/node/a.txt","utf8")
      .then(res=>{
          console.log(res)
          return fs.readFile("src/node/b.txt","utf8")
      })
      .then(res=>{
          console.log(res)
          return fs.readFile("src/node/c.txt","utf8")
      })
      .then(res=>{
          console.log(res)
      })
      .catch(res=>{
          console.log(res)
      })
      ```

  - 数组执行

    - ```js
      const pArr=[fs.readFile("src/node/a.txt","utf8"),fs.readFile("src/node/b.txt","utf8"),fs.readFile("src/node/c.txt","utf8")]
      Promise.all(pArr)
      .then(([a,b,c,])=>{
          console.log(a)
          console.log(b)
          console.log(c)
      })
      .catch(res=>{
          console.log(res)
      })
      ```

  - 争夺执行

    - ```js
      const pArr=[fs.readFile("src/node/a.txt","utf8"),fs.readFile("src/node/b.txt","utf8"),fs.readFile("src/node/c.txt","utf8")]
      Promise.race(pArr)
      .then((res)=>{
          console.log(res)
      })
      .catch(res=>{
          console.log(res)
      })
      ```

  - 手动封装一个Promise方法

    - ```js
      function getFile(fPath) {
          return new Promise(function (resolve,reject) {
              fs.readFile(fPath,"utf8",(err,res)=>{
                  if (err) {
                      reject(err)
                  }else{
                      resolve(res)
                  }
              })
          })
      }
      
      getFile("src/node/a1.txt").then(res=>{
          console.log(res);
      })
      ```

- async/await

  - ```js
    async function getAllFile(){
       const res1 = await fs.readFile("src/node/a.txt","utf8")
       console.log(res1);
       const res2 = await fs.readFile("src/node/b.txt","utf8")
       console.log(res2);
       const res3 = await fs.readFile("src/node/c.txt","utf8")
       console.log(res3);
    }
    
    getAllFile()
    ```

  - 


​	