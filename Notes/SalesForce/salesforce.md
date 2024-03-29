SF笔记

1. 入门概念

   1. 名词解释
      1. Iaas(Infrastructure as a service)基础即服务
      2. Paas(Platform as a service)平台即服务
      3. Saas(Softwareas a service)软件即服务
      4. Multi-Tenant(多租户架构)
      5. CRM(客户关系管理)
      6. OTB(out-of-the-box)开箱即用
      6. SOAP(Simple Object Access Protocol)
      6. WSDL(Web Service Description Language)
      6. ISV(Independent Software Vendors)独立软件开发商
      
   2. SF编程组件
      1. 自动化组件
         1. Flow
            1. Flow Builder
               1. 使用场景
                  1. Screen Flow:画面向导型
                  2. Record-Triggered Flow:记录增删改
                  3. Schedule-Triggered Flow:定时计划型
                  4. Platform Event—Triggered Flow:平台事件触发型
                  5. Autolaunched Flow (No Trigger):其他方式调用型
               2. 基本构成
                  1. Elements:基本构成要素(骨架)
                     1. Screen (画面)
                     2. Logic (逻辑)
                        1. Assignment:赋值
                        2. Decision:分支
                        3. Loop:循环
                        4. Collection Sort:排序
                        5. Collection Filter:过滤
                     3. Actions (动作)
                     4. Data(数据操作)
                        1. Create Records:增
                        2. Delete  Records:删
                        3. Update Records:改
                        4. Get  Records:查
                        5. Roll Back Records:回滚
                  2. Resources:资源(血肉)
                     1. Variable:变量
                        1. 类型
                           1. Text:字符串
                           2. Record:对象
                           3. Number:数字
                           4. Currency:货币
                           5. Boolean:布尔
                           6. Date:日期
                           7. Date/Time:日期/时间
                           8. Picklist:单选下拉列表
                           9. Multi-Select Picklist:多选下拉列表
                           10. Apex-Defined:Apex类
                        2. 注意
                           1. 勾选Allow multiple values (collection)时自动转换成List<T>
                           2. 勾选Available for input时可以作为传入参数
                           3. 勾选Available for output时可以作为返回结果
                     2. Constant:常量
                     3. Formula:表达式
                     4. Text Template:字符串模板
                     5. Choice:选择
                     6. Collection Choice Set:集合集
                     7. Record Choice Set:记录集
                     8. Picklist Choice Set:下拉列表集
                     9. Stage:阶段
               
            2. Process Builder
               1. 使用场景
                  1. 编辑现存处理型,官方已经不推荐使用
                  
               2. 基本构成
                  1. Trigger
                     1. 判断什么时候执行该Process
                        1. 记录变化
                           1. 创建时触发
                           2. 创建或者编辑时触发
                        2. 其他Process调用
                           1. 可被其他Process调用
                        3. Platform Event平台事件发生
                     
                  2. Criteria
                     1. 判断执行条件Criteria是否具备
                     
                  3. Action
                     1. 执行内容Action
                        1. 创建记录
                        
                           1. 使用流程
                              1. Action Name:动作名称
                              2. Record Type:创建对象类型
                              3. Set Field Values:字段赋值
                        
                        2. 更新记录
                        
                           1. 使用流程
                              1. Action Name:动作名称
                              2. Record:更新对象类型
                              3. Conditions:更新条件(可省略)
                              4. Field:更新字段赋值
                        
                        3. 提交批准
                        
                        4. 使用邮件模板发送邮件
                        
                           1. 使用流程
                              1. Action Name:动作名称
                              2. Email Alert:调用邮件警报
                                 1. Email Alert由调用某个邮件模板创建
                        
                        5. 发送到聊天室
                        
                           1. 使用流程
                              1. Action Name:动作名称
                              2. Post to:推送对象人
                              3. Message:推送消息
                        
                        6. 发送推送
                        
                           1. 使用流程
                              1. Action Name:动作名称
                              2. Notification Type:推送类型
                                 1. 创建Custom Notification
                              3. Notification Recipient:推送对象人
                              4. Notification Title
                              5. Notification Body
                        
                        7. 调用其他Flow
                        
                        8. 调用其他Process
                        
                           1. 使用流程
                              1. Action Name:动作名称
                              2. Process:被调用的进程名
                              3. Set Apex Variables:被调用的Process所需要的参数设置
                        
                        9. 调用Apex代码
                        
                           1. 使用流程
                        
                              1. Action Name:动作名称
                              2. Apex Class:被调用apex类名
                              3. Set Apex Variables:@InvocableMethod注解方法所需要的参数设置
                        
                           2. 注意事项
                        
                              1. 被调用的Apex必须有且仅有一个加有@InvocableMethod注解的静态static方法
                              2. 方法中的参数只能是List<T>,T只能是Sobject或者primitive类型
                        
                           3. 案例
                        
                              ```java
                              public class ProcessBuilderApexUse {
                                  public ProcessBuilderApexUse() {
                                  }
                              
                                  @InvocableMethod
                                  public static void sendEmailToExcellentPeople(List<Student__c> sc){
                                      //业务逻辑
                                      String body='您已经被录取您的成绩为:'+sc[0].Score__c;
                                      EmailManager.sendMail('liujianipanda001@gmail.com', '录取通知', body);
                                  }
                              }
                              ```
               
            3. Approval Process
               1. 使用场景
                  1. 批准流程
               2. 基本构成
                  1. Object
                     1. 批准监控对象
                  2. Email Template
                     1. 邮件模板
                  3. Criteria
                     1. 触发条件
                  4. Approver
                     1. 批准者
               
            4. WorkFlow Rules
            
               1. 使用流程
                  1. Object:选择设定对象
                  2. Condition:触发条件
                     1. 设定某些字段满足某种条件就会触发
                  3. Action:动作(添加后要Active)
                     1. Email Alerts:邮件警告
                        1. 选择邮件警告
                        2. 邮件警告选择调用邮件模板
                     2. Field Updates:字段更新
                        1. 选择更新对象
                        2. 选择更新字段
                        3. 设定字段更新表达式
                     3. Task:给指定人分配一个新任务
                        1. 选择任务对象
                        2. 指定任务接受者
                     4. Outbound Messages:外部系统消息发送
                        1. 参考https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_om_outboundmessaging.htm
                        2. 指定具有发送信息的对象
                        3. 设置基本信息
                           1. Name:WorkFlow Rules调用名
                           2. Unique Name:API名
                           3. Endpoint URL:外部送信URL
                              1. 参考https://www.asagarwal.com/how-to-send-outbound-message-from-flow-in-salesforce/?utm_source=asagarwal.com&utm_medium=blog&utm_campaign=test-outbound-message
                           4. User to send as:控制外部能否可视
                           5. Send Session ID:外部系统是否响应结果并调用SF的API
                           6. Fields To Send:发送项目
         
      2. Lightning Components
         1. LWC(Lightning Web Components)web组件模式
         2. Aura原生组件模式
         
      3. Apex
         1. 现有Flow Builder,Process Builder等组件无法完成,实现更复杂的自定义场景
         
      4. Visualforce
         1. 画面开发

   3. 环境
      1. Production (生产环境,实际用户交互访问)
      2. Developer(开发环境) 
      3. Sandbox (沙盒环境,测试环境,生产环境的副本)

2. Chatter

   1. 主要功能

      1. follow
         1. People
         2. Group
         3. Topic
         4. File
         5. Record

      2. Group
         1. Public:任何组织内得人可以加入
         2. Private
            1. Allow customers:可以邀请组织外的任何人

         3. Unlisted

