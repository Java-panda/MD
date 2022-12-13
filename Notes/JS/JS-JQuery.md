- 选择器

  - 获取元素

    - JS

      - ``` js
        //获取第一个元素
        document.querySelector("selector");
        //获取所有元素
        document.querySelectorAll("selector");
        ```

    - JQuery

      - ``` js
        //获取所有元素,DOM对象象定查询dom范围
        $("selector"[,DOM对象])
        ```

- 属性

  - 标签对象/文本/值

    - JS

      - ``` js
        //获取标签对象
        var html = Element.innerHTML;
        //设置标签对象
        html.innerHTML = "<span>123</span>";
        
        //获取标签文本
        var text = Element.innerText;
        //设置标签text
        text.innerText = "123";
        
        //获取标签值
        var val = Element.value;
        //设置标签值
        val.value = "123";
        ```

    - JQuery

      - ``` js
        //获取标签对象
        var html = $("selector").html();
        //设置标签对象
        html.html("<span>123</span>");
        
        //获取标签文本
        var text = $("selector").text();
        //设置标签text
        text.text("123");
        
        //获取标签值
        var val = $("selector").val();
        //设置标签值
        val.val("123");
        ```

  - 标签属性

    - JS

      - ```js
        //获取属性值
        document.getAttribute("attrName");
        //设置属性值
        document.setAttribute("attrName","attrValue");
        //删除属性
        document.setAttribute("attrName");
        ```

    - JQuery

      - ```js
        //获取属性值
        $("selector").attr("attrName");
        //设置属性值
        $("selector").attr("attrName","attrValue");
        //删除属性
        $("selector").removeAttr("attrName");
        
        //获取属性值
        $("selector").prop("propName");
        //设置属性值
        $("selector").prop("propName","propValue");
        //删除属性
        $("selector").removeProp("attrName");
        ```

  - Class属性

    - JS

      - ```js
        //是否具有class
        element.classList.contains("className");
        
        //添加class
        element.classList.add("className");
        
        //删除class
        element.classList.remove("className");
        
        //切换(存在就删除,不存在就添加)class
        element.classList.toggle("className");
        ```

    - JQuery

      - ```js
        //是否具有class
        $("selector").hasClass("className");
        
        //添加class
        $("selector").addClass("className");
        
        //删除class
        $("selector").removeClass("className");
        
        //切换class
        $("selector").toggleClass("className");
        ```

  - Css属性

    - JS

      - ```js
        //获取样式属性值
        element.style.styleName;
        //设置样式属性值
        element.style.styleName="styleValue";
        ```

      - 

    - JQuery

      - ```js
        //获取样式属性值
        $("selector").css("styleName");
        //设置样式属性值
        $("selector").css("styleName","styleValue");
        ```

      - 

- 事件

  - 画面加载完毕

    - JS

      - ``` js
        window.onload = function() {
            //do
        }
        ```

    - JQuery

      - ``` js
        //方法一
        $(document).ready(function(){
        	//do
        });
        
        //方法二
        $(function() {
            //do
        });
        ```

  - on

    - EventType

      - ```js
        click:点击
        dblclick:双击
        change:变化
        hover:鼠标选中
        focus:获取焦点
        focusin:获取进入焦点
        focusout:获取离开焦点
        blur:失去焦点
        select:选择
        submit:提交
        keydown:键盘按下
        keyup:键盘松开
        
        toggle:切换显示和隐藏
        ```

      - 

    - JS

      - ``` js
        //注册事件
        element.on???=function(){
            //do
        }
        //移除事件
        element.on???=null;
        
        //可多个相同事件
        element.addEventListener("???", function() {
        	//do
        },传播机制)//传播机制 true:捕获,false:冒泡
        element.removeEventListener("???", functionName)
        ```

      - 

    - JQuery

      - ```js
        $("selector").on("eventType",function(){
            //do
        });
        ```

      - 

- Dom操作

  - JQuery

    - ```js
      A.append(B)//B加到A内部最后
      A.appendTo(B)//A加到B内部最后(破坏)
      A.prepend(B)//B加到A内部最前
      A.prependTo(B)//A加到B内部最前(破坏)
      A.before(B)//B加到A的前面
      A.after(B)//B加到A的后面
      A.insertBefore(B)//A加到B的前面(破坏)
      A.insertAfter(B)//A加到B的后面(破坏)
      A.wrap(B)//A加上一层B壳
      A.wrapInner(B)//A的子元素加上一层B壳
      A.wrapAll(B)//第一个A加上一层B壳,但是其他A会after到第一个A
      A.unwrap(B)//A去掉上一层B壳
      A.clone()//克隆一个A避免破坏原来的A
      A.empty()//清空所有子元素包括文本
      A.replaceWith(B)//用B代替A
      A.replaceAll(B)//用A代替B
      A.remove()//删除A及其事件,数据,还原后事件无效
      A.detach()//删除A但是保留事件和数据,还原后事件还有效
      ```
  
- 筛选

  - JQuery

    - ``` js
      A.end()//返回A的上一级对象
      A.eq(索引)//获取第N个元素
      A.next()//获取A所在列表位置下一个元素
      A.prev()//获取A所在列表位置上一个元素
      A.children()//获取A的子元素
      A.parent()//获取A的父元素
      ```

    - 

- Ajax

  - JS

  - JQuery

    - ```js
      $.ajax({
          url:"",
          type: "GET",
          contentType: "application/json",
          data:{
          },
          dataType:"json",
          success:function (data, textStatus, jqXHR){
      
          },
          error:function (XMLHttpRequest, textStatus, errorThrown){
      
          }
      });
      ```

    - 

- JQuery手册

  - 元素隐藏/显示

    - ```js
      $("selector").show();
      $("selector").hide();
      ```

    - 

- 附录

  - 属性列表

    - ```js
      type:"text"
      src:""
      href:""
      disabled:true
      checked:true
      selected:true    
      display:"none/block"//隐藏和显示
      ```

    - 

  - 事件对象Event

    - ``` js
      //属性
      //触发对象原始对象
      event.target
      //冒泡过程中间接触发事件的当前对象
      this
      event.currentTarget
      
      //方法
      //阻止默认事件(如,组织a跳转,form提交)
      event.preventDefault();
      //阻止事件冒泡
      event.stopPropagation();
      ```

  - 事件委托

    - 原理:通过对父节点添加监听,避免给大量的相同子节点添加事件,提高性能,利用冒泡

  - 事件传播

    - ```js
      //捕获阶段
      document>html>body>div
          
      //冒泡阶段(默认)
      div>body>html>document
      ```

    - 

  - BOM(Browser Object Model)

    - document:前边所学的内容
    - location:
    - navigation:
    - screen:
    - history:

