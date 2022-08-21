### ServiceNow笔记

- ServiceNow是什么

  - ![ServiceNow is Platform of Platforms：企業の様々な部門（IT部門、営業部門、マーケティング部門、人事総務部門、経理財務部、顧客/代理店サービス部など）に個別に運用されている既存の情報システム（IT業務支援システム、IT運用システム、法務支援システム、人事総務システム、会計システム、営業支援システム、セキュリティ関係システム、資産管理システムなど）を統合し企業全体の業務の効率化、改善に繋げます。](https://www.mss.co.jp/product/img/servicenow/PlatformOfPlatforms.png)

  - 基于云计算的Saas服务平台,提供完整的应用程序开发,测试,部署,维护平台

  - 整合不同领域不同系统,提高工作流和生产效率

  - | ITサービスマネジメント       | ITSM   | ・インシデント管理 ・問題管理 ・変更管理 ・リリース管理 ・要求管理 ※これらの管理を効率化する自動化技術 |
    | :--------------------------- | ------ | ------------------------------------------------------------ |
    | ITオペレーションマネジメント | ITOM   | ・インフラ可視化 <br />・管理機器の自動監視 ・サービスマッピング管理 ・証明書管理 ・ファイアウォールポリシー追跡 ・外部データ標準化 ・構成管理 ・インフラ問題予測 |
    | セキュリティオペレーション   | SecOps | ・セキュリティインシデント管理 ・脆弱性管理 ・構成監査 ・脅威分析 |
    | ITビジネスマネジメント       | ITBM   | ・ITパフォーマンス分析 ・ビジネスデマンド管理 ・リソース管理 ・イノベーション管理 ・プロジェクト・ポートフォリオ管理 ・投資シナリオ管理 ・アジャイル開発支援 |
    | ヒューマンリソース           | HR     | ・従業員要求管理 ・文書標準化 ・従業員関係管理               |
  
- 专业术语

  - ITSM
  - ITOM
  - ITBM

- 基本概念

  - 版本
    - San Diego
    - Rome
    - Quebec
  - Application Scope
    - Scope
      - Scope Set(*x_27266_custom_rob*)
        - x
        - _<value from the glide.appcreator.company.code system property>_(2-5位)
        - <first_10_characters_of_app_name>(加上之前的位数之和最大为18)
    - Global

- App

  - Data:数据库
    - Table(表)
      - 默认字段
        - Created: 创建日期
        - Created by: 创建者
        - Sys ID: 系统自动生成ID
        - Updated: 更新日期
        - Updated by: 更新者
        - Updates: 被更新次数
      - 字段类型
        - 
  - Experience:界面
    - UI Builder:所见即所得的页面编辑器
      - Header
      - Page panel
      - Stage
      - Configuration panel
  - Logic and automation:逻辑自动化
    - Flow Designer
      - Flows:流(工作处理逻辑和过程)
        - Triggers:触发器(何时触发)
        - Actions:行为(触发后做什么)
  - Security
    - Roles:角色
      - Records:增删改查权限控制
      - Experiences:界面控制

- ServiceNow Scripting(ECMAScript5标准)

  - API

    - 分类
      - Client-side Scripting
        - **Client**: Client-side API for desktop apps
        - **Client Mobile**: ServiceNow Classic mobile application API. Not for ServiceNow Agent, Now Mobile, or ServiceNow Onboarding
        - **Now Experience UI Framework**: Agent Workspace component API
      - Server-side Scripting
        - **Server Scoped**: Scoped application API for server-side
        - **Server Global**: Global application API for server-side
      - Scripted REST APIs
        - **REST**: Restful APIs for interacting with a ServiceNow instance

  - 用途

    - Interact with database tables: query, update, create, delete
    - Validate data
    - Write to log files
    - Prompt users with alerts, confirmations, or messages
    - Launch workflows, scheduled jobs, and events
    - Interact with 3rd party web services

  - 要点

    - 开发
    - 测试
    - 调试

  - 应用

    - Client-side Scripting

      - 编码流程

        - Configure :决定何时执行
        - Coding:决定执行什么

      - 构成要素

        - Name: 脚本名
          Table: 作用数据表
          UI Type: 设备类型(Desktop/Tablet/Mobile/Service Portal/All)
          Type: 执行时点类型
          - onChange
          - onLoad
          - onSubmit
          - CellEdit
        - Field Name: 作用在哪个字段上(只有onChange类型会用到)
        - Active: 是否生效
        - Inherited: 是否继承(选中时脚本对该表及其子表均生效)
        - Global: 如果选中对所有视图生效
        - View: 作用于某个视图,如果省略不写则为默认视图

      - 执行时点类型

        - Loaded

          ```js
          function onLoad() {
          	//逻辑
          }
          ```

        - Changed

          ```js
          /**
          *control:对应字段的html标签
          *oldValue:旧值
          *newValue:新值
          *isLoading:是否处于画面加载中
          *isTemplate:是否是模板填充导致变化
          */
          function onChange(control, oldValue, newValue, isLoading, isTemplate) {
              if (isLoading || newValue === '') {
                  return;
              }
              //逻辑
          }
          ```

        - Submitted

          ```js
          function onSubmit() {
          	//逻辑
          }
          ```

        - CellEdit

    - Server-side Scripting

- 平台主页面

  - ALL
    - TableName.list:直接打开某个表的所有记录

