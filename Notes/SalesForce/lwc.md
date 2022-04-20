

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