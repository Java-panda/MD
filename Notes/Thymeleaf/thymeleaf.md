- 标签规则

  - th:tag="值"

- 变量取值

  ```bash
  #获取变量值OGNL (Object-Graph Navigation Language)
  
  #直接获取一级对象
  ${key}
  #获取Map对象
  ${map.key}
  ${map['key']}
  #获取Bean对象
  ${obj.key}#等同于java中的obj.getKey()
  ${obj['key']}
  ```

  ```bash
  #获取国际化信息
  #key1=这是一句简单话
  #key2=我姓{0}名{1}
  
  #{key1}
  #{key2("刘","建")}
  ```

  ```
  添加URL
  @{/images/gtvglogo.png}
  ```

- Text

  - th:text
    - 文本类型
  - th:utext
    - 非文本类型html