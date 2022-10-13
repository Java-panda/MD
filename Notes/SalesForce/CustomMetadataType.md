- Metadate

  - 定义:描述数据的数据称其为元数据
  - 个人理解:从数据结构来看可以理解为数据本身的存储结构,表的结构,从编程角度来看可以理解为是一个Class,也就是对象的模板,而Record则等效于基于Class创建的实例对象

- Custom Metadata Type

  - 定义:定义元数据结构的对象

  - 个人理解:就是一个存放固定常量的小表格,从而避免硬编码实现灵活配置的目的

  - 优点:

    - 部署Custom Metadata Type会把Metadata和data(其实也是Metadata)全部部署,而Custom Object/Custom Settings则只会部署对象结构信息,也就是Metadata
    - 可打包,部署,升级
    - SOQL查询无限制

  - 应用场景

    - Mappings:数据映射关系
    - Business rules:业务规则
    - Master data:主数据,一些固定常量信息,配置信息
    - Protected information:保护信息,如某些应用程序Keys

  - 支持的字段类型 

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

  - 创建

    1. 创建一个Custom Metadata Type
       1. Set up/Custom Metadata Types
    2. 添加Custom Fields
    3. 添加Record

  - 可视范围

    - Public:公开可视
    - Protected:同一个命名空间下可视
    - PackageProtected:同一个命名空间并且同包下可视

  - 使用范围

    1. Apex

       1. select 标准字段,定制字段 from 类型__mdt [条件限制];

          ``` java
          List<Support_Tier1__mdt> s=[select MasterLabel,Default_Discount__c,Minimum_Spending__c from Support_Tier1__mdt where MasterLabel='Silver' and Default_Discount__c=10 and Minimum_Spending__c=1000];
              System.debug(s[0].MasterLabel);
              System.debug(s[0].Minimum_Spending__c);
              System.debug(s[0].Default_Discount__c);
              for(Support_Tier1__mdt v:[select MasterLabel,Default_Discount__c,Minimum_Spending__c from Support_Tier1__mdt]){
                  System.debug(v.MasterLabel);
                  System.debug(v.Minimum_Spending__c);
                  System.debug(v.Default_Discount__c);
              }
          ```

    2. Flows

       1. 使用Get Records组件查询出符合条件的Custom Metadata Type记录

    3. Default Value

       1. $CustomMetadata.类型__mdt.记录API名.字段(手写)

    4. Formula fields

       1. $CustomMetadata.类型__mdt.记录API名.字段(选择器)

    5. Process Builder

       1. $CustomMetadata.类型__mdt.记录API名.字段(在ProcessBuilder中使用公式进行条件判断时,可通过系统变量选择)

    6. Validation rules

       1. $CustomMetadata.类型__mdt.记录API名.字段(选择器)

  - 打包

    - 新建一个Package
    - 把Custom Metadata Type加入包中
    - 把Custom Metadata Type的Records加入包中
    - 上传Package
    - 将返回邮件中的安装URL给安装者

