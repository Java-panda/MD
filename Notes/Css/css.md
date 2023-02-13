CSS

- 语法

  - ``` css
    选择器 {
        属性名:属性名;
        ...
        属性名:属性名;
    }
    ```

- CSS样式种类

  - 外部样式

    - ``` css
      <head>
      <link rel="stylesheet" type="text/css" href="外部样式.css">
      </head>
      ```

  - 内部样式

    - ``` css
      <head>
          <style>
      		内部样式
          </style>
      </head>
      ```

  - 内联样式

    - ``` css
      <p style="属性名:属性值;属性名:属性值"></p>
      ```

- 选择器

  - ID选择器:#id

  - Class选择器:.class

  - 标签选择器:tag

  - 伪类选择器

    - ``` css
      //类box 下的第三个div元素
      .box div:nth-child(3)
      //ul 下面第/最后一个li元素
      ul li:first-child/last-child
      //类box 鼠标悬浮
      .box:hover
      ```

    - 

  - 属性选择器

  - 伪元素选择器

    - ``` css
      //匿名前后相邻元素
      selector::before{
          content: "必须属性";
          display: block;//默认是行内元素,转化为块级元素
      }
      
      selector::after{
          content: "必须属性";
          display: block;//默认是行内元素,转化为块级元素
      }
      ```

- 组合选择器

  - ","并集选择器

    - ``` css
      h1,h2,p{}/*对h1或h2或p元素操作*/
      ```

  - ""交集选择器

    - ``` css
      p.class{}/*对p且.class的元素操作*/
      ```

  - " "后代选择器

    - ``` css
      .class p{}/*对.class下的p元素操作*/
      ```

  - ">"子代选择器

    - ``` css
      div>p{}/*对div后面直接子代p元素操作*/
      ```

    - 

  - "+"紧邻兄弟组合器

    - ``` css
      div+p{}/*对div后面第一个兄弟元素,且元素是p的元素操作*/
      ```

  - "~"一般兄弟组合器

    - ``` css
      div~p{}/*对div后面所有兄弟元素,且元素是p的元素操作*/
      ```

- 层叠性

  - 覆盖就近原则

- 继承性

  - 

