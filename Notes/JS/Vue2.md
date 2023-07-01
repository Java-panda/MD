Vue

- 工程化

  - 模块化
    - JS,CSS的复用
  - 组件化
    - UI结构的复用
  - 规范化
    - 工程目录结构,代码风格统一标准
  - 自动化
    - 自动打包,编译

- 工具

  - Node等同于JDK

  - Webpack(打包,编译,解决es5,6兼容性问题)等同于Spring Boot

  - NPM(Node package Manage)等同于后端的Maven

    - ```bat
      npm install jquery@3.6.3 --save #下载依赖并加到dependencies(生产必备)
      npm install jquery@3.6.3 --save-dev #下载依赖并加到devDependencies(开发所需)
      ```

    - 

- Webpack项目结构理解

  - src(开发代码)

  - babel.config.js

    - ```js
      module.exports = {
        plugins: [['@babel/plugin-proposal-decorators', {legacy: true}]]
      }
      ```

  - webpack.config.js

    - ```js
      const path = require('path')
      
      // 1. 导入 html-webpack-plugin 这个插件，得到插件的构造函数
      const HtmlPlugin = require('html-webpack-plugin')
      // 2. new 构造函数，创建插件的实例对象
      const htmlPlugin = new HtmlPlugin({
        // 指定要复制哪个页面
        template: './src/index.html',
        // 指定复制出来的文件名和存放路径
        filename: './index.html'
      })
      
      // 注意：左侧的 { } 是解构赋值
      const { CleanWebpackPlugin } = require('clean-webpack-plugin')
      
      // 使用 Node.js 中的导出语法，向外导出一个 webpack 的配置对象
      module.exports = {
        // 在开发调试阶段，建议大家都把 devtool 的值设置为 eval-source-map
        // devtool: 'eval-source-map',
        // 在实际发布的时候，建议大家把 devtool 的值设置为 nosources-source-map 或直接关闭 SourceMap
        devtool: 'nosources-source-map',
        // mode 代表 webpack 运行的模式，可选值有两个 development 和 production
        // 结论：开发时候一定要用 development，因为追求的是打包的速度，而不是体积；
        // 反过来，发布上线的时候一定能要用 production，因为上线追求的是体积小，而不是打包速度快！
        mode: 'development',
        // entry: '指定要处理哪个文件'
        entry: path.join(__dirname, './src/index1.js'),
        // 指定生成的文件要存放到哪里
        output: {
          // 存放的目录
          path: path.join(__dirname, 'dist'),
          // 生成的文件名和路径
          filename: 'js/bundle.js'
        },
        // 3. 插件的数组，将来 webpack 在运行时，会加载并调用这些插件
        plugins: [htmlPlugin, new CleanWebpackPlugin()],
        devServer: {
          // 首次打包成功后，自动打开浏览器
          open: true,
          // 在 http 协议中，如果端口号是 80，则可以被省略
          port: 80,
          // 指定运行的主机地址
          host: '127.0.0.1'
        },
        module: {
          rules: [
            // 定义了不同模块对应的 loader
            { test: /\.css$/, use: ['style-loader', 'css-loader'] },
            // 处理 .less 文件的 loader
            { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
            // 处理图片文件的 loader
            // 如果需要调用的 loader 只有一个，则只传递一个字符串也行，如果有多个loader，则必须指定数组
            // 在配置 url-loader 的时候，多个参数之间，使用 & 符号进行分隔
            //outputPath打包输出路径
            { test: /\.jpg|png|gif$/, use: 'url-loader?limit=470&outputPath=images' },
            // 使用 babel-loader 处理高级的 JS 语法
            // 在配置 babel-loader 的时候，程序员只需要把自己的代码进行转换即可；一定要排除 node_modules 目录中的 JS 文件
            // 因为第三方包中的 JS 兼容性，不需要程序员关心
            { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
          ]
        },
        resolve: {
          alias: {
            // 告诉 webpack，程序员写的代码中，@ 符号表示 src 这一层目录
            '@': path.join(__dirname, './src/')
          }
        }
      }
      ```

  - package.json

    - ```json
      {
        "name": "项目名",//项目名
        "version": "1.0.0",//版本号
        "description": "",//描述
        "main": "index.js",//入口js
        "scripts": {//命令集
          "test": "echo \"Error: no test specified\" && exit 1",//执行测试
          "dev": "webpack serve",//执行开发环境启动(打包到内存,并非物理磁盘)
          "build": "webpack --mode production"//打包到物理磁盘,供生产环境用
        },
        "keywords": [],
        "author": "",//作者
        "license": "ISC",//协议
        "dependencies": {//生产所需依赖
          "jquery": "^3.6.0"
        },
        "devDependencies": {//开发所需依赖  
          "@babel/core": "^7.14.6",//babel核心
          "babel-loader": "^8.2.2",//babel加载器
          "@babel/plugin-proposal-decorators": "^7.14.5",//解析JS装饰器语法
          "html-webpack-plugin": "^5.5.0",//复制src目录下index.html到根目录(内存)
          "less": "^4.1.3",//解析LESS
          "less-loader": "^11.1.0",//解析LESS
          "css-loader": "^6.7.3",//解析CSS
          "style-loader": "^3.3.1",//解析style
          "file-loader": "^6.2.0",//解析图片URL
          "url-loader": "^4.1.1",//解析图片URL
          "webpack": "^5.75.0",//webpack
          "webpack-cli": "^5.0.1",//webpack脚手架
          "webpack-dev-server": "^4.11.1"//webpack热部署插件
        }
      }
      ```

    - 

