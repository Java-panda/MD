

### LWC

1. HelloWorld

   1. 入门案例

      1. 默认VS Code环境已经配置完成

      2. SFDX: Create Project 创建一个空项目

      3. SFDX: Authorize an Org 建立SF平台授权关系

      4. SFDX: Create Lightning Web Component 创建一个LWC组件

      5. 目录结构如下

         1. 组件名.html

            ```html
            <template>
                <div>这是我的第一个LWC组件</div>
            </template>
            ```

         2. 组件名.js

            ```js
            //lwc模块引入LightningElement类
            import { LightningElement } from 'lwc';
            
            //创建同名class继承LightningElement
            export default class 组件名 extends LightningElement {}
            ```

         3. 组件名.js-meta.xml

            ```xml
            <?xml version="1.0" encoding="UTF-8"?>
            <LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
                <apiVersion>52.0</apiVersion>
                <isExposed>true</isExposed>
                <targets>
                    <target>lightning__AppPage</target>
                    <target>lightning__HomePage</target>
                    <target>lightning__RecordPage</target>
                </targets>
            </LightningComponentBundle>
            ```

         4. SFDX: Deploy Source to Org 部署到SF

         5. targets中的任意画面中Edit Page能够发现自定义组件

         6. 插入组件,保存,激活

      6. 注意事项

         1. 组件名和html标签元素为驼峰映射,如appTest对应<c-app-test>