- 优先级

  - 样式种类
    - 内联样式 > 内部样式 > 外部样式

  - 选择器
    - id 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 标签选择器 = 伪元素选择器

    - ```css
      !important 覆盖所有优先级,成为最高级
      ```

  - | 选择器        | 权重   |
    | ------------- | ------ |
    | 继承/*        | 0000   |
    | 标签选择器    | 0001   |
    | 类/伪类选择器 | 0010   |
    | ID选择器      | 0100   |
    | 行内style     | 1000   |
    | !important    | 最高级 |

    

- 盒子模型

  - ![CSS box-model](https://www.runoob.com/images/box-model.gif)
  - Margin(外边距)
  - Border(边框)
  - Padding(内边距)
  - Content(内容)
  - 总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距
  - 总元素的高度=高度+顶部填充+底部填充+上边框+下边框+上边距+下边距

- 属性设置默认顺序

  - 上>右>下>左

    ``` css
    border-style:dotted solid double dashed;
    margin:25px 50px 75px 100px;
    padding:25px 50px 75px 100px;
    ```

- transform

  - 2D

  - ``` css
    //定义变换原点
    transform-origin: left bottom;
    //x,y坐标平移
    translate(x,y)
    translateX(x)
    translateY(y)
    
    //x,y放大系数
    scale(x,y)
    scaleX(x)
    scaleY(y)
    
    //旋转90°
    transform: rotate(90deg)
    ```

  - 3D

  - ``` css
    //透视距离,设定在父元素上面
    perspective: 1000px;
    //保留子元素的3D效果
    transform-style: preserve-3d;
    //定义变换原点
    transform-origin: left bottom;
    
    //x,y,z坐标平移
    translate3d(x,y,z)
    translateX(x)
    translateY(y)
    translateZ(z)
    
    //x,y,z放大系数
    scale3d(x,y,z)
    scaleX(x)
    scaleY(y)
    scaleZ(z)
    
    //x,y,z向量旋转
    rotate3d(x,y,z,90deg)
    rotateX(x)
    rotateY(y)
    rotateZ(z)
    ```

- transition

  - ``` css
    //过度0.5s
    transition: all 0.5s;
    ```

- animation

  - ``` css
    //定义动画
    @keyframes r {
        0% {
            //状态0%
        }
        100% {
            //状态100%
        }
    }
    //调用动画名称
    animation-name: 动画名称;
    //动画持续时间
    animation-duration: 10s;
    //动画延时1s
    animation-delay: 1s;
    //动画过渡效果,linear,step
    animation-timing-function: linear;
    //动画周期无限
    animation-iteration-count: infinite;
    //动画结束状态保持在结束位置
    animation-fill-mode: forwards;
    //动画循环折返
    animation-direction: alternate;
    //暂停动画
    animation-play-state: paused;
    ```

  - 

- 网页布局

  - 分类

    - 标准流
    - 浮动
    - 定位

  - 法则

    - 纵向排列考虑标准流

    - 横向排列考虑浮动

    - 行内元素添加浮动后会变成行内块元素

    - 如果块级元素没有设置宽度默认同父元素,但是如果添加浮动后将会根据内容自适应宽度

    - 浮动盒子之间没有间隙

    - 清除浮动

      - 后面添加空元素

      - 父元素添加overflow属性

      - 父元素添加after伪元素

      - ``` css
        .box::after {
            content: "";
            display: block;
            height: 0;
            visibility: hidden;
            clear: both;
        }
        ```

      - 父元素添加before,after伪元素

      - ``` css
        .box::before,
        .box::after {
            content: "";
            display: table;
        }
        
        .box::after {
            clear: both;
        }
        ```

      - 

- Flex布局

  - ```css
    .parent{
        border: 1px solid red;
        height: 500px;
        /*padding: 10px 10px;*/
    
        display: flex; /* 声明父元素flex布局*/
        /* 设置主轴方向 */
        flex-direction: column-reverse; /* 纵向反转*/
        flex-direction: column; /* 纵向*/
        flex-direction: row-reverse; /* 横向反转*/
        flex-direction: row; /* 横向*/
    
        /* 设置主轴方向对齐方式:左,又,居中,循环间距相等 */
        justify-content: start;/* 左对齐*/
        justify-content: end;/* 右对齐*/
        justify-content: center; /* 居中对齐*/
        justify-content: space-around; /* 环绕循环均分对齐*/
        justify-content: space-between; /* 两侧靠边,中间均分对齐*/
        justify-content: space-evenly;/* 所有间隙均分对齐*/
    
        /* 是否换行 */
        flex-wrap: wrap-reverse;/*换行反转*/
        flex-wrap: wrap;/*换行*/
        flex-wrap: nowrap;/*默认不换行*/
    
        /* 侧轴对齐方式 单行 */
        align-items: stretch; /*自动拉伸,子元素不能设置高度*/
        align-items: flex-end;
        align-items: flex-start;
        align-items: center;
    
        /* 侧轴对齐方式 多行 */
        align-content: flex-start;
        align-content: flex-end;
        align-content: space-around;
        align-content: space-between;
        align-content: stretch;
        align-content: center;
    
    }
    .child{
        /* 设置子元素权重*/
        flex: 1;
        height: 100px;
        background-color: aquamarine;
        margin: 10px 5px;
    }
    .child:nth-child(3){
        /* 设置子元素权重*/
        flex: 3;
        /* 设置子元素权重*/
        /* 排序数字越小越靠前*/
        order: -1;
        align-self: flex-end;
    }
    ```

  - 

- 属性列表:link:https://www.runoob.com/cssref/css-reference.html

- 常用属性

  - ``` css
    #鼠标放上去后显示小手图标
    cursor: pointer;
    //不显示背面
    backface-visibility: hidden;
    //nowrap禁止文本换行 normal自动换行
    white-space: nowrap;
    //溢出盒子范围的内容隐藏
    overflow: hidden;
    //相对位置
    position: relative;
    //绝对位置,子元素以父元素为参考,绝对坐标
    position: absolute;
    ```

  - ``` css
    #块元素(有宽高,自动换行)
    display: block;
    #行内元素(无宽高,不换行)
    display: inline;
    #块级行内元素(有宽高,不换行)
    display: inline-block;
    ```

- BootStrap

  - 1
  - 1
  - 1
  - 知识点
    - 列嵌套时最好加上一个row这样可以避免父元素的自带padding,并且会继承父元素的高


​	