- 相关前置知识

  - axios

    - ```js
      //then/catch方式
      function(){
          axios({
            method:"GET/POST/PUT/DELETE",
            url:"localhost:9999/getUserById",
            params: {},//GET路径参数
            data: {}//POST请求体
          })
          .then(res =>{
              console.log(res.data)
            }
          )
          .catch(res =>{
              console.log(res.data)
            }
          )
      }
      
      //async+await
      async function(){
          {data: res} = await axios({
            method:"GET/POST/PUT/DELETE",
            url:"localhost:9999/getUserById",
            params: {},//GET路径参数
            data: {}//POST请求体
          })
          console.log(res)
      }
      ```

- 基本原理

  - 数据双向绑定
  - 数据驱动视图
  - HelloWorld
  - ![image-20230213200915894](C:\Users\77023\Desktop\MD笔记\MD\Notes\JS\images\image-20230213200915894.png)

- 指令

  - 内容渲染指令

    - ```bat
      v-text="'v-text'+msg"
      v-html="'v-html'+msg"
      {{ msg }}
      ```

  - 属性绑定指令

    - ```bat
      :title="'v-bind' + msg" #简写
      v-bind:title="'v-bind' + msg" #全写
      ```

  - 事件绑定指令

    - ```bat
      @click="method(参数,$event)" #简写
      v-on:click="method(参数,$event)" #全写
      
      #事件修饰
      @click.prevent="method" #阻止默认事件
      @click.stop="method" #阻止事件冒泡
      @click.capture="method" #使用捕捉模式
      @click.once="method" #阻止默认事件
      @click.self="method" #只对事件触发对象本身有效
      
      #按键修饰
      @keyup.esc="method" #松开ESC
      @keydown.enter="method" #按下回车
      ```

  - 双向绑定指令

    - ```bat
      v-model="msg"
      #只用于表单对象,可以进行用户输入的对象进行数据双向绑定
      
      #v-model修饰符
      v-model.number="msg" #自动转数字
      v-model.trim="msg" #自动清空两边空白
      v-model.lazy="msg" #输入完成后才进行V→M的数据同步,等效于@change换成@blur
      ```

  - 条件渲染指令

    - ```bat
      v-if="boolean" #true进行渲染加载
      v-else-if="boolean"
      v-else="boolean"
      
      v-show="boolean" #始终渲染加载,根据判断结果进行隐藏和显示
      ```

  - 列表渲染指令

    - ```bat
      v-for="(item,index) in items" :key="item.id"
      ```

- 属性

  - 过滤器(Vue3已经废弃)

    - 只能用在{{}}内和:两个地方

    - ```bat
      #私有过滤器
      filters: {
      	filterMethod(参数) {
      		return 返回值
      	}
      }
      
      #全局过滤器
      Vue.filter("filterMethod",function(参数){
      	return 返回值	
      })
      ```

  - 侦听器

    - ```bat
      el: "#app",
      data() {
        return { 
      	msg: "Hello Vue",
      	obj: {
      	  name: "姓名",
      	  age: 20
      	}
        };
      },
      watch:{
        #方法方式监听,无法设置立即执行和深度监听
        msg(nv,ov){
      	console.log("msg",nv,ov)
        },
        #属性方式监听,可以设置立即执行和深度监听,需要多写handler方法
        obj:{
      	handler(nv,ov){
      	  console.log("obj",nv,ov)
      	},
      	deep:true,
      	immediate:true
        },
        #直接监听对象属性
        'obj.name':{
      	handler(nv,ov){
      	  console.log("obj.name",nv,ov)
      	},
      	deep:true,
      	immediate:true
        }
      }
      ```

  - 计算属性

    - ```bat
      computed: {
        fullName: {
      	get(){
      	  return this.firstName + " " + this.lastName
      	},
      	set(newVal,oldVal){
      		  this.firstName = newVal.split(" ")[0]
      		  this.lastName = newVal.split(" ")[1]
      	}
        }
      }
      ```

    - 

