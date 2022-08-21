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
  - 伪类选择器
  - 属性选择器
  - 伪元素选择器
  - 标签选择器:tag

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

- 优先级

  - 样式种类
    - 内联样式 > 内部样式 > 外部样式
  - 选择器
    - id 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 标签选择器 = 伪元素选择器

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

- 属性列表:link:https://www.runoob.com/cssref/css-reference.html

​	