2. 工欲善其事必先利其器

   1. [LWC官方文档](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/)
   2. [LWC代码示例](https://github.com/trailheadapps/lwc-recipes/tree/main/force-app/main/default/lwc)
   3. [在线开发工具](https://webcomponents.dev/)
   4. [Base Component](https://developer.salesforce.com/docs/component-library/overview/components)
   5. [Base Component源码](https://github.com/salesforce/base-components-recipes/tree/master/force-app/main/default/lwc)
   6. [SalesForce Lighting Design System](https://www.lightningdesignsystem.com/)

3. LWC基本构成

   1. ```asciidoc
      myComponent
         ├──myComponent.html
         ├──myComponent.js
         ├──myComponent.js-meta.xml
         ├──myComponent.css
         ├──myComponent.svg
         ├──utils.js
         └──__tests__
              └──myComponent.test.js
      ```

4. HTM

   1. 数据绑定

      ```vue
      <!-- html -->
      <template>
          <div>属性:{pro}</div>
          <div>方法:{pro}</div>
      </template>
      
      <!-- js -->
      pro = "属性值";
      fun(){
      	...
      };
      ```

   2. 条件渲染

      ```vue
      <!-- html -->
      <template if:true={t}>
          结果为真则渲染
      </template>
      
      <template if:false={f}>
          结果为假则渲染
      </template>
      
      <!-- js -->
      t = true;
      f = false;
      ```

   3. 循环迭代

      ```vue
      <!-- html -->
      <!-- for each -->
      <template for:each={perons} for:item="person" for:index="i">
      	<li key={person.name}>
          	<div>{i},{person.name},{person.age},{person.gender}</div>
          </li>
      </template>
      
      <!-- iterator -->
      <template iterator:it={persons}>
      	<li key={it.index}>
          	<div>{it.value.name},{it.value.age},{it.value.gender}</div>
          </li>
      </template>
      
      <!-- js -->
      perons=[
          {
              name:"张三",
              age:25,
              gender:true
          },
          {
              name:"李四",
              age:26,
              gender:false
          },
          {
              name:"王五",
              age:27,
              gender:true
          }
      ];
      ```

5. CSS

   1. 基本概念

      1. 设计模式
         1. Responsive:响应式
         2. Adaptive:自适应式

   2. SLDS

      1. Base Lightning Components

         1. Variations:通过修改设定的变量得到不同的样式效果

      2. Utility Classes

      3. Lightning Design System Blueprints

      4. Styling Hooks

         1. 自定义单体class

            ```css
            .my-brand {
              --slds-c-button-brand-color-background: purple;
            }
            
            h1 {
                font-size: large;
            }
            ```

         2. 自定义群体class

            ```css
            /* host限定在该组件内有效 */
            :host {
              --slds-c-button-brand-color-background: purple;
              --slds-c-button-brand-color-border: purple;
            }
            
            :host(.active) {
                background-color: lightgreen;
            } 
            ```

         3. 声明自定义变量/调用变量

            ```css
            :host {
                --important-color: red;
            }
            
            .important {
                color: var(--important-color);
            }
            ```

         4. 注意事项

            ```css
            /* 不要重写SLDS现存的class */ 
            .slds-button_brand {
                background-color: purple;
            }
            ```

      5. Custom CSS Classes

      6. 共享样式

         1. 创建一个只含有css和xml的组件

            ```
            cssLibrary
               ├──cssLibrary.css
               └──cssLibrary.js-meta.xml
            ```

         2. 在其他组件中引用该css

            ```css
            @import 'c/cssLibrary';
            ```

      7. 选择器

         | 选择器                                                       | 例子                  | 例子描述                                             |
         | :----------------------------------------------------------- | :-------------------- | :--------------------------------------------------- |
         | [.*class*](https://www.w3school.com.cn/cssref/selector_class.asp) | .intro                | 选择 class="intro" 的所有元素。                      |
         | .*class1*.*class2*                                           | .name1.name2          | 选择 class 属性中同时有 name1 和 name2 的所有元素。  |
         | .*class1* .*class2*                                          | .name1 .name2         | 选择作为类名 name1 元素后代的所有类名 name2 元素。   |
         | [#*id*](https://www.w3school.com.cn/cssref/selector_id.asp)  | #firstname            | 选择 id="firstname" 的元素。                         |
         | [*](https://www.w3school.com.cn/cssref/selector_all.asp)     | *                     | 选择所有元素。                                       |
         | [*element*](https://www.w3school.com.cn/cssref/selector_element.asp) | p                     | 选择所有 <p> 元素。                                  |
         | [*element*.*class*](https://www.w3school.com.cn/cssref/selector_element_class.asp) | p.intro               | 选择 class="intro" 的所有 <p> 元素。                 |
         | [*element*,*element*](https://www.w3school.com.cn/cssref/selector_element_comma.asp) | div, p                | 选择所有 <div> 元素和所有 <p> 元素。                 |
         | [*element* *element*](https://www.w3school.com.cn/cssref/selector_element_element.asp) | div p                 | 选择 <div> 元素内的所有 <p> 元素。                   |
         | [*element*>*element*](https://www.w3school.com.cn/cssref/selector_element_gt.asp) | div > p               | 选择父元素是 <div> 的所有 <p> 元素。                 |
         | [*element*+*element*](https://www.w3school.com.cn/cssref/selector_element_plus.asp) | div + p               | 选择紧跟 <div> 元素的首个 <p> 元素。                 |
         | [*element1*~*element2*](https://www.w3school.com.cn/cssref/selector_gen_sibling.asp) | p ~ ul                | 选择前面有 <p> 元素的每个 <ul> 元素。                |
         | [[*attribute*\]](https://www.w3school.com.cn/cssref/selector_attribute.asp) | [target]              | 选择带有 target 属性的所有元素。                     |
         | [[*attribute*=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value.asp) | [target=_blank]       | 选择带有 target="_blank" 属性的所有元素。            |
         | [[*attribute*~=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_contain.asp) | [title~=flower]       | 选择 title 属性包含单词 "flower" 的所有元素。        |
         | [[*attribute*\|=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_start.asp) | [lang\|=en]           | 选择 lang 属性值以 "en" 开头的所有元素。             |
         | [[*attribute*^=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_begin.asp) | a[href^="https"]      | 选择其 src 属性值以 "https" 开头的每个 <a> 元素。    |
         | [[*attribute*$=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_end.asp) | a[href$=".pdf"]       | 选择其 src 属性以 ".pdf" 结尾的所有 <a> 元素。       |
         | [[*attribute**=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_contain.asp) | a[href*="w3schools"]  | 选择其 href 属性值中包含 "abc" 子串的每个 <a> 元素。 |
         | [:active](https://www.w3school.com.cn/cssref/selector_active.asp) | a:active              | 选择活动链接。                                       |
         | [::after](https://www.w3school.com.cn/cssref/selector_after.asp) | p::after              | 在每个 <p> 的内容之后插入内容。                      |
         | [::before](https://www.w3school.com.cn/cssref/selector_before.asp) | p::before             | 在每个 <p> 的内容之前插入内容。                      |
         | [:checked](https://www.w3school.com.cn/cssref/selector_checked.asp) | input:checked         | 选择每个被选中的 <input> 元素。                      |
         | [:default](https://www.w3school.com.cn/cssref/selector_default.asp) | input:default         | 选择默认的 <input> 元素。                            |
         | [:disabled](https://www.w3school.com.cn/cssref/selector_disabled.asp) | input:disabled        | 选择每个被禁用的 <input> 元素。                      |
         | [:empty](https://www.w3school.com.cn/cssref/selector_empty.asp) | p:empty               | 选择没有子元素的每个 <p> 元素（包括文本节点）。      |
         | [:enabled](https://www.w3school.com.cn/cssref/selector_enabled.asp) | input:enabled         | 选择每个启用的 <input> 元素。                        |
         | [:first-child](https://www.w3school.com.cn/cssref/selector_first-child.asp) | p:first-child         | 选择属于父元素的第一个子元素的每个 <p> 元素。        |
         | [::first-letter](https://www.w3school.com.cn/cssref/selector_first-letter.asp) | p::first-letter       | 选择每个 <p> 元素的首字母。                          |
         | [::first-line](https://www.w3school.com.cn/cssref/selector_first-line.asp) | p::first-line         | 选择每个 <p> 元素的首行。                            |
         | [:first-of-type](https://www.w3school.com.cn/cssref/selector_first-of-type.asp) | p:first-of-type       | 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。     |
         | [:focus](https://www.w3school.com.cn/cssref/selector_focus.asp) | input:focus           | 选择获得焦点的 input 元素。                          |
         | [:fullscreen](https://www.w3school.com.cn/cssref/selector_fullscreen.asp) | :fullscreen           | 选择处于全屏模式的元素。                             |
         | [:hover](https://www.w3school.com.cn/cssref/selector_hover.asp) | a:hover               | 选择鼠标指针位于其上的链接。                         |
         | [:in-range](https://www.w3school.com.cn/cssref/selector_in-range.asp) | input:in-range        | 选择其值在指定范围内的 input 元素。                  |
         | [:indeterminate](https://www.w3school.com.cn/cssref/selector_indeterminate.asp) | input:indeterminate   | 选择处于不确定状态的 input 元素。                    |
         | [:invalid](https://www.w3school.com.cn/cssref/selector_invalid.asp) | input:invalid         | 选择具有无效值的所有 input 元素。                    |
         | [:lang(*language*)](https://www.w3school.com.cn/cssref/selector_lang.asp) | p:lang(it)            | 选择 lang 属性等于 "it"（意大利）的每个 <p> 元素。   |
         | [:last-child](https://www.w3school.com.cn/cssref/selector_last-child.asp) | p:last-child          | 选择属于其父元素最后一个子元素每个 <p> 元素。        |
         | [:last-of-type](https://www.w3school.com.cn/cssref/selector_last-of-type.asp) | p:last-of-type        | 选择属于其父元素的最后 <p> 元素的每个 <p> 元素。     |
         | [:link](https://www.w3school.com.cn/cssref/selector_link.asp) | a:link                | 选择所有未访问过的链接。                             |
         | [:not(*selector*)](https://www.w3school.com.cn/cssref/selector_not.asp) | :not(p)               | 选择非 <p> 元素的每个元素。                          |
         | [:nth-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-child.asp) | p:nth-child(2)        | 选择属于其父元素的第二个子元素的每个 <p> 元素。      |
         | [:nth-last-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-child.asp) | p:nth-last-child(2)   | 同上，从最后一个子元素开始计数。                     |
         | [:nth-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-of-type.asp) | p:nth-of-type(2)      | 选择属于其父元素第二个 <p> 元素的每个 <p> 元素。     |
         | [:nth-last-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-of-type.asp) | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。                 |
         | [:only-of-type](https://www.w3school.com.cn/cssref/selector_only-of-type.asp) | p:only-of-type        | 选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。     |
         | [:only-child](https://www.w3school.com.cn/cssref/selector_only-child.asp) | p:only-child          | 选择属于其父元素的唯一子元素的每个 <p> 元素。        |
         | [:optional](https://www.w3school.com.cn/cssref/selector_optional.asp) | input:optional        | 选择不带 "required" 属性的 input 元素。              |
         | [:out-of-range](https://www.w3school.com.cn/cssref/selector_out-of-range.asp) | input:out-of-range    | 选择值超出指定范围的 input 元素。                    |
         | [::placeholder](https://www.w3school.com.cn/cssref/selector_placeholder.asp) | input::placeholder    | 选择已规定 "placeholder" 属性的 input 元素。         |
         | [:read-only](https://www.w3school.com.cn/cssref/selector_read-only.asp) | input:read-only       | 选择已规定 "readonly" 属性的 input 元素。            |
         | [:read-write](https://www.w3school.com.cn/cssref/selector_read-write.asp) | input:read-write      | 选择未规定 "readonly" 属性的 input 元素。            |
         | [:required](https://www.w3school.com.cn/cssref/selector_required.asp) | input:required        | 选择已规定 "required" 属性的 input 元素。            |
         | [:root](https://www.w3school.com.cn/cssref/selector_root.asp) | :root                 | 选择文档的根元素。                                   |
         | [::selection](https://www.w3school.com.cn/cssref/selector_selection.asp) | ::selection           | 选择用户已选取的元素部分。                           |
         | [:target](https://www.w3school.com.cn/cssref/selector_target.asp) | #news:target          | 选择当前活动的 #news 元素。                          |
         | [:valid](https://www.w3school.com.cn/cssref/selector_valid.asp) | input:valid           | 选择带有有效值的所有 input 元素。                    |
         | [:visited](https://www.w3school.com.cn/cssref/selector_visited.asp) | a:visited             | 选择所有已访问的链接。                               |

6. JS

   1. js中实现异步的三种方式
      1. CallBack
      2. Promise
      3. Async+Await

7. 事件交互

8. 访问SalesForce Resources

9. SalesForce数据库交互

   1. Base Components

      1. 特性

         | FEATURE                         | `LIGHTNING-RECORD-FORM`                                      | `LIGHTNING-RECORD-VIEW-FORM`                                 | `LIGHTNING-RECORD-EDIT-FORM`                                 |
         | :------------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
         | Create Records                  | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |                                                              | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |
         | Edit Records                    | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |                                                              | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |
         | View Records                    | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |                                                              |
         | Read-Only Mode                  | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |                                                              |
         | Layout Types                    | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |                                                              |                                                              |
         | Multi Column Layout             | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |
         | Custom Layout for Fields        |                                                              | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |
         | Custom Rendering of Record Data |                                                              | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) | ![Yes](https://resources.docs.salesforce.com/images/e74d957594bda6387b19f8f62f8907dc.png) |

      2. lightning-record-edit-form

      3. lightning-record-view-form

      4. lightning-record-form

         1. ```vue
            <lightning-record-form
                record-id="ID"
                object-api-name="Object"
                fields=[fields]>
            </lightning-record-form>
            ```

   2. Lightning Data Service

      1. 对象及字段的动态获取

         用途:避免对象或字段名称修改,出现匹配失败,动态获取类比于反射

         ```js
         标准对象
         import standardObject from '@salesforce/schema/StandardObject';
         定制对象
         import customObject from '@salesforce/schema/CustomObject__c';
         标准字段
         import STANDARD_OBJECT_FIELD_NAME from '@salesforce/schema/StandardObject.fieldName';
         定制字段
         import CUSTOM_OBJECT_FIELD_NAME from '@salesforce/schema/CustomObject__c.fieldName__c';
         ```

      2. API模块

         1. lightning/ui*Api

            1. lightning/uiRecordApi

               1. 适配器名

                  1. getRecord

               2. 参数配置

                  1. {recordId: '$recordId', fields: 字段集合}
                     1. 注
                        1. $作为变量最外层时代表该变量为响应式,一旦监控到发生变化,则wire会重新渲染,非最外层例如数组的元素的时候则变成字面量
                           1. $只作用于
                              1. 私有属性
                              2. 具有getter/setter的属性
                              3. 具有@api的属性
                        2. 字段集合:[ObjectName.FieldName,...]

               3. 返回值

                  ```js
                  {
                      data:{fields:{
                          FieldName:{value:值}
                      }},
                      error:Error
                  }
                  ```

      3. 使用

         1. ```js
            import { wire } from 'lwc';
            import { 适配器名 } from 'API模块';
            @wire(适配器名, 参数配置)
            变量名;
            ```

   3. Apex

      1. 创建LWC可调用Apex

         ```java
         public with sharing class GetContact {
             public GetContact() {
             }
         
             //1.特有注解,cacheable=true允许客户端缓存,只有查询方法可以加缓存,且通过wire获取的数据必须要开启缓存模式
             @AuraEnabled(cacheable=true)
             //2.public/global
             //3.statuc
             public static Contact getContactById(Id contactId){
                 return [select Id,Name,Title,Email,Phone from Contact where Id=:contactId];
             }
         }
         
         ```

      2. LWC导入Apex

         ```js
         import 方法名 from '@salesforce/apex/命名空间.类名.方法名';
         import getContactById from '@salesforce/apex/GetContact.getContactById';
         ```

      3. LWC调用Apex

         1. wire绑定调用

            ```js
            @api
            recordId="0035h00000L8Y5CAAV";
            
            //调用
            @wire(getContactById,{contactId:"$recordId"})
            apexContact
            ```

         2. 直接调用

            ```js
            import { api, LightningElement, track, wire } from 'lwc';
            import getContactById from '@salesforce/apex/GetContact.getContactById';
            
            searchId='';
            handleSearchId(event){
                this.searchId=event.target.value;
            }
            @track
            clickContact;
            
            handleClick(){
                getContactById({ contactId: this.searchId})
                    .then(ret=>{
                    this.clickContact=ret;
                })
                    .catch(error=>{
                    console.log("error"+JSON.stringify(error));
                });
                console.log("clickContact"+JSON.stringify(this.clickContact));
            }
            ```

      4. 更新缓存

         1. Wire绑定

            ```js
            import { refreshApex } from '@salesforce/apex';
            
            @wire(getContactById,{contactId:"$recordId"})
            getApexContact(data,error){
                if(data){
                    //数据
                    this.apexContact=data;
                }else{
                    console.log(error);
                }
            }
            handler() {
              //更新数据缓存
              refreshApex(this.apexContact);
            }
            ```

         2. 直接Apex

            ```js
            import { LightningElement, wire } from 'lwc';
            import { getRecord, getRecordNotifyChange } from 'lightning/uiRecordApi';
            
            async handler() {
                // 某个更新方法
                await updateMethod(this.recordId);
                // 手动更新回最新状态
                getRecordNotifyChange([{recordId: this.recordId}]);
            }
            ```

      5. 安全访问声明

         1. Record级别
            1. with sharing
               1. 强制当前用户共享规则,记录级别控制
            2. without sharing
               1. 非强制当前用户共享规则,记录级别控制
            3. inherited sharing
               1. 运行时动态控制共享规则,由调用该类的调用者类共享模式决定,调用者为with sharing时则为with sharing为without sharing时则为without sharing
         2. Object/Field级别
            1. SOQL句尾加上WITH SECURITY_ENFORCED

10. LWC在SalesForce中的各种使用

    1. 存储场所
       1. Setup→Home→Lightning Components

11. Debug

12. Test

    1. 学习Jest

       [Jest](https://jestjs.io/zh-Hans/docs/getting-started)

    2. 环境搭建

       1. 安装Jest

          ```bash
          npm install @salesforce/sfdx-lwc-jest --save-dev
          
          可能出现的安装问题
          出现问题:npm ERR! notarget No matching version found for prettier-plugin-apex@^1.10.1.
          解决方法:替换package.json中的"prettier-plugin-apex": "^1.10.1"为"prettier-plugin-apex": "^1.10.0"
          ```

    3. 代码

       1. 结构

          1. ```
             __test__
                ├──component.test.js
                └──data
                   └──xxx.json
             ```

       2. 模板(无数据交互)

          ```js
          // 导入相关组件库
          import { createElement } from 'lwc';
          import Hello from 'c/hello';
          
          // 描述块包含若干个test
          describe('c-hello', () => {包含若干个test
              afterEach(() => {
                  // 每一个测试Case完成后从dom中删除组件,因为每一个组件都共用同一个dom
                  while (document.body.firstChild) {
                      // 循环删除所有子节点
                      document.body.removeChild(document.body.firstChild);
                  }
              });
          
              //it/test是测试方法的标识符it(测试说明,方法)
              it('displays greeting', () => {
                  // 创建组件
                  const element = createElement('c-hello', {
                      is: Hello
                  });
                  // 组件添加到Dom
                  document.body.appendChild(element);
          
                  // querySelector取得标签元素
                  const div = element.shadowRoot.querySelector('div');
                  // 校验
                  expect(div.textContent).toBe('Hello, World!');
              });
          });
          ```

       3. 模板(有数据交互)

          ```js
          import { createElement } from 'lwc';
          import GetRecord from 'c/getRecord';
          import { getRecord} from 'lightning/uiRecordApi';
          
          describe('c-get-record', () => {
              afterEach(() => {
                  while (document.body.firstChild) {
                      document.body.removeChild(document.body.firstChild);
                  }
              });
              const mockGetRecord = require("./data/data.json");
              it('getRecord', async () => {
                  const element = createElement('c-get-record', {
                      is: GetRecord
                  });
                  document.body.appendChild(element);
                  getRecord.emit(mockGetRecord);
                  return Promise.resolve().then(() => {
                      const phoneTag = element.shadowRoot.querySelector(
                          'lightning-formatted-phone'
                      );
                      const phone = phoneTag.value;
                      expect(phone).toBe("Phone");
                  });
              });
          });
          ```

    4. 运行测试

       1. 方法一点击单个方法

          1. Run/Debug

       2. 方法二VsCode右上角运行按钮

       3. 方法三命令行

          1. ```bash
             npm run test:unit
             npm run test:unit:debug
             npm run test:unit:watch
             ```

       4. 方法四SFDX

          1. ```bash
             SFDX: Run Current Lightning Web Component Test File
             SFDX: Run All Lightning Web Component Test File
             ```

13. LWC中的各种Import合集

    1. Label:标签

       ```js
       import 标签变量名 from '@salesforce/label/命名空间.标签APIName';
       import LJ from '@salesforce/label/c.lj';
       ```

    2. Share Css Rule:CSS共享

       ```js
       @import '命名空间/Component名';
       @import 'c/cssLibrary';
       ```

    3. LDS

       ```js
       //导入API
       import { API,... } from 'API模块';
       import { getRecord,getFieldValue } from 'lightning/uiRecordApi';
       
       //导入对象API
       import 对象ApiName from '@salesforce/schema/对象ApiName';
       import customObject from '@salesforce/schema/CustomObject__c';
       
       //导入字段API
       import 字段ApiName from '@salesforce/schema/对象ApiName.字段ApiName';
       import CUSTOM_OBJECT_FIELD_NAME from '@salesforce/schema/CustomObject__c.fieldName__c';
       ```

    4. Apex

       ```js
       import 方法名 from '@salesforce/apex/命名空间.类名.方法名';
       
       import getContactById from '@salesforce/apex/GetContact.getContactById';//调用自定义的class
       import { getSObjectValue } from '@salesforce/apex';//getSObjectValue(Sobject,FieldName)类似于getFieldValue,用语Apex返回的Sobjects
       ```

    5. 