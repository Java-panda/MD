XPath

- XPath 是一门在 XML 文档中查找信息的语言

- 节点

  - 元素节点
  - 属性节点
  - 文本节点
  - 命名空间
  - 处理指令
  - 注释
  - 文档节点/根节点

- 快捷使用

  - 打开Google浏览器,F12查看源代码
  - Ctrl+F可以弹出Xpath选择器
  
- 语法

  - | 规则                  | 含义                       |
    | --------------------- | -------------------------- |
    | nodename              | 获取nodename节点下所有节点 |
    | /rootnode             | 获取根节点                 |
    | //nodename            | 跨越查询nodename节点       |
    | //nodename1/nodename2 | 连续节点查询               |
    | //@atrr               | 获取attr属性节点           |

- 通配符

  - | 规则   | 含义               |
    | ------ | ------------------ |
    | *      | 匹配任意节点       |
    | @*     | 匹配任意属性       |
    | node() | 匹配任意类型的节点 |

  - 

- 限定条件

  - | 规则                               | 含义                                      |
    | ---------------------------------- | ----------------------------------------- |
    | //nodename[条件函数]               | 获取该条件下的nodename节点                |
    | //nodename[n]                      | 获取第n个nodename节点                     |
    | //nodename[last()]                 | 获取最后一个nodename节点                  |
    | //nodename[position()<3]           | 获取前两个nodename节点                    |
    | //nodename[@attr]                  | 获取包含attr属性的nodename节点            |
    | //nodename[@attr='attr1']          | 获取包含attr='attr'的nodename节点         |
    | //nodename[childname='childname1'] | 获取子节点名字='childname1'的nodename节点 |

- 函数

  - | 函数                      | 含义                             |
    | ------------------------- | -------------------------------- |
    | text()                    | 获取该节点的值                   |
    | contains(@class,'class1') | 包含,class属性中包含class1的节点 |
    | last()                    | 结果节点集的左后一个节点索引     |
    | position()                | 当前节点的索引                   |

  轴

  - | 轴名称             | 结果                                                     |
    | :----------------- | :------------------------------------------------------- |
    | ancestor           | 选取当前节点的所有先辈（父、祖父等）。                   |
    | ancestor-or-self   | 选取当前节点的所有先辈（父、祖父等）以及当前节点本身。   |
    | attribute          | 选取当前节点的所有属性。                                 |
    | child              | 选取当前节点的所有子元素。                               |
    | descendant         | 选取当前节点的所有后代元素（子、孙等）。                 |
    | descendant-or-self | 选取当前节点的所有后代元素（子、孙等）以及当前节点本身。 |
    | following          | 选取文档中当前节点的结束标签之后的所有节点。             |
    | namespace          | 选取当前节点的所有命名空间节点。                         |
    | parent             | 选取当前节点的父节点。                                   |
    | preceding          | 选取文档中当前节点的开始标签之前的所有节点。             |
    | preceding-sibling  | 选取当前节点之前的所有同级节点。                         |
    | self               | 选取当前节点。                                           |

- 运算符

  - | 运算符 | 描述           | 实例                          | 返回值                             |
    | :----- | :------------- | :---------------------------- | :--------------------------------- |
    | \|     | 多路径选择     | 表达式1\|表达式2\|表达式3     | 返回所有表达式的并集               |
    | +      | 加法           | //book[price=15+15]           | 选取price=15+15的book节点          |
    | -      | 减法           | //book[price=20-1]            | 选取price=20-1的book节点           |
    | *      | 乘法           | //book[price=5*6]             | 选取price=5*6的book节点            |
    | div    | 除法           | //book[price=18*19 div 18]    | 选取price=18*19/18的book节点       |
    | =      | 等于           | //book[price=18]              | 选取price=18的book节点             |
    | !=     | 不等于         | //book[price!=18]             | 选取price!=18*的book节点           |
    | <      | 小于           | //book[price<18]              | 选取price<18的book节点             |
    | <=     | 小于或等于     | //book[price<=18]             | 选取price<=18的book节点            |
    | >      | 大于           | //book[price>18]              | 选取price>18的book节点             |
    | >=     | 大于或等于     | //book[price>=18]             | 选取price>=18的book节点            |
    | or     | 或             | //book[price=18 or price=19]  | 选取price=18或者price=19的book节点 |
    | and    | 与             | //book[price>18 and price<30] | 选取price>18并且price<30的book节点 |
    | mod    | 计算除法的余数 | //book[price mod 2=0]         | 选取price为偶数的book节点          |

- Jsoup中使用

  - 引入maven坐标

  - ```xml
    <dependency>
        <groupId>org.jsoup</groupId>
        <artifactId>jsoup</artifactId>
        <version>1.15.3</version>
    </dependency>
    ```

  - ```java
    // 读取本地文件
    // Document document = Jsoup.parse(new File("C:\\Users\\77023\\Desktop\\a.html"));
    // 读取在线网页
    Document document = Jsoup.connect("http://www.tl.changyou.com/data/mission/kuang1.shtml").get();
    // 获取xpath匹配元素集
    Elements stones = document.selectXpath("//table[@class=\"center table_news\"]");
    for (int i = 0; i < stones.size(); i++) {
        //提取图片标签
        Elements urlE = stones.get(i).selectXpath("//table[@class=\"center table_news\"]//tr[1]//img");
        //获取图片src属性
        String src = urlE.get(i).attr("src");
        //实际业务需求适应
        if (src.split("/").length==2){
            src="http://www.tl.changyou.com/data/mission/"+urlE.get(i).attr("src");
        }
        System.out.println("URL:"+src);
        //提取元素
        Elements descriptionE = stones.get(i).selectXpath("//table[@class=\"center table_news\"]//tr[2]/td");
        //获取文本
        System.out.println("Description:"+descriptionE.get(i).text());
        //提取元素
        Elements titleE = stones.get(i).selectXpath("//table[@class=\"center table_news\"]//th");
        //获取文本
        System.out.println("Title:"+titleE.get(i).text());
        System.out.println();
    }
    ```

  - 