- Vue-CLI

  - 安装/创建项目

    - ```bat
      npm install -g @vue/cli
      
      vue create 项目名
      
      cd 项目名
      npm run serve //开发环境
      npm run build //生产打包
      ```

  - 项目结构

    - node_modules:依赖包
    - public
      - 图标
      - 首页
    - src:开发代码
      - assets:静态资源
      - components:开发的组件
      - App.vue:根组件
      - main.js:程序入口
    - babel.config.js
    - package.json
    - vue.config.js

- 组件

  - 作用:抽象UI模型以便用于复用

  - 组成

    - ```vue
      <template>
        <div id="app">
        </div>
      </template>
      
      template下只能有一个根标签
      ```

    - ```vue
      <script>
      import HelloWorld from './components/HelloWorld.vue'
      
      export default {
        name: 'App',
        components: {
          HelloWorld
        }
      }
      </script>
      ```

    - ```vue
      lang="less"支持less
      scope防止式样冲突,之作用在this组件
      /deep/可以穿透到子组件修改子组件样式
      <style lang="less" scoped>
      /deep/ h3 {
        margin: 40px 0 0;
      }
      #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
      }
      </style>
      ```

  - 使用步骤

    - 导包组件

      - ```js
        import HelloWorld from './components/HelloWorld.vue'
        ```

    - 注册组件

      - ```js
        components: {HelloWorld}//私有
        
        //在main.js中注册全局组件
        import HelloWorld from './components/HelloWorld'
        Vue.component("HelloWorld",HelloWorld)
        ```

    - 标签传参

      - ```vue
        <template>
          <div id="app">
            <HelloWorld msg="Hello Vue"/>
          </div>
        </template>
        ```

  - 开发组件

    - ```vue
      <template>
        <div>
          <input type="text" v-model="dmsg">
          <input type="text" v-model="dname">
        </div>
      </template>
      
      <script>
      export default {
        name: 'HelloWorld',
        props: {//定义属性供调用者传参
          msg: {
            type: String,
            default: "Hello",
            required: true
          },
          name: {
            type: String,
            default: "Panda",
            required: false
          }
        },
        data(){
          return {
            dmsg: this.msg,//属性默认不允许修改进而转存到其他变量中
            dname: this.name
          }
        }
      }
      </script>
      <style scoped lang="less">
      
      </style>
      
      ```

  - 生命周期

    - 创建阶段
      - beforecreate:属性,数据,方法均为创建,基本不用
      - created:发送请求获取初始化加载数据
      - beforemount:准备挂载dom
      - mounted:dom结构渲染完成
    - 运行阶段(值发生变化触发)
      - beforeupdate
      - updated
    - 销毁阶段(v-if为false可以触发)
      - beforedestroy:销毁前
      - destroyed:销毁后

  - 组件传值

    - 父传子

      - 原理:子组件创建属性,父组件调用的时候传入属性值

    - 子传父

      - 原理:子组件发送事件并传值给父组件,父组件监听事件并获取值

    - 兄弟传值

      - 原理:借助一个事件总线,进行间接传值,相当于A传给父F,F再传给子B

    - ```vue
      main.js
      //创建事件总线,进行数据通信
      Vue.prototype.$bus =new Vue()
      
      ```

    - ```vue
      <template>
        <div>
          <h3>父组件</h3>
          <input type="text" v-model="dmsg">
          <Son :msg="dmsg" @sendMsgToFarther="getMsgFromSon"></Son>
          <Brother></Brother>
        </div>
      </template>
      
      <script>
      import Son from './Son.vue'
      import Brother from './Brother.vue'
      export default {
          components:{
              Son,Brother
          },
          props:{
              msg:{
                  type: String
              }
          },
          data() {
              return {
                  dmsg : this.msg
              }
          },
          methods: {
              getMsgFromSon(val){
                  this.dmsg = val
              }
          },
      }
      </script>
      
      <style scoped lang="less">
          h3{
              color:pink;
          }
      </style>
      
      
      <template>
        <div>
          <h3>子组件</h3>
          <input type="text" v-model="dmsg">
        </div>
      </template>
      
      <script>
      export default {
          props:{
              msg:{
                  type: String
              }
          },
          data() {
              return {
                  dmsg : this.msg
              }
          },
          methods: {
              sendMsgToFarther(){
                  
              }
          },
          watch:{
              msg(){
                  this.dmsg = this.msg
              },
              dmsg(){
                  this.$emit("sendMsgToFarther",this.dmsg)
                  this.$bus.$emit("share",this.dmsg)
              }
          }
      }
      </script>
      
      <style scoped lang="less">
          h3{
              color: aqua;
          }
      </style>
      
      
      <template>
        <div>
          <h3>兄弟组件</h3>
          <input type="text" v-model="dmsg">
        </div>
      </template>
      
      <script>
      export default {
          data() {
              return {
                  dmsg : ""
              }
          },
          created() {
              this.$bus.$on("share",val=>{
                  this.dmsg=val
                  }
              )
          },
          methods: {
          },
          watch:{
          }
      }
      </script>
      
      <style scoped lang="less">
          h3{
              color: aqua;
          }
      </style>
      ```

    - 操作Dom元素

      - 为元素设置ref属性,通过this.refs.xxx获取dom元素

    - this.nextTick

      - 实现dom加载完毕后回调函数

    - ```js
      this.$nextTick(()=>{
      	this.$refs.inputRef.focus()
      })
      ```

  - 动态组件

    - ```vue
      <keep-alive exclude="Right">//卸载是缓存,include,exclude包括或者排除的组件列表,二者只能选其一
          <component :is="compName"></component>
      </keep-alive>
      ```

  - 插槽

    - ```vue
      定义
      <slot name="header" :data="obj">默认值</slot>
      <slot name="content" :data="obj">默认值</slot>
      <slot name="footer" :data="obj">默认值</slot>
      ```

    - ```vue
      调用
      <template slot="header" slot-scope="scope"> //写法一
      	<p>{{scope.data.xxx}}</p>
      </template>
      <template #header="scope"> //写法二
      	<p>{{scope.data.xxx}}</p>
      </template>
      ```

  - 自定义指令

    - ```js
      Vue.directive('color',{
        bind(el,binding){
          console.log(el,binding);
          el.style.color=binding.value
        },
        update(el,binding){
          el.style.color=binding.value
        }
      })
      
      <div v-color="'red'"></div>
      ```

