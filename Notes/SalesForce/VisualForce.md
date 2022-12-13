[VisualForce](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages )

画面框架,类似于JSP,基于MVC

1. 入门案例

   1. 打开开发模式便于在线调试(非必须):My Personal Information->Advanced User Details->Development Mode->Show View State in Development

   2. 默认VS Code环境已经配置完成

   3. SFDX: Create Project 创建一个空项目

   4. SFDX: Authorize an Org 建立SF平台授权关系

   5. SFDX: Create VisualForce Page创建一个VF页面

   6. 目录结构如下

      1. 页面名.page

         ```html
         <apex:page>
             <h1>Congratulations</h1>
             This is your new Page
         </apex:page>
         ```

      2. 页面名.page-meta.xml

         ```xml
         <?xml version="1.0" encoding="UTF-8"?>
         <ApexPage xmlns="http://soap.sforce.com/2006/04/metadata"> 
             <apiVersion>52.0</apiVersion>
             <label>页面名</label>
         </ApexPage>
         ```

   7. SFDX: Deploy Source to Org 部署到SF

   8. 访问https://当前实例地址/apex/页面名

2. 运行原理

   ![A diagram indicating the way a Visualforce page runs on the platform](https://developer.salesforce.com/docs/resources/img/en-us/234.0?doc_id=dev_guides%2Fpages%2Fimages%2Fpages_intro_dev_architecture.jpg&folder=pages)

   1. 开发人员编写VisualForce源代码保存到云平台
   2. 平台编译为抽象指令集保存其元数据到元数据仓库
   3. VisualForce页面渲染器通过解析渲染元数据展示画面内容

3. 核心构成(MVC)

   1. V:Page
   2. MC:Controller
      1. Standard Controller
         1. 单对象标准控制器提供CRUD
      2. Standard List Controller
         1. 多对象标准控制器提供CRUD
      3. Custom Controller
         1. 完全自定义所有功能
      4. Controller Extension
         1. 扩展标准控制器,相当于对标准控制器的加强
      5. 注意事项
         1. 标准控制器是User Mode执行
         2. 自定义控制器是System Mode执行也就是忽略Profile,自行编码判断Profile进行权限控制

4. 核心知识要点

   1. [标签](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_compref.htm)

      1. 原生html标签

      2. apex标签

         1. page:根标签

            ```html
            <!-- 基本 -->
            <apex:page>
            	根标签,类似于html中的<html>
            </apex:page>
                
            <!-- 属性 -->
            <!-- 标准控制器,引入标准对象,提供标准的CRUD操作 -->
            standardController="标准控制器"
            ```

         2. pageBlock/pageBlockSection/pageBlockButtons

            pageBlock:定义一个区块

            pageBlockSection:定义一个下拉折叠

            pageBlockButtons:定义一个按钮区

            ```html
            <apex:form> 
                <apex:pageBlock title="1.页面区块标签">
                    <!-- 折叠区域 -->
                    <apex:pageBlockSection columns="1" title="1.1折叠区域">
                        <apex:inputField value="{!Contact.FirstName}"></apex:inputField>
                        <apex:inputField value="{!Contact.LastName}"></apex:inputField>
                        <apex:inputField value="{!Contact.Email}"></apex:inputField>
                        <apex:inputField value="{!Contact.Birthdate}"></apex:inputField>
                    </apex:pageBlockSection>
                    <!-- 按钮    -->
                    <apex:pageBlockButtons>
                        <apex:commandButton action="{!save}" value="保存">
                        </apex:commandButton>
                    </apex:pageBlockButtons> 
                </apex:pageBlock>
            </apex:form>
            ```

         3. pageBlockTable

            pageBlockTable:迭代器

            ```html
            <apex:pageBlock title="pageBlockTable">
                <apex:pageBlockSection columns="1" title="展开/折叠">
                    <apex:pageBlockTable value="{!Contact.Cases}" var="case">
                        <apex:column title="操作">
                            <apex:outputLink value="{!URLFOR ($Action.Case.Edit,Case.Id)}">
                                Edit
                            </apex:outputLink>
                            &nbsp;
                            <apex:outputLink value="{!URLFOR ($Action.Case.Delete,Case.Id)}">
                                Del
                            </apex:outputLink>
                        </apex:column>
                        <apex:column value="{!case.Subject}"></apex:column>
                        <apex:column value="{!case.Priority}"></apex:column>
                        <apex:column value="{!case.Status}"></apex:column>
                    </apex:pageBlockTable>
                </apex:pageBlockSection>
            </apex:pageBlock>=
            ```

         4. details/relatedList

            details:显示该条记录

            relatedList:显示相关记录

            ```html
            <apex:pageBlock title="details/relatedList">
                <apex:pageBlockSection columns="1" title="展开/折叠">
                    <!-- 默认为true展示所有相关记录.设为false则可通过relatedList显示部分相关记录 -->
                    <apex:detail relatedList="false">
                        <!-- 定义想要展示的相关记录 -->
                        <apex:relatedList list="Cases"></apex:relatedList>
                    </apex:detail>
                </apex:pageBlockSection>
            </apex:pageBlock>
            ```

         5. inputField/pageMessages

            inputField:输入框

            pageMessages:消息提示

            ```html
            <apex:form> 
                <apex:pageBlock title="inputField/pageMessages">
                    <!-- 消息提示 -->
                    <apex:pageMessages></apex:pageMessages>
                    <!-- 输入框 -->
                    <apex:pageBlockSection columns="1" title="展开/折叠">
                        <apex:inputField value="{!Contact.FirstName}"></apex:inputField>
                        <apex:inputField value="{!Contact.LastName}"></apex:inputField>
                        <apex:inputField value="{!Contact.Email}"></apex:inputField>
                        <apex:inputField value="{!Contact.Birthdate}"></apex:inputField>
                    </apex:pageBlockSection>
                    <!-- 按钮    -->
                    <apex:pageBlockButtons>
                        <apex:commandButton action="{!save}" value="保存"></apex:commandButton>
                    </apex:pageBlockButtons> 
                </apex:pageBlock>
            </apex:form>
            ```

         6. outputField

            outputField:输出

            ```html
            <apex:pageBlock title="outputField">
                <apex:pageBlockSection columns="1" title="展开/折叠">
                    <apex:outputField value="{!Contact.FirstName}"></apex:outputField>
                    <apex:outputField value="{!Contact.LastName}"></apex:outputField>
                    <apex:outputField value="{!Contact.Email}"></apex:outputField>
                    <apex:outputField value="{!Contact.Birthdate}"></apex:outputField>
                </apex:pageBlockSection>
            </apex:pageBlock>
            ```

         7. image

            image:图片

            ```html
            <apex:pageBlock title="image">
                <apex:pageBlockSection columns="1" title="展开/折叠">
                    <apex:image url="https://developer.salesforce.com/files/salesforce-developer-network-logo.png"></apex:image>
                </apex:pageBlockSection>
            </apex:pageBlock>
            ```

         8. includeScript/stylesheet

            includeScript:引入JS静态资源

            stylesheet:引入CSS静态资源

            ```html
            <!-- 预先上传静态资源 -->
            <!-- 引入js -->
            <apex:includeScript value="{! $Resource.Jquery }"/>
            <!-- 从压缩文件引入js -->
            <apex:includeScript value="{! URLFOR($Resource.MyZip,'jquery-3.6.0.min.js') }"/>
            <!-- 引入css -->
            <apex:stylesheet value="{! $Resource.MyCss }" />
            <!-- 从压缩文件引入css -->
            <apex:stylesheet value="{! URLFOR($Resource.MyZip,'MyCss.css') }" />
            <!-- 使用引入的js -->
            <script type="text/javascript">
                $(document).ready(function() {
                    $("#div").html("静态JS的引用");
                });
            </script>
            <apex:pageBlock title="includeScript">
                <apex:pageBlockSection columns="1" title="展开/折叠">
                    <div id="div"></div>
                </apex:pageBlockSection>
            </apex:pageBlock>
            ```

   2. 表达式

      1. {!表达式}

         ```html
         <apex:page standardController="Account">
             <!-- 访问页面时加上?id=AccountId即可显示该Account的名称-->
             {!Account.Name}
         </apex:page>
         ```

      2. [函数](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_variables_functions.htm)

         ```html
         TODAY()
         MONTH()
         DAY()
         YEAR()
         MAX/MIN(1,2,3)
         SQRT(100)
         <!-- 判断父中是否包含子-->
         CONTAINS('父', '子')
         <!-- 三元表达式-->
         IF(Boolean,True,False)
         <!-- 跳转路径-->
         URLFOR(URL,参数)
         ```

      3. [全局变量](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_variables_global.htm)

         ```html
         <apex:page>
             {!$User.FirstName}
         </apex:page>
         ```

         | 常用全局变量表达式                  | 含义         |
         | ----------------------------------- | ------------ |
         | $Action.对象名.Edit/Delete          | 定义一个行为 |
         | $Api                                | 尚未学习     |
         | $Asset                              | 尚未学习     |
         | $Cache.Org                          | 尚未学习     |
         | $Cache.Session                      | 尚未学习     |
         | $Component                          | 尚未学习     |
         | $ComponentLabel                     | 尚未学习     |
         | $CurrentPage                        | 尚未学习     |
         | $FieldSet                           | 尚未学习     |
         | $Label                              | 尚未学习     |
         | $Label.Site                         | 尚未学习     |
         | $MessageChannel                     | 尚未学习     |
         | $Network                            | 尚未学习     |
         | $ObjectType                         | 尚未学习     |
         | $Organization                       | 尚未学习     |
         | $Page                               | 尚未学习     |
         | $Permission                         | 尚未学习     |
         | $Profile                            | 尚未学习     |
         | $Resource.静态资源API名             | 静态资源引用 |
         | $SControl                           | 尚未学习     |
         | $Setup                              | 尚未学习     |
         | $Site                               | 尚未学习     |
         | $System.OriginDateTime              | 尚未学习     |
         | $User.用户属性                      | 用户信息获取 |
         | $User.UITheme/User.UIThemeDisplayed | 尚未学习     |
         | $UserRole                           | 尚未学习     |
   
5. 限制相关

   1. 一个Page最大170KB
      1. 再现方式
         1. 大量写入标签
      2. 错误现象
         1. ![image-20221215203546266](C:\Users\77023\Desktop\MD笔记\MD\Notes\SalesForce\images\image-20221215203546266.png)