3. 管理画面

   1. 权限综合

      ![ERD of user, profile, and permission objects](https://developer.salesforce.com/docs/resources/img/en-us/236.0?doc_id=dev_guides%2Fapi%2Fimages%2FSforce_profile_permission_objects.png&folder=api)

      ![ERD of User and child objects](https://developer.salesforce.com/docs/resources/img/en-us/236.0?doc_id=dev_guides%2Fapi%2Fimages%2Fuser_objects.png&folder=api)

      1. 权限级别

         1. Organization
            1. 对整个org进行权限控制
         2. Objects
            1. 对象级别控制,如对象的增删改查
         3. Fields
            1. 字段级别控制,如设置某些用户不能访问某些字段
         4. Records
            1. 记录级别控制,如设置某些用户只能读取部分数据记录
            2. 四种记录权限控制(从上到下,由严到松)
               1. Org-wide defaults
                  1. 通过设定Sharing Settings指定默认设定
               2. Role hierarchies 
                  1. 上级可查看下级
               3. Sharing rules 
                  1. 特定用户群制定共享规则查看
               4. Manual sharing
                  1. 手动分享给其他用户查看

      2. User
         1. 核心对象表User
         2. 纽带关系
            1. User.ProfileId->Profile.Id
            2. User.UserRoleId->UserRole.Id
         3. 结论
            1. 一个User对应一个Profile
            2. 一个User可以对应多个Permission Set

      3. Profile
         1. 核心对象表Profile

         2. 纽带关系
            1. Profile.UserLicenseId->UserLicense.Id
            
         3. 核心设置

            1. Apps

               1. Assigned Apps

               2. Assigned Connected Apps

               3. Object Settings

                  1. Tab Settings

                     1. Default On
                        1. 默认打开,tab默认设置显示
                     2. Default Off
                        1. 默认关闭,tab默认设置不显示,但是可以在全部表示栏里面找到
                     3. Tab Hidden
                        1. 彻底关闭隐藏该Tab使其不可用

                  2. Record Types/Page Layout

                     1. 字段类型
                        1. 为对象创建不同的Record Type,控制不同的list值显示值
                     2. 页面布局
                        1. 可以为某个Profile的某个对象的页面布局设置显示内容,从而达到对不同User的可视权控制

                  3. Object Permissions:表级别增删改查权限控制

                     1. | 选项       | 含义                    |
                        | ---------- | ----------------------- |
                        | Read       | 读取                    |
                        | Create     | 创建                    |
                        | Edit       | 编辑                    |
                        | Delete     | 删除                    |
                        | View All   | 无视记录共享规则,全能读 |
                        | Modify All | 无视记录共享规则,全能写 |

                  4. Field Permissions:字段级别读写权限控制

                     1. | 选项        | 含义 |
                        | :---------- | :--- |
                        | Read Access | 读取 |
                        | Edit Access | 编辑 |

               4. App Permissions

               5. Apex Class Access

               6. Visualforce Page Access

               7. External Data Source Access

               8. Named Credential Access

               9. Data Category Visibility

               10. Flow Access

               11. Service Presence Statuses Access

               12. Custom Permissions

               13. Custom Metadata Types

               14. Custom Setting Definitions

            2. System

               1. System Permissions
               2. Login Hours
                  1. 设定特定日时来控制用户登录
               3. Login IP Ranges
                  1. 设定特定IP范围的用户来控制用户登录
               4. Service Providers
               5. Session Settings
                  1. 控制浏览器session失效时间,默认2小时未活动则失效
               6. Password Policies
                  1. 密码策略包括有效时间,历史密码记录数,密码最小长度,密码复杂度,试错次数等
               7. Default Experience

         4. 结论
            1. Profile赋予某些用户最低权限,使用Permission Set可以给特定用户群添加更加细化的特定权限

      4. License
         1. 核心对象表UserLicense
         2. 纽带关系
            1. UserLicense.Id
         3. Company Information查看License详细分配

      5. Permission Set
         1. 核心对象表
            1. PermissionSet
         2. 对Profile的增强和细化
         3. 创建Permission Set
            1. License类型
               1. NONE:通用型,根据分配的User决定LIcense类型
               2. Specific User License:仅作用于使用指定类型的License的User
               3. Specific Permission Set License:暂时不明
         4. 核心设置
            1. Apps
               1. Assigned Apps
               2. Assigned Connected Apps
               3. Object Settings
                  1. 对象的tab,record type,增删改查,字段访问权限的控制
               4. App Permissions
               5. Apex Class Access
               6. Visualforce Page Access
               7. External Data Source Access
               8. Flow Access
               9. Named Credential Access
               10. Custom Permissions
               11. Custom Metadata Types
               12. Custom Setting Definitions
            2. System
               1. System Permissions
               2. Service Providers
         5. 结论
            1. Permission Set不能删除对应User的Profile权限,只能在此Profile的基础上增加权限

      6. Permission Set Groups

         1. 创建一个组包含若干个Permission Set的并集
            1. Permission Sets in Group:子集集合
            2. Muting Permission Set in Group:被排除的权限

      7. Role
         1. 核心对象表UserRole
         2. 纽带关系
            1. UserRole.Id
         3. 公司树状层级结构图
            1. 上层默认可以查看下层记录
            2. 同级默认不可查看对方记录
         4. 核心算法
            1. 父节点ID=Null的时候本身为根节点
            2. 父节点ID!=Null的时候该节点为子节点

      8. 数据共享

         1. Org-wide defaults:通过设定Sharing Settings指定组织范围内全局设定
            1.  Default Internal Access:内部
               1. Private
               2. Public ReadOnly
               3. Public Read/Write
               4. Public Read/Write/Transfer
               5. Controllered by Parent
                  1. 主从关系的子对象的权限遵从父对象且不可编辑
            2.  Default External Access:外部
               1. Private
               2. Public ReadOnly
               3. Public Read/Write
               4. Public Read/Write/Transfer
            3.  Grant Access Using Hierarchies:Role层级
               1. 使用该模式的时候将上述两种模式改为Private
         2. Role hierarchies:角色层级
            1. 默认上级可查看下级.同级不可相互查看
         3. Sharing Settings:共享设定
            1. 设置核心要点
               1. 共有基准
                  1. 基于所有者:选择共有源User集合共享
                  2. 基于判断条件:选择某些字段满足某些条件的时候共享
               2. 共有源User集合(基于评判标准时没有此选项)
                  1. Public Groups:一群User集合
                  2. Roles:某一个职位或者级别的User集合
                  3. Roles And Internal Subordinates:某一职位或者级别及其下属下级的User集合
                  4. Roles,Internal And Portal Subordinates:整个站点所有User
               3. 共有先User集合
                  1. Public Groups:一群User集合
                  2. Roles:某一个职位或者级别的User集合
                  3. Roles And Internal Subordinates:某一职位或者级别及其下属下级的User集合
                  4. Roles,Internal And Portal Subordinates:整个站点所有User
               4. 赋予什么样的权限
                  1. 只读
                  2. 读写
         4. Manual sharing:手动共享
            1. 某个所有者手动选择某一记录共享给某个User或者User集合

      9. 总结

         1. Salesforce权限只能扩大不能缩小,所以Profile控制最小权限
         2. Profile和Permission Set能够配置的功能大体相同,Permission Set继承于Profile
         3. 对象及其字段的可见性由Profile和Permission Set设置(控制模板)
         4. 对象记录的可见性由Role和Sharing Settings以及Public Group设置(控制实例)

   2. 常用功能查找

      1. Schema Builder
         1. 对象模型构建器
      2. Lightning Components
         1. 组件库
      3. Process Builder
         1. 业务处理流程图构建器
      4. Flows
         1. 流构建器

   3. 对象管理

      1. Type:对象种类

         1. Standard objects
            1. 标准对象
         2. Custom objects
            1. 定制对象

      2. Details:对象详细

         1. 新建
            1. 对象新建后会自动创建以下几个项目
               1. Name
               2. CreatedById
               3. OwnerId
               4. LastModifiedById
         2. 删除
         3. 编辑
         4. 查询
            1. SOQL
            2. Tab画面

      3. Fields&Relationships字段类型及关系

         | 类型名                           | 含义       |
         | -------------------------------- | ---------- |
         | **Auto Number**                  | 自增       |
         | **Formula**                      | 公式       |
         | **Roll-Up Summary**              | 聚合操作   |
         | **Lookup Relationship**          | 参考关系   |
         | **Master-Detail Relationship**   | 主从关系   |
         | **External Lookup Relationship** | 外部参考   |
         | **Checkbox**                     | 布尔       |
         | **Currency**                     | 货币       |
         | **Date**                         | 日期       |
         | **Date/Time**                    | 日期/时间  |
         | **Email**                        | 邮件       |
         | **Geolocation**                  | 位置       |
         | **Number**                       | 数字       |
         | **Percent**                      | 百分比     |
         | **Phone**                        | 手机       |
         | **Picklist**                     | 单选列表   |
         | **Picklist (Multi-Select)**      | 复选列表   |
         | **Text**                         | 文本       |
         | **Text Area**                    | 短文本区块 |
         | **Text Area (Long)**             | 长文本区块 |
         | **Text Area (Rich)**             | 富文本区块 |
         | **Text (Encrypted) **            | 加密文本   |
         | **Time**                         | 时间       |
         | **URL**                          | 链接       |

         1. 聚合操作
            1. Count/Sum/Min/Max(可条件聚合,只能是主从关系中的使用,计算父对象的关联list的)
            1. 自动计算无法修改,新建或者编辑的时候并不会出现
         2. 参考关系
            1. 引用关系,互不干扰(子找父,父的关联列表会出现子)
               1. 父参照非必须
               2. 可以多层
               3. 最多参照40个
               4. 不可聚合
         3. 主从关系
            1. 从属关系,子随父变(子对象关联父对象后所有者会被删除自动变成相应的父对象)
               1. 父参照必须
               2. 只能三层
               3. 最多参照2个
               4. 可聚合
         4. 外部参考
            1. 通过创建外部数据源,参考外部对象(参考https://trailhead.salesforce.com/en/content/learn/projects/quickstart-lightning-connect/quickstart-lightning-connect2)
               1. 外部参考种类
                  1. 间接查找关系:Indirect Lookup Relationship
                     1. 子对象是外部对象，父对象是内部对象(父对象中需要有唯一的外部ID)
                  2. 外部查找关系:External Lookup Relationship
                     1. 父对象是外部对象，子对象可以是内部或外部对象

      4. Page Layouts:页面布局

         1. 修改页面字段布局
         2. 按钮
         3. 快速动作
         4. 关联列表
         5. 报表

      5. Buttons, Links, and Actions:按钮,链接,动作

         1. Buttons, Links:主要用于外部链接的访问,或频繁访问的地址

            1. 分类
               1. List button
                  1. 创建List按钮
                  2. 用途
                     1. 关联List
                        1. 关联Object的Page Layouts中的关联List里面加入按钮
                     2. 搜索主画面
                        1. 该Object的Search Layouts for Salesforce Classic中加入按钮
               2. Detail page link
                  1. 创建详细link按钮
                  2. Page Layouts中将其添加到Custom Links
                  3. 记录详细画面下方可以看到
               3. Detail page button
                  1. 创建详细button链接
                  2. Page Layouts中将其添加到Custom Buttons
                  3. 根据情况还需要加入下面两处
                     1. Quick Actions in the Salesforce Classic Publisher(Classic主题)
                     2. Salesforce Mobile and Lightning Experience Actions(Lightning Experience主题)
                  4. 记录主画面右上角的下拉列表中存在该按钮

         2. Actions

            1. 分类

               1. Object-specific对象特有Action:自动与关联对象参照关系

                  1. 类型
                     1. Create Record
                     2. Update Record
                     3. Send Email
                     4. Log Call
                     5. VisualForce
                     6. LC
                     7. LWC
                     8. Flow
                  2. 使用
                     1. 对某个Object创建一个Action
                     2. 把该Action加入到Page Layout中

               2. Global全局Action:该APP内所有人均可以点击使用的快捷Action,与其他记录不建立关系

                  1. 类型
                     1. Create Record
                     2. Send Email
                     3. Log Call
                     4. VisualForce
                     5. Canvas
                     6. LC

                  1. 使用
                     1. Global Actions:创建全局Action
                     2. Publisher Layouts:全局Action布局
                     3. 在界面右上角的＋中可以看到添加的Action

      6. Compact Layouts:设置缩略显示布局

      7. Record Types:设置不同的画面类型

      8. Search Layouts/Search Layouts for Salesforce Classic

         1. 设置搜索页面布局
         2. 编辑搜索页面按钮

      9. Triggers:添加触发器

      10. Validation Rules:校验规则

4. 特殊对象

   1. Custom Metadata Type

      1. 使用场景

         1. Mappings
         2. Business rules
         3. Master data
         4. Protected information

      2. 支持的字段类型 

         | 类型名                | 含义       |
         | --------------------- | ---------- |
         | Metadata Relationship | 元数据关系 |
         | Checkbox              | 布尔       |
         | Date                  | 日期       |
         | Date/Time             | 日期/时间  |
         | Email                 | 邮箱       |
         | Number                | 数字       |
         | Percent               | 百分比     |
         | Phone                 | 电话       |
         | Picklist              | 单选列表   |
         | Text                  | 字符串     |
         | Text Area             | 文本区     |
         | Text Area (Long)      | 长文本区   |
         | URL                   | 链接       |

      3. 创建

         1. 创建一个Custom Metadata Type
         2. 添加Custom Fields
         3. 添加Record

      4. 使用范围

         1. Apex
            1. select 标准字段,定制字段 from 类型__mdt [条件限制];
         2. Flows
            1. 使用Get Records组件查询出符合条件的Custom Metadata Type记录
         3. Default Value
            1. $CustomMetadata.类型__mdt.记录API名.字段
         4. Formula fields
            1. $CustomMetadata.类型__mdt.记录API名.字段
         5. Process Builder
            1. $CustomMetadata.类型__mdt.记录API名.字段(在ProcessBuilder中使用公式进行条件判断时,可通过系统变量选择)
         6. Validation rules
            1. $CustomMetadata.类型__mdt.记录API名.字段(可以方便的点击手选)

   2. Big Object

      1. 特点

         1. 存储十亿数量级别的数据
         2. 只支持对象和字段级别的权限
         3. 一旦部署就不可以再修改或删除对象的索引
         4. 每个org最多只能创建10个大对象
         5. 不可使用trigger,flow,process等

      2. 支持的字段类型 

         | 类型名              | 含义      |
         | ------------------- | --------- |
         | Lookup Relationship | 参照关系  |
         | Date/Time           | 日期/时间 |
         | Email               | 邮箱      |
         | Number              | 数字      |
         | Phone               | 电话      |
         | Text                | 字符串    |
         | Text Area (Long)    | 长文本区  |
         | URL                 | 链接      |

      3. 创建

         1. 创建一个Big Object

         2. 添加自定义字段

         3. 添加索引

            1. 一个索引最少有1个自定义字段,最多有5个自定义,且索引字段为必须项目
            2. Long Text不可以当做索引使用

         4. 添加数据

            1. Data Loader

            2. Apex

               1. 示例

                  ```java
                  Customer_Interaction__b bo = new Customer_Interaction__b();
                  bo.Account__c = '001R000000302D3';
                  bo.Game_Platform__c = 'PC';
                  bo.Play_Date__c = DateTime.newInstance(2018, 2, 5);
                  bo.In_Game_Purchase__c = 'A12569';
                  bo.Level_Achieved__c = '45';
                  bo.Lives_This_Game__c = 3;
                  bo.Score_This_Game__c = '5500';
                  bo.Play_Duration__c = 25;
                   
                  database.insertImmediate(bo);
                  ```

                  

         5. 查询数据

            1. 大对象中SOQL与标准对象/自定义对象的区别

               1. 可以使用=, <, >, <=, >=, IN
               2. 不可使用!=, LIKE, NOT IN, EXCLUDES,INCLUDES

            2. SOQL

               1. 对象后缀类型__b

               2. 查询条件必须遵循索引顺序,且索引字段不能出现断层,即当有n个索引字段时,查询条件只能是前m个连续字段(m<=n)

               3. 示例

                  ```sql
                  -- Account__c, Game_Platform__c, Play_Date__c分别是三个索引字段
                  
                  -- 全部索引查询可
                  SELECT Account__c, Game_Platform__c, Play_Date__c
                  FROM Customer_Interaction__b
                  WHERE Account__c='0015h00000m3NfDAAU' AND Game_Platform__c='PC' AND Play_Date__c=2018-02-06T00:00:00Z
                  
                  -- 前两个索引查询可
                  SELECT Account__c, Game_Platform__c, Play_Date__c
                  FROM Customer_Interaction__b
                  WHERE Account__c='0015h00000m3NfDAAU' AND Game_Platform__c='PC'
                  
                  -- 前一个索引查询可
                  SELECT Account__c, Game_Platform__c, Play_Date__c
                  FROM Customer_Interaction__b
                  WHERE Account__c='0015h00000m3NfDAAU'
                  
                  -- 出现索引不连续查询不可
                  SELECT Account__c, Game_Platform__c, Play_Date__c
                  FROM Customer_Interaction__b
                  WHERE Account__c='0015h00000m3NfDAAU' AND Play_Date__c=2018-02-06T00:00:00Z
                  ```

            3. Async SOQL

               1. 特点
                  1. 查询不具有实时性
                  2. 批量查询
               2. 使用场景
                  1. 查询出某些大对象的数据放入其他标准或者自定义对象,然后进行报表统计等操作
               3. 使用方式
                  1. Workbench:神器
                  2. Apex RestApi

5. 数据导入/导出

   1. Data Import Wizard
      1. 特点
         1. 轻量,便捷,手动,单次限量(50000条)
      2. 使用
         1. 设置快捷查找Data Import Wizard
   2. Data Export Wizard
      1. 特点
         1. 可定月,定周自动备份导出数据,根据版本不同功能有差异
      2. 使用
         1. 设置快捷查找Data Export
         2. 选择实时导出或者定期导出
         3. 选择导出数据规则及数据对象
         4. 通过邮件连接下载导出数据
   3. Data Loader
      1. 特点
         1. 客户端安装,强大,可连接数据库,可API自动调用,单次限量(5000000条)
      2. 使用
         1. 设置快捷查找Data Loader下载最新Data Loader安装
         2. 登录账户
         3. 导入导出
            1. 导出
               1. 选表→导出地→导出SOQL做成→导出
            2. 删除
               1. 导出删除目标获取删除ID数据源
               2. 选表→选择删除ID数据源→编辑ID映射→删除
            3. 导入
               1. 选表→导入源文件→编辑字段映射→导入
      3. 可能问题
         1. 乱码
            1. UTF-8以外乱码调整数据源编码格式为对应编码格式如中文GBK,日文Shift-JIS
            2. UTF-8乱码则设置读写编码格式为UTF-8

6. Package:打包

   1. Package Manager
      1. 创建包名
      2. 添加打包内容
      3. 上传
      4. 向邮箱发送安装URL

7. Apex

   1. 使用

      1. 场景
         1. Lightning Components
         2. Visualforce
         3. SOAP API
      2. 工具
         1. Developer Console
         2. Visual Studio Code
         3. Workbench
            1. https://github.com/forceworkbench/forceworkbench
      3. 阶段
         1. 开发
            1. 创建对象
            2. 建立触发
         2. 编译
         3. 调试
            1. System.debug()
         4. 测试
            1. @isTest()
            2. System.assertEquals('期望值', '实际值');
         5. 性能检测
         6. 部署
            1. deploy()
            2. runTests()

   2. 运行原理

      ![Apex is compiled, run, and stored entirely on the Lightning Platform](https://developer.salesforce.com/docs/resources/img/en-us/234.0?doc_id=dev_guides%2Fapex%2Fimages%2Fapex_architecture.png&folder=apexcode)

      1. 开发人员编写apex源代码保存到平台应用服务
      2. 平台编译为抽象指令集保存其元数据(编译为class文件)
      3. 终端用户请求交互触发执行Apex代码,平台通过元数据查询指令集,移交Apex运行时解释器执行返回结果

   3. 基本语法
      1. 数据类型

         ```java
         String name='LJ';//apex中的字符串为基本变量类型,且只能用单引号
         Integer age=26;
         Boolean gender=true;
         Decimal money=3.1415926;
         Double asset=2.71828;
         Long s=123456789L;
         Date d = Date.newInstance(2021, 12, 25);
         Time t=Time.newInstance(12, 30, 30, 888);
         DateTime dt=DateTime.newInstance(2021, 12, 25, 01, 01, 01);
         ID id='00300000003T2PGAA0';
         Object obj = 对象老祖;
         ```

      3. 变量与常量

         ```java
         /* 静态常量*/
         public static final Integer C=123;
         
         /* 变量同数据类型*/
         Integer i = 1;
         Account account = new Account();
         Account[] accounts = [SELECT Id FROM Account];
         ```

      4. 运算符

         | 符号      | 含义                                 |
         | --------- | ------------------------------------ |
         | ==        | 值相等,引用可不等                    |
         | ===       | 值相等,引用也得相等                  |
         | !=        | 同上                                 |
         | !==       | 同上                                 |
         | =         | 以下同java                           |
         | x ? y : z | 三元运算                             |
         | &&        | 与                                   |
         | \|\|      | 或                                   |
         | !         | 取反                                 |
         | <,<=,>,>= | 比较大小                             |
         | +,-,*,/   | 加减乘除                             |
         | ++,--     | 自增,自减                            |
         | &         | 按位与(0&0=0,1&0=0,0&1=0,1&1=1)      |
         | \|        | 按位或(0\|0=0,1\|0=1,0\|1=1,1\|1=1)  |
         | ^         | 按位异或(0\^0=0,1\^0=1,0\^=1,1\^1=0) |
         | <<        | 有符号左移                           |
         | >>        | 有符号右移                           |
         | >>>       | 无符号右移                           |
         
      5. 枚举
      
         ```java
         enum WeekDay{
             MON,
             TUE,
             WED,
             THU,
             FRI,
             SAT,
             SUN
         }
         System.debug(WeekDay.values());
         System.debug(WeekDay.MON.name());
         System.debug(WeekDay.MON.ordinal());
         ```

      6. 集合

         ```java
         /*
         详细API参考官方指南,同java几乎完全一致
         */
         /* List */
         List<Integer> list = new List<Integer>();//未初始化,list和数组等效互换可
         List<Integer> list = new List<Integer>{1, 2, 3};//初始化
         
         /* Set */
         Set<String> set = new Set<String>();//未初始化
         Set<String> set = new Set<String>{'a', 'b', 'c'};//初始化
         
         /* Map */
         Map<Integer, String> map = new Map<Integer, String>();//未初始化
         Map<Integer, String> map = new Map<Integer, String>{1 => 'a', 2 => 'b', 3 => 'c'};//初始化
         ```

      7. 控制流程
      
         ```java
         /* If*/
         Integer i =0;
         if (i<0){
           System.debug('负数');  
         }else if(i>0){
           System.debug('正数');
         }else{
           System.debug('0');   
         }
         
         /* switch*/
         //使用类型[Integer,Long,sObject,String,Enum]
         switch on day{
             when MON {
                 System.debug('星期一');  
             } 
             ...
             when null {
                 System.debug('空');  
             } 
             when else {
                 System.debug('预定以外');  
             }     
         }
         
         /* for*/
         Integer sum=0;
         for(Integer i=0;i<=100;i++){
             sum+=i;
         }
         System.debug(sum);
         
         /* for each*/
         List<Account> accounts=[select id,name from Account limit 5];
         for(Account acc:accounts){
             System.debug(acc);
         }
         
         /* do*/
         Integer sum=0;
         Integer i=0;
         do{
             sum+=i;
             i++;
         }while(i<=100);
         System.debug(sum);
         
         /* while*/
         Integer sum=0;
         Integer i=0;
         while(i<=100){
             sum+=i;
             i++;
         }
         System.debug(sum);
         ```
      
   4. 数据交互层

      1. DML

         1. DML数据操作语言(数据库的增删改)

            1. SF中可以操作单条记录(Single)也可以操作批量数据(Bulk)
            2. 一次SF事务控制中只能有150个DML语句限制,所以尽量使用Bulk,从而防止被监视限制(governor limits)

         2. 增删改

            1. 增加

               ```sql
               insert 对象/对象集合
               ```

            2. 增加/更新

               ```sql
               -- 省略字段名则默认ID
               upsert 对象/对象集合
               upsert 对象/对象集合 sObject.Fields.字段名
               ```

            3. 删除

               ```sql
               delete 对象/对象集合
               ```

            4. 更新

               ```sql
               update 对象/对象集合
               ```

            5. 恢复

               ```sql
               -- SOQL中使用ALL ROWS可查询出删除掉的数据然后恢复
               undelete 对象/对象集合
               ```

            6. 合并

               ```sql
               -- 合并只能作用于Account,Contact,Case,Lead对象,合并对象最多有两个
               merge 对象 合并对象 
               ```

            7. 基本使用

               ```java
               try {
                   //DML操作语句
                   ...
               } catch (DmlException e) {
                   //异常处理
                   System.debug(e.getMessage()); 
               }
               ```

      2. Database命名空间

         1. 增加

            ```java
            SaveResult[] saveResult = Database.insert(对象/对象集合, false);//默认true全局事务,false局部事务
            ```

         2. 更新

            ```java
            SaveResult[] saveResult = Database.update(对象/对象集合, false);
            ```

         3. 增加/更新

            ```java
            UpsertResult[] upsertResult = Database.upsert(对象/对象集合, false);
            ```

         4. 删除

            ```java
            DeleteResult[] deleteResult = Database.delete(对象/对象集合, false);
            ```

         5. 恢复

            ```java
            UndeleteResult[] undeleteResult = Database.undelete(对象/对象集合, false);
            ```

         6. 合并

            ```java
            MergeResult[] mergeResult = Database.merge(对象,合并对象, false);
            ```

         7. 基本使用

            ```java
            for (Database.结果 ret : 结果集) {
                if (ret.isSuccess()) {
                    System.debug('成功');
                } else {
                    for(Database.Error err : sr.getErrors()) {
                        System.debug('失败'+ ': ' + err.getMessage());
                    }
                }
            }
            ```

      3. SOQL

         1. 通用模板

            ```sql
            SELECT 对象字段,[父.字段],[(子查询)] FROM 对象 [WHERE 条件] [ORDER BY 某个可排序字段 ASC/DESC] [NULLS FIRST/LAST] [LIMIT 件数限制] [OFFSET 偏移行数] [FOR UPDATE] [ALL ROWS]
            ```

            1. 对象字段
               1. 无法使用SELECT *
               2. 可选当前对象的任意字段,默认会自动查询出ID
               3. Fields(ALL,CUSTOM.STANDARD)
                  1. 使用ALL,CUSTOM时最大限度200条

            2. 父.字段
               1. 子查父时可以直接查询父对象的字段

            3. (子查询)
               1. 父查子时使用小括号

            4. 对象
               1. 标准对象
                  1. 对象名=对象名
               2. 定制对象
                  1. 主干查询
                     1. 对象名=对象名__c
                  2. 子查父/父查子
                     1. 对象名=对象名__r

            5. 条件
               1. =,!=,<,<=,>,>=, 
               2. AND/OR/NOT
               3. LIKE
                  1. %:通配任意字符
                  2. _:通配一个字符
                  3. 匹配特殊字符需要用\转义
               4. IN('A','B')
                  1. A或者B
               5. NOT IN('A','B')
                  1. A,B以外
               6. INCLUDES('A;B','C')
                  1. 包含A同时包含B或者包含C
               7. EXCLUDES('A')
                  1. 不包含A
               8. 条件中引用变量
                  1. WHERE 字段名=:变量

            6. ORDER BY
               1. ASC:升序
               2. DESC:降序

            7. NULLS FIRST/LAST
               1. FIRST:排序时遇到NULL放在前面
               2. LAST:排序时遇到NULL放在后面

            8. OFFSET
               1. 设定偏移行数,结合LIMIT可用于分页查询

            9. FOR UPDATE
               1. 加锁更新操作

            10. ALL ROWS
                1. 可查询出删除掉的数据

            11. 共通多态字段

                1. 常见多态字段

                   1. Owner:数据的所有者(User,Calendar)
                   2. Who:数据的所有人(User)
                   3. What:数据相关的参照类型(任意)

                2. 属性

                   1. Type
                      1. What.Type可以判断多态字段的真实类型

                3. 动态SOQL

                   1. ```sql
                      SELECT 
                        TYPEOF What
                          WHEN Account THEN Phone, NumberOfEmployees
                          WHEN Opportunity THEN Amount, CloseDate
                          ELSE Name, Email
                        END
                      FROM Event
                      ```

                   2. 

            12. 举例

                1. ```sql
                   -- 自定义子对象查父对象,父对象为自定义对象时使用__r(Order__r),父对象为标准对象时将末尾ID去掉(CreatedById=>CreatedBy),父对象的项目可以直接
                   select ID,Name,Price__c,Order__r.ID,Order__r.Name,Order__r.SumPrice__c,CreatedBy.Name from OrderDetail__c
                   -- 标准子对象查父对象,父对象为自定义时对象使用__r(Order__r),父对象为标准对象时将末尾ID去掉(AccountId=>Account)
                   SELECT Id,FirstName, Account.Id,Account.Name,Order__r.Name from Contact where id ='0035h00000ZpefOAAR'
                   -- 标准父对象查子对象,子对象为标准对象时使用关联list名(Contacts)
                   Select Id,Name,(Select Id,Name From Contacts) From Account
                   -- 自定义父对象查子对象,父对象为自定义对象时使用关联list名__r(Employee__r)
                   Select Id,Name,(Select Id,Name From Employee__r) From Department__c
                   
                   -- OwnerId,WhatId,WhoId是泛型字段,代表未知类型的参照关系
                   SELECT Id, Subject,Owner.Type,What.Type,Who.Type,StartDateTime,EndDateTime FROM Event
                   ```

                2. 

         2. 转义字符

            | 用法                      | 含义                                          |
            | :------------------------ | :-------------------------------------------- |
            | \n or \N                  | New line                                      |
            | \r or \R                  | Carriage return                               |
            | \t or \T                  | Tab                                           |
            | \b or \B                  | Bell                                          |
            | \f or \F                  | Form feed                                     |
            | \\"                       | One double-quote character                    |
            | \\'                       | One single-quote character                    |
            | \\\                       | Backslash                                     |
            | LIKE expression only: \\_ | Matches a single underscore character ( _ )   |
            | LIKE expression only:\\%  | Matches a single percent sign character ( % ) |

      4. SOSL

         1. 通用模板

            ```sql
            -- apex格式
            FIND '目标值' [IN 字段范围] RETURNING sObject(查询字段 [WHERE 条件] [ORDER BY 排序字段] [LIMIT 件数限制])
            
            -- Query Editor格式
            FIND {目标值} [IN 字段范围] RETURNING sObject(查询字段 [WHERE 条件] [ORDER BY 排序字段] [LIMIT 件数限制])
            ```
            
            1. 目标值
               1. 'A'
                  1. 包含A
               2. 'A  AND B'
                  1. 同时包含A和B
               3. 'A OR B'
                  1. 包含A或者B
               4. 'A?'
                  1. A后跟一个字符
               5. 'A*'
                  1. A后跟任意多个字符
            2. 字段范围
               1. ALL FIELDS:全字段

      5. SOQL和SOSL的异同

         1. 都是为了查询符合某些条件的数据记录
         2. SOQL一次只能查询一种对象,而SOSL一次可以查询多种对象
         3. SOQL不使用LIKE时为精准查找,而SOSL为包含匹配

   5. Apec面向对象编程

      1. Keyword

         1. private 

            1. 作用在内部类时可省略
            2. 可作用在测试类
            3. 作用在方法和变量的时候类内部私有

         2. protected

            1. 只对子类可视或者内部类
            2. 只能用在方法和成员变量

         3. public 

            1. 在应用范围和命名空间内可视,比java的public范围小

         4. global

            1. 全局暴露,当前应用外访问时

         5. with sharing

            1. 强制当前用户共享规则,记录级别控制

         6. without sharing

            1. 非强制当前用户共享规则,记录级别控制

         7. inherited sharing

            1. 运行时动态控制共享规则,由调用该类的调用者类共享模式决定,调用者为with sharing时则为with sharing为without sharing时则为without sharing

         8. virtual

            1. 作用在类上时为虚类,可以实例化
            2. 虚类可以直接实例化,也可以被继承
            3. 作用在方法上时为虚方法,虚方法可以被默认使用,也可以被重写,重写时需要override关键字

         9. abstract

            1. 作用在类时为抽象类,不可被实例需要被子类继承
            2. 作用在方法时,没有方法体,子类应当override重写该方法,或者继续保持抽象,存在抽象方法的类必须为抽象类

         10. override

             1. 重写方法关键字,区别于java的注解,apex直接写在方法修饰子处
             2. override只用在虚方法和抽象方法,接口方法实现无需override关键字

         11. extends 

             1. 只有抽象类或者虚类才能被继承
             2. 继承父类,同java只可单继承
             3. 接口也可以继承接口

         12. implements 

             1. 实现接口,同java可以多实现

         13. final

             1. final修饰的变量只能赋值一次,且只能是声明时直接赋值或者构造器中赋值
             2. final修饰的静态常量只能赋值一次,且只能是声明时直接赋值或者静态代码块中赋值
             3. 定义常量使用static final双修饰子
             4. apec中final不能作用在类上和方法上,因为默认为final,如若类想被继承或者方法想被重写,使用virtual虚类和虚方法

         14. static

             1. 同java静态变量只能直接被类访问,不能被实例对象访问
             2. 同java静态方法只能直接被类访问,不能被实例对象访问

         15. instanceof

             1. 和java一样判断某个实例是否为某个类的实例

                ```java
                实例对象 instanceof 类名
                ```

         16. super

             1. 调用父类构造器时同java

                ```java
                super();
                ```

             2. 还可重写父类方法时调用父类方法

                ```java
                  public override void methodName() {
                        super.methodName();
                        ...
                  }
                ```

         17. this

             1. 调用成员变量

                ```java
                public class ClassNama {
                private string name;
                  {
                      this.name = 'LJ';
                  }
                }
                ```

             2. 重载构造器

                ```java
                public class ClassName {
                   public ClassName(string name) {
                       
                   }
                   public ClassName() {
                       this('LJ');
                   }
                }
                ```

      2. Object/sObject

         1. Object 

            1. 面向对象的根类

         2. sObject 

            1. SalesForce中的特殊Object,他对应一张表,可能是内置的也可能是自定义的,是Object子类

            2. sObject作为多态引用时,只能[.]取得实际对象的ID,若要读写其他字段使用get(),put()如下

               ```java
               sObject account =[SELECT Id, Name FROM Account LIMIT 1];
               ID accId=account.Id;//直接获取ID
               String name=account.get('NAME');//get字段
               account.put('NAME','');//put字段
               ```

      3. Class

         ```java
         //类定义
         private | public | global [virtual | abstract | with sharing | without sharing | inherited sharing] class 类名 [implements 接口] [extends 父类名] { 
             //成员变量
             private 类型 成员变量名 [{get;set;}];
             
             //静态代码块
             static {
         		//静态代码
         	}
             
             //初始代码块
             { 
         		//初始代码
         	}
             
             //构造器
             public 类名 ([参数列表]) {
                 //构造器代码
             };
             
             //变量定义
         	[public | private | protected | global] [final] [static] 变量类型 变量名 [= 赋值];
             
             //方法定义
         	[public | private | protected | global] [override] [static] 返回类型 方法名 (参数列表) {
         		//方法体
         	};
             
             //属性(getter/setter)
             public | private | protected | global [static] 类型 成员变量名 {
                 get { 
                     return 成员变量名;
                 }
                 set { 
                     成员变量名 = value;
                 }
             }
         }
         ```

         1. 同java静态代码块只在初始加载类信息时运行一次
         2. 初始代码同构造器一样每次实例化都被执行一次
         3. 执行顺序优先度
            1. 静态代码块>初始代码块>构造器
         4. 同java方法可以重写,也能重载
         5. 方法参数基本类型值传递,引用类型引用传递
         6. 属性的功能主要是封装成员变量,并对外暴露访问权限,面向对象的封装性质,还能控制成员变量的读写过程,如写入校验
         7. 属性
            1. get方法必须有返回值
            2. 自动属性
               1. {get;set;}读写
               2. {get;}只读
               3. {set;}只写

      4. Interface

         ```java
         //接口定义
         public [virtual] interface 接口名{
             //方法
         	void methodName();
         } 
         ```

         1. 接口不能包含构造器,因为不可被实例化
         2. 接口中的方法没有方法体,要被实现类实现,只能含有空方法
         3. 接口中的方法不要加public,实现类实现的时候需要加public
         4. 接口可以多实现,而继承只能单继承
         5. 接口中不能含有成员变量,常量
         6. 实现接口方法无需override关键字

      5. Exception

         1. 基本使用

            ```java
            try {
                throw new NullPointerException();
            } catch (Exception e) {
                System.debug(e.getMessage());
            } finally {
                System.debug('最后执行');
            }
            ```
         
         2. 常见异常

            1. | Exception                   | 示例                          | 异常信息                                                     |
               | --------------------------- | ----------------------------- | ------------------------------------------------------------ |
               | System.MathException        | 1/0                           | Divide by 0                                                  |
               | System.LimitException       | 一次执行101条SOQL             | Too many SOQL queries: 101                                   |
               | System.DmlException         | 插入记录未给必须项目赋值      | Required fields are missing                                  |
               | System.ListException        | 数组越界                      | List index out of bounds                                     |
               | System.NullPointerException | 空指针                        | Attempt to de-reference a null object                        |
               | System.QueryException       | Sobject sobj=[]未取到一条数据 | List has no rows for assignment to SObject                   |
               | System.SObjectException     | 无法获取到相应字段            | SObject row was retrieved via SOQL without querying the requested field |
         
         3. 共同方法
         
            1. | 方法名              | 含义               |
               | ------------------- | ------------------ |
               | getCause            | 返回导致异常的对象 |
               | getLineNumber       | 异常抛出代码行数   |
               | getMessage          | 异常信息           |
               | getStackTraceString | 异常栈信息         |
               | getTypeName         | 异常类             |

         4. DML特有方法

            1. | 方法名                  | 含义             |
               | ----------------------- | ---------------- |
               | getDmlFieldNames(index) | 导致错误的字段   |
               | getDmlId(index)         | 错误记录ID       |
               | getDmlMessage(index)    | 指定行的错误信息 |
               | getNumDml()             | 错误行数         |
         
         5. 自定义异常
         
            1. ```java
               //必须以Exception结尾
               //可以实现一个或多个接口
               public class MyException extends Exception {
                   
               }
               ```
         
      6. Trigger
      
         触发器
      
         1. 触发时点
      
            | 时点           | 用法   |
            | -------------- | ------ |
            | before insert  | 插入前 |
            | before update  | 更新前 |
            | before delete  | 删除前 |
            | after insert   | 插入后 |
            | after update   | 更新后 |
            | after delete   | 删除后 |
            | after undelete | 恢复后 |
      
         2. 内置对象变量
      
            System.Trigger内部自带运行时状态变量
      
            | 变量名        | 用法                                                         |
            | ------------- | ------------------------------------------------------------ |
            | isExecuting   | 当前上下文是否为触发器                                       |
            | isInsert      | 插入事件                                                     |
            | isUpdate      | 更新事件                                                     |
            | isDelete      | 删除事件                                                     |
            | isBefore      | 事件发生前                                                   |
            | isAfter       | 事件发生后                                                   |
            | isUndelete    | 恢复数据                                                     |
            | new           | 获取原生对象列表,只用于insert, update,undelete(必须有新对象产生) |
            | newMap        | 原生ID映射的对象map,只用于before update, after insert, after update, after undelete |
            | old           | 旧对象列表,只用于update,delete                               |
            | oldMap        | 旧ID映射的对象map,只用于update,delete                        |
            | operationType | BEFORE_INSERT, BEFORE_UPDATE, BEFORE_DELETE,AFTER_INSERT, AFTER_UPDATE, AFTER_DELETE, and AFTER_UNDELETE分别与触发时点一一对应 |
            | size          | 一次触发中涉及的所有记录条数                                 |
      
         3. 使用方法
      
            为了覆盖到所有知识点,刻意构造以下模板
      
            ```java
            trigger 触发器名Trigger on 对象名称 (before insert,before update,before delete,after insert,after update,after delete,after undelete) {
               //模板代码
               if(Trigger.isExecuting){
                  System.debug('触发器开始');
                  System.debug('执行类型:'+Trigger.operationType);
                  if(Trigger.isBefore){
                     System.debug('触发器执行前事件');
                     if(Trigger.isInsert){
                        System.debug('触发器执行前事件->插入');
                        //new
                        Object[] newObjs = Trigger.new;
                        System.debug(newObjs);
                     }
                     if(Trigger.isUpdate){
                        System.debug('触发器执行前事件->更新');
                        //new
                        Object[] newObjs = Trigger.new;
                        System.debug(newObjs);
                        //newMap
                        Map<ID,Object> newMaps=Trigger.newMap;
                        System.debug(newMaps);
                        //old
                        Object[] oldObjs = Trigger.old;
                        System.debug(oldObjs);
                        //oldMap
                        Map<ID,Object> oldMaps=Trigger.oldMap;
                        System.debug(oldMaps);           
                     }
                     if(Trigger.isDelete){
                        System.debug('触发器执行前事件->删除');
                        //old
                        Object[] oldObjs = Trigger.old;
                        System.debug(oldObjs);
                        //oldMap
                        Map<ID,Object> oldMaps=Trigger.oldMap;
                        System.debug(oldMaps);         
                     }
                  }else if(Trigger.isAfter){
                     System.debug('触发器执行后事件');
                     if(Trigger.isInsert){
                        System.debug('触发器执行后事件->插入');
                        //new
                        Object[] newObjs = Trigger.new;
                        System.debug(newObjs);
                        //newMap
                        Map<ID,Object> newMaps=Trigger.newMap;
                        System.debug(newMaps);
                     }
                     if(Trigger.isUpdate){
                        System.debug('触发器执行后事件->更新');
                        //new
                        Object[] newObjs = Trigger.new;
                        System.debug(newObjs);
                        //newMap
                        Map<ID,Object> newMaps=Trigger.newMap;
                        System.debug(newMaps);
                        //old
                        Object[] oldObjs = Trigger.old;
                        System.debug(oldObjs);
                        //oldMap
                        Map<ID,Object> oldMaps=Trigger.oldMap;
                        System.debug(oldMaps);            
                     }
                     if(Trigger.isDelete){
                        System.debug('触发器执行后事件->删除');
                        //old
                        Object[] oldObjs = Trigger.old;
                        System.debug(oldObjs);
                        //oldMap
                        Map<ID,Object> oldMaps=Trigger.oldMap;
                        System.debug(oldMaps);              
                     }
                     if(Trigger.isUndelete){
                        System.debug('触发器执行后事件->恢复');
                        //new
                        Object[] newObjs = Trigger.new;
                        System.debug(newObjs);
                        //newMap
                        Map<ID,Object> newMaps=Trigger.newMap;
                        System.debug(newMaps);            
                     }
                  }
               }
               System.debug('触发器执行记录数:'+Trigger.size);
               System.debug('触发器结束');
            }
            ```
      
      7. Apex Asynchronous(异步)
      
         1. 特点
      
            1. 高效性
            2. 可扩展性
            3. 增加Governor Limits(单个线程的Governor Limits是独立的)
      
         2. 四种方法
      
            1. Future methods
      
               1. 使用场景
      
                  1. Web service callout
      
               2. 用法
      
                  1. @future
                  2. static
                  3. void
                  4. 参数:必须为基本类型或者基本类型的集合
                     1. 原因:异步执行并非立刻执行,系统资源可用时调用,若传入Sobject可能对象内容已经被修改
                  5. 不能用future方法调用另一个future方法，也不能在trigger中调用，future方法中不能使用getContent()和GetContentAsPDF()方法
      
               3. 示例
      
                  1. ```java
                     public class 类名 {
                       @future()/@future(callout=true)
                       public static void 方法名(List<Id> recordIds) {
                     	
                       }
                     }
                     
                     //调用
                     类名.方法名(参数);
                     ```
      
            2. Batch Apex
      
               1. 使用场景
      
                  1. 海量记录处理,增加Governor Limits
      
               2. 用法
      
                  1. 实现Database.Batchable接口,若要实现多个job资源共享可以添加Database.Stateful实现
                  2. 实现start/execute/finish三个方法
      
               3. 示例
      
                  1. ```java
                     
                     public with sharing class LeadProcessor implements Database.Batchable<SObject> ,Database.Stateful {
                         public LeadProcessor() {
                         }
                         //共享变量
                         public Integer recordCount = 0;
                         // 前置处理
                         public Database.QueryLocator start(Database.BatchableContext bc) {
                             String query = 'select LeadSource from Lead';
                             return Database.getQueryLocator(query);
                         }
                         // 业务处理
                         public void execute(Database.BatchableContext bc, List<Lead> records){
                             List<Lead> updateRecords = new List<Lead>();
                             for (Lead record : records) {
                                 record.LeadSource = 'Dreamforce';
                                 updateRecords.add(record);
                                 recordCount++;
                             }
                             update updateRecords;
                         }
                         // 后置处理,释放资源,邮件发送等
                         public void finish(Database.BatchableContext bc){
                             //获取job执行信息
                             AsyncApexJob job = [SELECT Id, Status, NumberOfErrors,JobItemsProcessed,TotalJobItems, CreatedBy.Email
                             FROM AsyncApexJob
                             WHERE Id = :bc.getJobId()];
                             System.debug('job:'+job);
                             System.debug('recordCount:'+recordCount);
                         }
                     }
                     
                     
                     
                     //测试
                     @isTest
                     public with sharing class LeadProcessorTest {
                     	//准备数据
                         @TestSetup
                         static void makeData(){
                             List<Lead> leads = new List<Lead>();
                             for (Integer i = 1; i <= 200; i++) {
                                 Lead ld = new Lead(LastName='name'+i,Company='comp'+i);
                                 leads.add(ld);
                             }
                             insert leads;
                         }
                     
                         @isTest
                         public static void testLeadProcessor(){
                             //开始
                             Test.startTest();
                             	//调用Batch
                                 LeadProcessor lp =new LeadProcessor();
                                 ID batchId = Database.executeBatch(lp, 200);
                             //结束
                             Test.stopTest();
                             //断言
                             System.assertEquals(200, [select count() from Lead where LeadSource = 'Dreamforce']);
                         }
                     
                     }
                     
                     ```
      
            3. Queueable Apex
      
               1. 使用场景
      
               2. 用法
      
                  1. 实现Queueable接口
                  2. 实现execute方法
      
               3. 特点
      
                  1. 可以使用非基本类型Sobject,Apex Class
                  2. 可以链式执行,前一个Batch执行结果交给后一个Batch
                  3. 单次事务控制队列中最多只能有50个job
                  4. 链式job只能调用一个子job
      
               4. 示例
      
                  1. ```java
                     public with sharing class AddPrimaryContact implements Queueable{
                     
                         private Contact cat;
                         private String state;
                         public AddPrimaryContact(Contact cat,String state) {
                             this.cat = cat;
                             this.state = state;
                         }
                     
                         public void execute(QueueableContext context) {
                            List<Account> accs = [select Name,(Select id,FirstName,LastName from contacts) from Account where BillingState=:this.state limit 200];
                            List<Contact> cats = new List<Contact>();
                             for (Account acc : accs) {
                                 //克隆一个对象
                                 Contact con =cat.clone(false, false, false, false);
                                 con.AccountId = acc.id;
                                 cats.add(con);
                             }
                             insert cats;
                         }
                     
                     }
                     
                     
                     
                     //测试
                     @IsTest
                     public with sharing class AddPrimaryContactTest {
                         @IsTest
                         static void methodName(){
                             //准备数据
                             List<Account> accs = new List<Account>();
                             for (Integer i = 1; i <= 50; i++) {
                                 accs.add(new Account(Name='Name'+i,BillingState='NY'));
                             }
                             for (Integer i = 51; i <= 100; i++) {
                                 accs.add(new Account(Name='Name'+i,BillingState='CA'));
                             }
                             insert accs;
                             Contact cat = new Contact(LastName='AddPrimaryContactTest');
                             insert cat;
                             Test.startTest();
                             AddPrimaryContact ac = new AddPrimaryContact(cat,'CA');
                                 //启动Job
                                 ID jobId = System.enqueueJob(ac);
                             Test.stopTest();
                             //断言
                             Integer cout = [select count() from Contact where AccountId in (select id from Account where BillingState='CA')];
                             System.assertEquals(50, cout);
                             
                         }
                     }
                     ```
      
            4. Scheduled Apex
      
               1. 使用场景
      
                  1. 定时执行
      
               2. 用法
      
                  1. 实现Schedulable 接口
                  2. 实现execute方法
      
               3. 示例
      
                  1. ```java
                     public with sharing class DailyLeadProcessor implements Schedulable{
                         public DailyLeadProcessor() {
                         }
                         public void execute(SchedulableContext ctx) {
                             List<Lead> leads = [SELECT Id, LeadSource FROM Lead WHERE LeadSource = null OR LeadSource=''];
                             for (Lead ld : leads) {
                                 ld.LeadSource = 'Dreamforce';
                             }
                             update leads;
                         }
                     }
                     
                     
                     
                     //测试
                         @isTest
                     public with sharing class DailyLeadProcessorTest {
                         @TestSetup
                         static void makeData(){
                             List<Lead> leads = new List<Lead>();
                             for (Integer i = 1; i <=200; i++) {
                                 leads.add(new Lead(LastName='name'+i,Company='comp'+i));
                             }
                             insert leads;
                         }
                     
                         @isTest
                         public static void testDailyLeadProcessor(){
                             Test.startTest();
                             	//启动定时任务
                                 DailyLeadProcessor dp = new DailyLeadProcessor();
                             	//'0 0 0 15 6 ? 2022' CRON定时表达式,参考下面链接
                             	//https://developer.salesforce.com/docs/atlas.en-us.224.0.apexcode.meta/apexcode/apex_scheduler.htm
                                 String jobId = System.schedule('定制执行任务名称', '0 0 0 15 6 ? 2022', dp);
                             Test.stopTest();
                         }
                     }
                     
                     ```
      
            5. 监控job状态
      
               1. Set Up→Apex Jobs
      
               2. 分类
      
                  1. Queued Jobs
      
                     ```sql
                     AsyncApexJob jobInfo = [SELECT Status, NumberOfErrors FROM AsyncApexJob WHERE Id = :jobID];
                     ```
      
                  2. Scheduled Jobs
      
                     ```sql
                     CronTrigger ct = [SELECT TimesTriggered, NextFireTime FROM CronTrigger WHERE Id = :jobID];
                     CronJobDetail ctd = [SELECT Id, Name, JobType FROM CronJobDetail WHERE Id = :job.CronJobDetail.Id];
                     ```
      
      8. Test
      
         1. 要点
      
            1. 测试方法必须为static修饰
            2. SalesForce覆盖率最低要求75/100
            3. SF默认Test数据不会提交,也不可以访问系统存在的数据除非加@isTest(SeeAllData=true)
      
         2. Class测试
      
            ```java
            //被测试代码
            public class Calculator {
                public Calculator(){}
                //加
                public static Decimal add(Decimal a, Decimal b) {
                    return a + b;
                }
                //减(非静态方法)
                public Decimal sub(Decimal a, Decimal b) {
                    return a - b;
                }
                //乘
                public static Decimal mul(Decimal a, Decimal b) {
                    return a * b;
                }
                //除
                public static Decimal div(Decimal a, Decimal b) {
                    return a / b;
                }
            }
            
            //测试代码
            @isTest
            public|private class CalculatorTest {
                private static Calculator calc = new Calculator();
            
                @testSetup
                static void setup() {
            		//如果需要准备数据,此处准备测试数据
                }
                
                @isTest(SeeAllData=false)
                public static void testAdd() {
                    System.assertEquals(2, Calculator.add(1, 1));
                }
            
                @isTest
                public static void testSub() {
                    System.assertEquals(0, calc.sub(1, 1));
                }
            
                @isTest
                public static void testMul() {
                    System.assertEquals(1, Calculator.mul(1, 1));
                }
            
                @isTest
                public static void testDiv() {
                    System.assertEquals(1, Calculator.div(1, 1));
                }
            }
            ```
      
         3. Trigger测试
      
            ```java
            //被测试代码
            trigger DeleteAccountTrigger on Account (before delete) {
                List<Account> accounts=[select id from Account where id in :Trigger.old and id in (select accountid from Opportunity)];
                for(Account acc:accounts){
                    Trigger.oldMap.get(acc.Id).addError('Cannot delete account with related opportunities.');
                }
            }
            
            
            //测试代码
            @isTest()
            public class DeleteAccountTriggerTest {
                @testSetup
                static void setup() {
            		//如果需要准备数据,此处准备测试数据
                    Account acc=new Account(Name='acc1');
                    insert acc;
                    Opportunity opp=new Opportunity(AccountId=acc.id,
                                                    Name='opp1',
                                                    StageName='StageName1',
                                                    CloseDate=Date.newInstance(2021, 12, 25),
                                                    Discount_Percent__c=50);
                    insert opp;
                }
                
                @isTest()
                public static void deleteAccountTriggerTest1(){
            
            
                    //开始测试
                    Test.startTest();
                        Database.DeleteResult result =Database.delete(acc.Id, false);
                        //删除失败
                        System.assertEquals(false, result.isSuccess());
                        //失败信息不为空
                        System.assertEquals(true, result.getErrors().size()!=0);
                        //失败信息
                        System.assertEquals('Cannot delete account with related opportunities.', 
                                            result.getErrors().get(0).getMessage());
                    //测试结束
                    Test.stopTest();
                }
            }
            ```
      
      9. SF与外部系统的交互
      
         1. RESTApi
      
            1. SF访问外部
      
               1. Remote Site Settings的设定
      
               2. 写apex代码访问外部系统API
      
                  ```java
                  HttpRequest request =new HttpRequest();
                  request.setEndpoint('http://localhost:8001/edu/course/getAllCourseList');
                  request.setMethod('GET');
                  Http htp=new Http();
                  HttpResponse response= htp.send(request);
                  System.debug(response.getBody());
                  ```
      
            2. 外部访问SF
      
               1. 创建RESTApi
      
                  ```java
                  @RestResource(urlMapping='/test/*')//URL映射
                   global with sharing class MyRestApi {
                      public MyRestApi() {
                      }
                  
                      @httpGet//get
                      global static String getTest(){
                          return 'this is a restapi get test';
                      }
                  }
                  ```
      
               2. 创建一个Connected App
      
                  1. 参考:https://help.salesforce.com/s/articleView?id=sf.connected_app_create.htm&type=5
      
               3. 获取Token
      
                  1. post请求获取Token
      
                     ```bash
                     https://自己的域名/services/oauth2/token?grant_type=password&client_id=公钥&client_secret=秘钥&username=用户名&password=密码
                     ```
      
                  2. 响应得到Token
      
                     ```json
                     {
                         "access_token": "00D5h000004eAF0!ARIAQPDkGrkWZUlN8TjSDk8mF2oBptVafZuVg0UEz9QhVKEntSfD_nkkYSDCf2uBoTD3AYxaeDkslGAIMRAV7fbBVKEZDrhX",
                         "instance_url": "https://jpanda-dev-ed.my.salesforce.com",
                         "id": "https://login.salesforce.com/id/00D5h000004eAF0EAM/0055h000003yxsXAAQ",
                         "token_type": "Bearer",
                         "issued_at": "1649511637621",
                         "signature": "+pCGWKnDRFPQtrHBjbA6bNxruDu8Kg4j76oy2naPqyQ="
                     }
                     ```
      
                  3. 请求时带上Authorization=token_type+" "+access_token
      
                     ```bash
                     https://自己的域名/services/apexrest/URL映射/
                     ```
      
               4. 注意事项
      
                  1. @RestResource注解的类必须为global
                  2. @httpGet注解的方法必须为static
                  3. 公钥=自己创建的Connected App的Consumer Key
                  4. 秘钥=自己创建的Connected App的Consumer Secret
      
      10. Annotation
      
         注解暂不研究,遇到一个完善一个
      
         | 注解                | 参数                                | 含义                                |
         | ------------------- | ----------------------------------- | ----------------------------------- |
         | AuraEnabled         | @AuraEnabled(cacheable=true)        | 组件可调用的Apex                    |
         | deprecated          |                                     |                                     |
         | future              |                                     |                                     |
         | InvocableMethod     |                                     |                                     |
         | InvocableMethod     |                                     | 方法体加上该注解可以被process等调用 |
         | InvocableVariable   |                                     |                                     |
         | isTest              | SeeAllData:能否访问SF已经存在的数据 | 标注为测试类或者标记为测试方法      |
         | JsonAccess          |                                     |                                     |
         | namespaceAccessible |                                     |                                     |
         | ReadOnly            |                                     |                                     |
         | RemoteAction        |                                     |                                     |
         | SuppressWarnings    |                                     |                                     |
         | testSetup           |                                     |                                     |
         | TestVisible         |                                     |                                     |
         | RestResource        | urlMapping='/test/*'                | restApi的URL映射                    |
         | HttpDelete          |                                     |                                     |
         | HttpGet             |                                     | get请求                             |
         | HttpPatch           |                                     |                                     |
         | HttpPost            |                                     |                                     |
         | HttpPut             |                                     |                                     |
      
      11. Namespace
      
          命名空间,等同于包定义,用于精准定位,防止歧义
      
          1. System命名空间和Schema命名空间
             1. 系统默认命名空间可以直接默认省略使用
             2. 当自定义类名和默认命名空间内的类名重复的时候,此时调用该重复类的时候需要强制加上命名空间,因为发生了歧义需要精准定位
             3. System.debug()为System命名空间内的System类的方法,全称应该为System.System.debug()
          2. 命名空间,类名,变量名如果发生歧义的优先级顺序
             1. 变量.字段名>类名.静态方法名.字段名>命名空间.类名.静态方法名.字段名

9. lwc

   前端框架,类似于Vue

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

   2. 核心知识要点

      1. 官方连接

         ```
         //官方文档
         https://developer.salesforce.com/docs/component-library/documentation/en/lwc/
         
         //在线工具
         https://webcomponents.dev/
         
         //官方组件库
         https://developer.salesforce.com/docs/component-library/overview/components
         
         //样式设计
         https://www.lightningdesignsystem.com/
         ```

      2. 调试准备

         1. 本地组件预览功能

            ```shell
            #安装@salesforce/lwc-dev-server
            sfdx plugins:install @salesforce/lwc-dev-server
            
            #安装失败参考以下
            https://developer.salesforce.com/tools/vscode/en/localdev/set-up-lwc-local-dev
            https://developer.salesforce.com/forums/?id=9062I000000R0FqQAK
            ```

      3. js中import和export基础

         1. export

            ```js
            //情况1-----------------------------------------------------
            //直接导出变量和方法
            export let var1 = '属性1' 
            export function method1() { 
            　　// ... 
            }
            //情况2-----------------------------------------------------
            //先定义后导出
            let var1 = '属性1'
            let var2 = '属性2'
            let var3 = '属性3'
            function method1() {
                // ...
            }
            export {var1, var1, var1, method1}
            ```

         2. export default

            ```js
            //默认导出,有且仅有一个默认导出
            export default class 组件名 {
                // ...
            }
            ```

         3. import

            ```js
            //非default import-----------------------------------------------------
            //因为存在多个所以有大括号
            import {var1, var1, var1, method1} from '命名空间/模块名称'
            
            //default import-----------------------------------------------------
            //因为只有一个默认导出,不用大括号,且可以重命名
            import 组件名 from '命名空间/模块名称'
            ```

      4. 基本语法

         1. template 

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
               	<li key={it.value.name}>
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

      5. 生命周期及回调函数

         1. 组件创建时
            1. constructor()
               1. 必须首先调用父类super()构造器
               2. 无需返回值
               3. 不能属性赋值,其他生命周期可以赋值
         2. 植入Dom时
            1. connectedCallback()
               1. 父到子
         3. 浏览器渲染时
            1. renderedCallback()
               1. 子到父
         4. 错误发生时
            1. errorCallback(error: Error, stack: string)
         5. 移出Dom时
            1. disconnectedCallback
               1. 父到子
      
      6. 装饰器
      
         1. @api
            1. 全局监听,当监听值发生变化后画面相应值也会刷新
            
         2. @track
            1. 对对象或者数组变化深度监听
            
         3. @wire
            1. 与SF绑定数据
            
            2. 作用在方法上的时候@wire的取值将会成为方法的参数
            
               如下
            
               ```js
               @wire(getRecord, { recordId: '$recordId', fields: ['Account.Name'] })
               getAccount({ error, data }) {
                   if (data) {
                       this.record = data;
                       this.error = undefined;
                   } else if (error) {
                       this.error = error;
                       this.record = undefined;
                   }
               }
               ```
      
      7. 事件交互
      
         1. 创建和分发事件
      
      8. 数据库交互
      
         1. Base Lightning Components
      
            1. lightning-record-form
            2. lightning-record-edit-form
            3. lightning-record-view-form
      
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

10. VisualForce Modules

   1. @salesforce

      1. @salesforce/apex

         1. ```java
            refreshApex(valueProvisionedByWireService);
            getSObjectValue(sobject, fieldApiName);
            ```

      2. @salesforce/apexContinuation

      3. @salesforce/client/formFactor

      4. @salesforce/community

      5. @salesforce/contentAssetUrl

      6. @salesforce/i18n

      7. @salesforce/label

      8. @salesforce/messageChannel

      9. @salesforce/resourceUrl

      10. @salesforce/schema

      11. @salesforce/user

      12. @salesforce/userPermission

      13. @salesforce/customPermission

11. VisualForce和Aula和LWC的选择场景以及优缺点

   12. VisualForce

   13. Aula

   14. LWC

15. MeataData

    1. 获取
       1. 安装插件:https://marketplace.visualstudio.com/items?itemName=VignaeshRamA.sfdx-package-xml-generator
       2. **SFDX Package.xml Generator: Choose Metadata Components** 
       3. select the Metadata you need and click on **Update Package.xml** button.
       4. Package.xml file opens up with the selected metadata components.
       5. **SFDX: Retrieve Source in Manifest from Org**

16. 部署

    1. Production环境中创建SandBox环境
       1. ![image-20220430195332958](C:\Users\77023\Desktop\MD笔记\MD\Notes\SalesForce\image-20220430195332958.png)

    2. 登录SandBox环境
       1. UserName:Production用户名+.SandBox名称
       2. Password:

    3. 受信/送信
       1. 前提:受信方允许接收送信方的送信请求
       2. 送信方设定上传内容
          1. 创建一个送信set
          2. 添加送信内容
          3. 选定受信方
          4. 上传

       3. 受信方检验内容
       4. 受信方部署内容