- 路由

  - 本质上画面上锚点#的变化

      - ```html
        <template>
          <div id="app">
            <router-link to="/a">A</router-link><br>
            <router-link to="b">B</router-link><br>
            <router-link to="/c">C</router-link><br>
            <hr>
            <router-view></router-view>
          </div>
        </template>
        ```

      - ```js
        //使用路由插件
        Vue.use(VueRouter)
        //实例化路由插件
        const router = new VueRouter({
            routes:[
                {
                    path:"/",
                    redirect:"/a"//重定向
                },
                {
                    path:"/a",
                    component:A
                },
                {
                    path:"/b",
                    component:B,
                    redirect:"/b/b1",
                    children:[
                        {
                            path:"b1",
                            component:B1
                        },
                        {
                            path:"b2",
                            component:B2
                        }
                    ]
                },
                {
                    path:"/c",
                    component:C,
                    children:[
                        {
                            path:"cc/:id",//动态路由
                            component:CC,
                            props:true//作为属性传给组件,组件内部需要声明该属性
                        }
                    ]
                }
            ]
        })
        ```

  - 

  - 动态路由

    - ```js
      /user/detail/1?name=lj&age=25
      
      
      获取路径参数
      //方式一
      <div>ID:{{ this.$route.params.id }}</div>
      //方式二通过路由配置中指定形参的props:true属性可以在组件内部通过属性值直接获取
      <div>ID:{{id}}</div>
      
      //其他常用
      <div>Name:{{this.$route.query.name}}</div>
      <div>Age:{{this.$route.query.age}}</div>
      <div>Path:{{this.$route.path}}</div>
      <div>FullPath:{{this.$route.fullPath}}</div>
      ```

  - 导航

      - ```js
          this.$router.push("/path")
          this.$router.replace("/path")
          this.$router.go(n)//前进n,负数为倒退
          this.$router.back()//倒退
          this.$router.forward()//前进
          ```

      - 导航守卫

          - ```js
              router.beforeEach((to,from,next)=>{
                  if (to.path==='/带权限路径') {
                      const token = sessionStorage.getItem('token')
                      if (token) {
                          //跳转到其他路径
                          next('/指定路径')
                      } else {
                          //原地不动
                          next(false)
                      }
                  } else {
                      //直接放行
                      next()
                  }
              })
              ```

          - 

- Vuex

